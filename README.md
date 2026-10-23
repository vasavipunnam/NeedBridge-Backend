# NeedBridge Backend

NeedBridge is a Spring Boot + MySQL backend for a community donation platform. Verified organizations publish real needs, donors offer items, the system analyzes and scores matches, volunteers accept pickups, QR/verification codes support handoff, and organizations confirm delivery.

## Technology
- Java 21
- Spring Boot 3.3.5
- Spring Security + JWT
- Spring Data JPA
- MySQL 8
- Bean Validation
- Swagger/OpenAPI
- ZXing QR generation
- JUnit 5
- Docker / Docker Compose

## Quick start with Docker
```bash
docker compose up --build
```
Open Swagger UI: `http://localhost:8080/swagger-ui.html`

## Run locally
1. Start MySQL and create database `needbridge` (or allow the JDBC URL to create it).
2. Set `MYSQL_USER` and `MYSQL_PASSWORD` if different from `root/root`.
3. Run:
```bash
mvn spring-boot:run
```

## Demo users
| Role | Email | Password |
|---|---|---|
| Donor | priya@needbridge.demo | Test@123 |
| Organization | ravi@hope.demo | Test@123 |
| Volunteer | arjun@needbridge.demo | Test@123 |
| Admin | admin@needbridge.demo | Admin@123 |

The demo seed also creates **Hope Community School** and a need for **20 School Backpacks**.

## End-to-end demo
1. Login as Priya (`POST /api/auth/login`).
2. Create a donation (`POST /api/donations`) for 5 School Backpacks.
3. Analyze it (`POST /api/donations/{id}/analyze`).
4. Generate matches (`POST /api/donations/{id}/matches`).
5. Accept the Hope Community School match (`POST /api/donations/matches/{matchId}/accept`).
6. Request pickup (`POST /api/pickups`).
7. Login as Arjun and view `/api/pickups/available`, then accept the pickup.
8. Use the pickup verification code (or QR endpoint) to collect it.
9. Login as Ravi and confirm delivery with the same code.
10. Check `/api/needs/mine` — received becomes 5 and remaining becomes 15.

## AI-ready design
The project uses `DonationIntelligenceService`. The default implementation is `RuleBasedDonationIntelligenceService`, which makes the demo fully runnable without an external AI account. Replace that implementation with a vision/LLM adapter later; the rest of the workflow does not change.

## Important production notes
- Replace the demo JWT secret and passwords.
- Restrict registration/role assignment.
- Use Flyway or Liquibase instead of `ddl-auto=update`.
- Store images in object storage rather than raw URLs supplied by clients.
- Add rate limiting, secrets management, audit retention, malware scanning for uploads, and a real notification provider.
- Do not treat automated condition assessment as a safety guarantee; require human confirmation for donated goods.
