# spendwise-config

Externalized configuration for every SpendWise service, served by `spendwise-config-server`
(Spring Cloud Config, Git backend).

## How files are picked

A service named `transaction-service` running with profile `prod` receives, highest priority first:

1. `transaction-service-prod.yml`
2. `transaction-service.yml`
3. `application-prod.yml`
4. `application.yml` (shared by every service)

The Git branch or tag is the config server's *label*. `main` is the default; a service can pin a
tag with `spring.cloud.config.label`.

## Rules

- No secrets in this repo. Passwords, client secrets and keys come from Vault or environment
  variables (`${DB_PASSWORD}`, `${TRANSACTION_SERVICE_CLIENT_SECRET}`); defaults here are local-dev only.
- Every change goes through a pull request; the `validate` workflow checks that each file parses.
- Running services pick up a change on restart, or on `POST /actuator/refresh` for `@RefreshScope` beans.
