# Spring Cloud Config Repository

*A central configuration repository, used by
[Spring Cloud Config Server](https://github.com/ByteB00k/ByteBook-config-server).*

## Conventions

### Configuration File Naming Rules

- `application.yml` — Global settings for all applications.
- `application-{profile}.yml` — Global settings for a specific profile.
- `{application}.yml` — Base application settings.
- `{application}-{profile}.yml` — Settings for a specific application profile.

### Environments Naming Rules

- `dev` — Development environment
- `test` — Testing environment
- `stage` — Staging environment
- `prod` — Production environment

### YAML File Structure

1. Spring settings
2. Server settings
3. Application-specific settings
4. Logging and management

### Handling Sensitive Data

All sensitive data must be encrypted before being added to *.yml* files. Encrypted values should be prefixed
with `{cipher}`.

#### Encrypt a value

```bash
curl -X POST http://config-server:8882/encrypt -d "password"
```

#### Decrypt a value

```bash
curl -X POST http://config-server:8882/decrypt -d "65fa5ba164977153d5b9949ab51d8f52f2713e469ca28e9aa8bcd8b641e05ddf"
```

#### {cipher} prefix for encrypted values

```yaml
spring:
  datasource:
    password: '{cipher}65fa5ba164977153d5b9949ab51d8f52f2713e469ca28e9aa8bcd8b641e05ddf'
```

### Repository structure

```text
config-repo/
├── README.md
├── global/                      # Shared configurations
│   ├── application.yml              # Global settings
│   ├── application-dev.yml          # Global dev settings
│   ├── application-prod.yml         # Global prod settings
│   └── application-test.yml         # Global test settings
└── configuration/               # Application configurations
    ├── gateway/                    # API Gateway configurations
    │   ├── gateway.yml                 # Base settings
    │   ├── gateway-dev.yml             # Dev environment
    │   ├── gateway-prod.yml            # Production environment
    │   └── gateway-test.yml            # Test environment
    ├── user-service/               # User service configurations
    │   ├── user-service.yml
    │   ├── user-service-dev.yml
    │   ├── user-service-prod.yml
    │   └── user-service-test.yml
    └── ...
```

### Configuration Approval Flow

1. Create a branch `{application}/configuration-vX.X.X`

```bash
git checkout -b user-service/configuration-v0.2.1
```

2. Make changes and commit*

```bash
git commit -m "chore(config): update user-service configs"
```

3. Create a Pull Request (PR) to `main`

- PR Title: `{application}: updated configuration vX.X.X`
- Description:
  - What changed?
  - Links to related tasks

Example:

```text
Title: user-service: updated configuration v1.2.0

Changes:
- Added new DB connection pool settings
- Related: User Service: Add config server connection #2
```

5. Review and approval

- Reviewers: minimum 1 developer
- Verification checklist:
  - Syntax correctness
  - Compliance with standards
  - Security (no sensitive data exposed)

6. Merge to `main` and deploy

### Additional Links

*[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)