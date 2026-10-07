# Platformy Programowania — проєкт «Biblioteka»

Spring Boot 4.1 · Java 21 · Maven · Thymeleaf · Spring Data JPA · H2 / PostgreSQL.
Предметна область — бібліотека (`Book`, `CoverType`, `Category`), бо саме на ній побудовані приклади з силабусу.

Завдання до кожної лаби — у [LABS.md](LABS.md).

## Запуск у VS Code

1. `code ~/dev/platformy-programowania`. VS Code запропонує рекомендовані розширення, вони вже встановлені.
2. Зачекай, поки Java-розширення проіндексує проєкт (статус унизу ліворуч).
3. Запуск: **Run and Debug** (Ctrl+Shift+D) → `Library (dev, H2)` → F5.
   Або панель **Spring Boot Dashboard** (з'являється зліва) → ▶ біля `library`.
4. Відкрий http://localhost:8080

| Адреса | Що там |
|---|---|
| http://localhost:8080 | застосунок |
| http://localhost:8080/h2-console | консоль H2 (JDBC URL `jdbc:h2:mem:library`, user `sa`, без пароля) |
| http://localhost:8080/swagger-ui.html | Swagger UI (знадобиться в лабі 11) |
| http://localhost:8025 | Mailpit, листи з лаби 10 (лише профіль `postgres`) |

Завдання терміналу (Ctrl+Shift+P → *Tasks: Run Task*): `spring-boot:run`, `test`, `docker: postgres + mailpit`.

### Профілі

- `dev` (типовий): H2 у пам'яті, після рестарту база порожня.
- `postgres`: спершу `docker compose up -d`, потім конфігурація `Library (postgres)`.

### Чому Java 21

У системі за замовчуванням стоїть JDK 27. Lombok і Hibernate з найновішими JDK часто відстають, тому проєкт збирається на LTS 21.
`.vscode/settings.json` прописує `JAVA_HOME` на `/usr/lib/jvm/java-21-openjdk` для Java-розширення, Maven і вбудованого терміналу.
Поза VS Code: `JAVA_HOME=/usr/lib/jvm/java-21-openjdk ./mvnw spring-boot:run`.

## IntelliJ Ultimate → VS Code

Силабус розрахований на IntelliJ IDEA Ultimate. Те саме у VS Code:

| IntelliJ | VS Code |
|---|---|
| New Project → Spring Boot | Ctrl+Shift+P → *Spring Initializr: Create a Maven Project* |
| Generate (Alt+Insert) → Getters/Setters, Constructor | ПКМ → *Source Action…* (або Lombok `@Getter @Setter`) |
| Run/Debug Spring Boot | F5 або Spring Boot Dashboard |
| Database tool window | H2 console або розширення *SQLTools* |
| Endpoints tool window | Spring Boot Dashboard → *Endpoint Mappings* |
| Шорткати для `@Autowired`, бінів | Ctrl+Click по біну, `@` в Outline; Spring Boot Tools показує біни |
| Live templates | Snippets, напр. `@GetMapping` → Tab |

На захисті проєкту викладач може попросити показати щось в IntelliJ. Варто хоча б раз відкрити цей проєкт там (`File → Open → pom.xml`), він відкривається без змін.

## Що вже є

- `pom.xml`: Web MVC, Thymeleaf, Data JPA, Data REST, Validation, Mail, Lombok, DevTools, H2, PostgreSQL, springdoc (Swagger).
- **Spring Security закоментований** у `pom.xml`. Його вмикаєш сам у лабі 8, як того вимагає завдання з силабусу.
- `HomeController` + `templates/index.html`: мінімальна сторінка, щоб перевірити, що все запускається.
- `compose.yaml`: PostgreSQL 17 + Mailpit.

Решту пишеш сам: на захисті треба буде змінювати код наживо.
