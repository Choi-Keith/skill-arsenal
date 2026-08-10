# Project Coding Guidelines: Go + DDD (Domain-Driven Design)

This document defines the code organization, directory structure, and naming constraints for this project or any Go service that adopts this standard.
**When generating, modifying, or refactoring any Go code, you must follow these rules strictly.** If a rule conflicts with a specific scenario, prioritize the four core principles: domain layer dependency-free, dependency inversion, no package cycles, and single responsibility.

---

## 0. Core Principles (Highest Priority)

1. **Domain first, infrastructure later**: The `domain` layer must not depend on any database, web framework, ORM, or third-party library.
2. **Dependency inversion**: Higher layers (`application` / `adapters`) depend on interfaces defined by `domain`; concrete implementations are injected at runtime.
   The dependency direction must be `infra/adapter → domain`; never the opposite.
3. **Minimal packages and single responsibility**: A package should focus on one responsibility set (e.g. a single aggregate such as `order` or `user`); avoid “god packages.”
4. **Use `internal` to isolate implementation details**: Implementation details that must not be imported by external modules belong in `internal/`.
   Only truly reusable components should go into `pkg/`, and `pkg/` should be used sparingly.
5. **Avoid cyclic dependencies**: Decouple with interfaces and constructor injection; place shared types in `pkg/` (use with caution) or a dedicated `shared/` package (`shared` should be rare).

---

## 1. Directory Structure (Required)

Place new code using the following structure; do not add new top-level directories without approval.

```text
myapp/
├── cmd/
│   └── myapp/            # application entry point main.go (multiple cmds allowed)
│       └── main.go
├── configs/              # configuration files, templates
├── internal/
│   ├── domain/           # domain layer: entities, value objects, domain services, repository interfaces (interfaces only, no implementations)
│   │   ├── user/
│   │   │   ├── entity.go
│   │   │   ├── value_objects.go
│   │   │   └── repository.go   # repository interface
│   │   └── order/
│   │       └── ...
│   ├── application/      # application layer: use case orchestration, transaction boundaries, DTOs, application services
│   │   └── usercase/
│   │       ├── service.go
│   │       └── dto.go
│   ├── adapter/          # adapters: input/output (HTTP handlers, gRPC, message consumers)
│   │   ├── http/
│   │   │   └── user_handler.go
│   │   └── grpc/
│   └── infra/            # infrastructure: DB, cache, email, external system implementations
│       ├── db/
│       │   └── postgres_user_repo.go
│       └── logger/
├── api/                  # OpenAPI / Proto / interface definitions
├── pkg/                  # packages reusable by external projects (use sparingly)
├── scripts/
├── deployments/
└── go.mod
```

**Placement rules:**
- Business code belongs in `internal/` so it cannot be imported externally.
- `domain` contains only domain concepts and repository interfaces; it must not contain any ORM, SQL, HTTP, or technology-specific details.
- Implementations for repositories, handlers, message bus consumers, etc. belong in `adapter/` or `infra/`.

---

## 2. Naming Guidelines (Must Follow)

| Item | Rule |
| --- | --- |
| Package names | Short, singular, lowercase (e.g. `user`, `order`, `payment`); no underscores or camelCase |
| Package name anti-patterns | Do not use generic package names like `models` or `common` under `domain` (unclear responsibility, easy to cause cyclic dependencies) |
| File names | Split by responsibility: `entity.go`, `repository.go`, `service.go`, `handler_http.go`; avoid overly large single files |
| Type names | Exported types are PascalCase (`User`, `Order`); unexported types are lower-case |
| Interface names | Use behavior suffixes (`UserRepository`, `PaymentGateway`); no `IUser` prefixes |
| Variables / functions | camelCase for unexported; exported functions start with uppercase |

**About `user.User`**: Package name + type name (`user.User`) is normal and idiomatic in Go; it clarifies type ownership.
If it feels verbose, alias the import with `u "myapp/internal/domain/user"` and use `u.User`, but do not rename packages to `models` or `entities`.

---

## 3. Domain Layer (`internal/domain/...`)

**Characteristics: clean, implementation-free, zero third-party dependencies.**

- Define aggregate roots, entities, value objects, domain services, and **repository interfaces** (contracts).
- Domain behavior (invariant validation and state changes) belongs on entity/domain service methods, for example:

```go
package user

import "time"

// User is the aggregate root.
type User struct {
    ID        string
    Email     string
    Password  string // hashed password
    CreatedAt time.Time
}

func NewUser(id, email, passwordHash string) *User {
    return &User{ID: id, Email: email, Password: passwordHash, CreatedAt: time.Now()}
}

// ChangeEmail ensures the domain invariant here.
func (u *User) ChangeEmail(new string) {
    u.Email = new
}
```

- Repository interfaces define persistence contracts but **do not include implementations**:

```go
package user

// UserRepository defines the persistence contract.
type UserRepository interface {
    Save(u *User) error
    FindByID(id string) (*User, error)
    FindByEmail(email string) (*User, error)
}
```

---

## 4. Application Layer (`internal/application/...`)

**Responsibility: orchestrate use cases, manage transaction boundaries, call domain interfaces, and map DTOs. Keep it as thin as possible.**

```go
package usercase

import (
    "errors"
    "myapp/internal/domain/user"
)

type UserService struct {
    repo user.UserRepository
}

func NewUserService(r user.UserRepository) *UserService { return &UserService{repo: r} }

func (s *UserService) Register(email, passwordHash string) (*user.User, error) {
    if existing, _ := s.repo.FindByEmail(email); existing != nil {
        return nil, errors.New("email exists")
    }
    u := user.NewUser(generateID(), email, passwordHash)
    if err := s.repo.Save(u); err != nil {
        return nil, err
    }
    return u, nil
}
```

- Transaction boundaries can be opened and committed inside application service methods, with the transaction object passed through repository implementations.
- Do not implement domain business validation logic in the application layer; that belongs in the domain layer.

---

## 5. Adapter / Infrastructure Layer (`internal/adapter`, `internal/infra`)

**Responsibility: connect domain interfaces to concrete technical implementations (ORM, HTTP, message queue).**

```go
package db

import (
    "database/sql"
    "myapp/internal/domain/user"
)

type PostgresUserRepo struct{ db *sql.DB }

func NewPostgresUserRepo(db *sql.DB) *PostgresUserRepo { return &PostgresUserRepo{db: db} }

func (r *PostgresUserRepo) Save(u *user.User) error        { /* SQL operations */ return nil }
func (r *PostgresUserRepo) FindByID(id string) (*user.User, error)   { return nil, nil }
func (r *PostgresUserRepo) FindByEmail(email string) (*user.User, error) { return nil, nil }
```

- Dependency injection and composition should be centralized in `cmd/myapp/main.go`:

```go
func main() {
    dbConn := mustOpenDB()
    userRepo := db.NewPostgresUserRepo(dbConn)
    userSvc := usercase.NewUserService(userRepo)
    // wire userSvc into HTTP handlers
}
```

---

## 6. Transaction and Consistency Boundaries

- **Short-lived transactions**: open and commit them in the application layer; pass transaction objects (such as `*sql.Tx`) through repository implementations.
- **Cross-aggregate consistency**: prefer eventual consistency using domain events or message queues rather than distributed transactions (two-phase commit), unless the business requires it.

---

## 7. Error Handling

- At boundaries (infra/adapter exits), wrap errors with `%w` or `errors.Wrap` to preserve context and make debugging easier; do not swallow errors.
- The domain layer should return semantic errors; the HTTP layer should convert them into normalized responses/status codes.

---

## 8. Testing Requirements

- **Domain unit tests**: cover `internal/domain/*` behavior and invariants without depending on a database.
- **Integration tests**: replace `infra` with test doubles or use a test database (testcontainers/docker-compose).
- **End-to-end tests**: run E2E in CI against real containerized dependencies (DB/MQ).
- Run integration tests with `-race` to detect race conditions.

---

## 9. Anti-Patterns (Prohibited)

- ❌ Putting ORM/SQL/HTTP code inside `domain` (violates dependency inversion).
- ❌ Large, catch-all `models` packages (unclear responsibility, easy cyclic dependencies).
- ❌ Global state or singleton database variables (bad for testing and concurrency).
- ❌ Overusing `pkg/shared` (creates hidden coupling; prefer explicit interface injection).
- ❌ Ignoring error wrapping (preserve error context at boundaries).

---

## 10. Code Style and Tooling

- Before committing, run `gofmt` / `goimports` and `golangci-lint` with `govet` and `staticcheck` enabled.
- CI must run `go test ./...` continuously.
- Manage dependencies with `go mod`; avoid overusing `vendor`.

---

## 11. Pre-Deployment Checklist (Self Review After Each Change)

- [ ] The domain layer has no third-party dependencies.
- [ ] Repository interfaces are in `domain`; concrete implementations are in `infra`/`adapter`.
- [ ] Implementation details are isolated with `internal`; reusable components are only in `pkg`.
- [ ] Package names are lowercase, singular, and single-responsibility.
- [ ] `cmd/` handles dependency injection and startup.
- [ ] CI covers `gofmt`, `golangci-lint`, and `go test`.
- [ ] No cyclic dependencies and no global singleton DB.

---

