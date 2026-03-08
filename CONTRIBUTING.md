# Contributing to Cashflytic

Thank you for contributing! This guide covers the conventions and requirements across all cashflytic repositories.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Commit Convention](#commit-convention)
- [Branch Strategy](#branch-strategy)
- [Local Development Setup](#local-development-setup)
- [Architecture Rules](#architecture-rules)
- [Test Requirements](#test-requirements)
- [Pull Request Process](#pull-request-process)

---

## Code of Conduct

Be respectful and constructive. We're building fintech tooling — correctness and clarity matter more than cleverness.

---

## Commit Convention

All commits **must** follow [Conventional Commits](https://www.conventionalcommits.org/). This is enforced by a Git commit-msg hook.

### Format

```
<type>(<scope>): <short description>

[optional body]

[optional footer]
```

### Allowed types

| Type | When to use |
|------|-------------|
| `feat` | New feature |
| `fix` | Bug fix |
| `chore` | Maintenance, tooling, config |
| `docs` | Documentation only |
| `test` | Adding or updating tests |
| `refactor` | Code change without behaviour change |
| `perf` | Performance improvement |

### Allowed scopes

| Scope | Area |
|-------|------|
| `domain` | Core domain model and business logic |
| `application` | Application services and use cases |
| `infrastructure` | Adapters, repositories, external integrations |
| `api` | REST controllers, DTOs, OpenAPI spec |
| `db` | Database migrations and schema |
| `docker` | Docker / Compose configuration |
| `deps` | Dependency upgrades |
| `ci` | CI/CD pipeline changes |
| `release` | Release preparation |

### Examples

```
feat(domain): add expense categorisation rule engine
fix(infrastructure): handle Mistral OCR timeout on large receipts
test(application): add coverage for split-expense use case
chore(deps): bump Spring Boot to 3.4.2
```

---

## Branch Strategy

- Base all feature branches off `develop`
- Name branches: `<type>/<short-description>` (e.g. `feat/receipt-ocr-retry`)
- Open PRs targeting `develop`
- `main` receives merges from `develop` only via release PRs

---

## Local Development Setup

### Prerequisites

- Java 21+
- Maven (or use `./mvnw`)
- Docker Desktop (required for integration tests and local services)

### Environment variables

Create a `.env` file (never commit it) or export the following:

```bash
MISTRAL_API=<your-mistral-api-key>
UBER_CLIENT_ID=<your-uber-oauth-client-id>
UBER_CLIENT_SECRET=<your-uber-oauth-client-secret>
```

### Start dependencies

```bash
docker compose up -d
```

This starts PostgreSQL and any other local service dependencies.

### Run the backend

```bash
./mvnw spring-boot:run
```

---

## Architecture Rules

The backend follows **hexagonal architecture** (ports & adapters). These rules are enforced at build time by [ArchUnit](https://www.archunit.org/):

- **Domain layer** must have **no Spring Framework dependencies** — no `@Component`, `@Service`, `@Repository`, no Spring imports at all
- **Domain layer** must have **no infrastructure dependencies** — it cannot reference adapters, JPA entities, or external libraries
- **Application layer** depends only on domain interfaces (ports)
- **Infrastructure layer** implements ports and may reference Spring, JPA, external SDKs
- **API layer** depends on the application layer; it must not reach into domain internals directly

ArchUnit tests live in `src/test/java/.../architecture/ArchitectureTest.java`. Run them explicitly with:

```bash
./mvnw test -Dtest=ArchitectureTest
```

Do not suppress or work around ArchUnit failures — fix the design.

---

## Test Requirements

### Unit tests

- Use **JUnit 5** and **Mockito**
- Test all domain logic and application use cases in isolation
- Aim for meaningful assertions, not just coverage numbers

### Coverage threshold

**JaCoCo enforces ≥ 98% line coverage** — the build will fail if coverage drops below this threshold.

```bash
./mvnw test   # runs unit tests + JaCoCo report
```

Do not exclude classes from JaCoCo to game the metric.

### Integration tests

- Use **Testcontainers** — Docker must be running locally
- Integration tests run during the `verify` phase:

```bash
./mvnw verify   # runs all tests including integration tests
```

Integration tests are the source of truth for database interactions and external adapter behaviour.

---

## Pull Request Process

1. Ensure all CI checks pass before requesting review
2. Link to the relevant issue in the PR description
3. Request review from at least one team member
4. Address all review comments before merging
5. Squash or rebase commits to keep history clean; the merge commit title must follow the conventional commit format
