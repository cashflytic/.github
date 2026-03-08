## Description

<!-- Briefly describe what this PR does and why. Link the relevant issue: Closes #N -->

---

## Checklist

- [ ] Commit title follows conventional commit format with a valid scope (`domain`, `application`, `infrastructure`, `api`, `db`, `docker`, `deps`, `ci`, `release`)
- [ ] Unit tests added or updated; **line coverage ≥ 98%** (`./mvnw test`)
- [ ] Architecture tests pass — domain has no Spring or infrastructure dependencies (`./mvnw test -Dtest=ArchitectureTest`)
- [ ] Integration tests pass — requires Docker to be running (`./mvnw verify`)
- [ ] Domain layer has no Spring Framework dependencies and no infrastructure imports
- [ ] No secrets, credentials, or `.env` values committed

---

## Testing notes

<!-- Describe how you tested this change. Include any edge cases considered. -->
