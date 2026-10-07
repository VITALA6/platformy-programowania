# Лаби — план робіт

Кожна лаба продовжує попередню в цьому ж проєкті. Після кожної роби коміт `lab N: ...`, щоб на захисті можна було показати історію.
Пункти **Теорія** — це те, що питатимуть на колоквіумі (50 балів, поріг 26).

Оцінювання лаб: регулярні заняття 35 балів (мін. 16) + захист індивідуального проєкту 15 балів (мін. 11).

---

## Lab 1 — Spring Boot, Spring MVC, Thymeleaf. Перший MVC-проєкт

- [ ] Запусти проєкт (README), відкрий `/`.
- [ ] Додай сторінку `/about` з окремим контролером і шаблоном.
- [ ] Передай у шаблон дані через `Model` (наприклад, поточну дату) і виведи їх через `th:text`.
- [ ] Винеси спільні шматки (header/footer) у fragment: `th:fragment`, `th:replace`.

**Теорія:** що таке MVC; як `DispatcherServlet` обробляє запит крок за кроком (HandlerMapping → Controller → ViewResolver → View); `@Controller` і `@RestController`.

## Lab 2 — Java Beans і вирази Thymeleaf

- [ ] Створи POJO `Book` (поки без JPA): `title`, `author`, `isbn`, `year`, `price`. Getters/setters згенеруй через *Source Action*.
- [ ] Контролер віддає список книжок (поки зі статичного `List`), шаблон виводить таблицю через `th:each`.
- [ ] Спробуй `${...}`, `*{...}`, `@{...}`, `#{...}`, `th:if`/`th:unless`, `th:object`, утиліти `#numbers`, `#temporals`, `#strings`.
- [ ] Змінні контексту: `${param.x}`, `${session.x}`.

**Теорія:** що таке JavaBean (конвенції); чим відрізняються типи виразів Thymeleaf.

## Lab 3 — Розширене мапування запитів. CRUD і прості форми

- [ ] `@RequestMapping` на класі + `@GetMapping`/`@PostMapping` на методах.
- [ ] `@PathVariable` (`/books/{id}`), `@RequestParam` (пошук `?q=`).
- [ ] Форма додавання/редагування: `th:object`, `th:field`, `@ModelAttribute`.
- [ ] Видалення книжки. Після POST зроби `redirect:` (патерн PRG) і поясни, навіщо він.
- [ ] Повідомлення «збережено» через `RedirectAttributes.addFlashAttribute`.

**Теорія:** як Spring Boot мапить HTTP на методи (`@GetMapping`, `@PostMapping`, …); PRG.

## Lab 4 — Форматування, валідація, власне зв'язування даних

- [ ] Bean Validation на `Book`: `@NotBlank`, `@Size`, `@Min`, `@Past`, `@Pattern` (ISBN).
- [ ] `@Valid` + `BindingResult` у контролері; помилки у формі через `th:errors`, `#fields.hasErrors`.
- [ ] `@DateTimeFormat`, `@NumberFormat`.
- [ ] Власний `Formatter<T>` або `PropertyEditor` (наприклад, для `CoverType` із select) і реєстрація через `WebMvcConfigurer.addFormatters`.
- [ ] Власна анотація-валідатор (`ConstraintValidator`), напр. `@ValidIsbn`.
- [ ] Повідомлення про помилки в `messages.properties`.

**Теорія:** чим binding error відрізняється від validation error; хто й коли викликає валідатор.

## Lab 5 — Spring Data JPA і DI

- [ ] Перетвори `Book` на `@Entity` (`@Id`, `@GeneratedValue`, `@Column`).
- [ ] `BookRepository extends JpaRepository<Book, Long>`; заміни статичний список на репозиторій.
- [ ] Encja `CoverType` + `@ManyToOne` з `Book`.
- [ ] Початкові дані: `data.sql` або `CommandLineRunner`-бін.
- [ ] Вприскування: через конструктор, `@Autowired` на полі, `@Bean` у класі `@Configuration`. Порівняй, чому конструктор кращий.
- [ ] Подивись таблиці в H2 console і SQL у логах.

**Теорія:** що таке encja; її життєвий цикл (transient → managed → detached → removed, діаграма станів); ORM; DAO і Repository.

## Lab 6 — Many-to-Many, Lombok, перші власні запити

- [ ] Lombok на сутностях: `@Getter @Setter @NoArgsConstructor`… Поясни, чому `@Data` на `@Entity` небезпечний (`equals`/`hashCode`/`toString` + lazy-зв'язки).
- [ ] Encja `Category`, `@ManyToMany` з `Book` (`@JoinTable`), вибір категорій у формі (multi-select / checkbox).
- [ ] Derived queries: `findByTitleContainingIgnoreCase`, `findByCategoriesName`, `countBy…`.

**Теорія:** власник зв'язку, `mappedBy`; `FetchType.LAZY` і `EAGER`.

## Lab 7 — Розширені запити

- [ ] `@NamedQuery` на сутності.
- [ ] `@Query` з JPQL і з `nativeQuery = true`; іменовані параметри `:name` + `@Param`.
- [ ] SpEL у `@Query` (`:#{#book.title}`, `#{#entityName}`).
- [ ] Метод репозиторію, що повертає `Stream<Book>` (потрібна транзакція!).
- [ ] `@EntityGraph`: розв'яжи проблему N+1 для списку книжок з категоріями (подивись SQL у логах до і після).
- [ ] Criteria API або `Specification` для фільтра з кількома необов'язковими полями.
- [ ] Open Session In View: вимкни `spring.jpa.open-in-view` і подивись, що зламається.

**Теорія:** N+1; рівні кешу Hibernate (1st level, 2nd level, query cache); OSIV — переваги й недоліки.

## Lab 8 — Spring Security

- [ ] Розкоментуй 3 залежності «Lab 8» у `pom.xml`, запусти, увійди з паролем із консолі.
- [ ] `SecurityFilterChain`-бін: публічні й захищені шляхи, ролі `USER`/`ADMIN`.
- [ ] Власна форма логіну (`.formLogin(f -> f.loginPage("/login"))`) і logout.
- [ ] Сторінка 403 (`accessDeniedPage`).
- [ ] Користувачі: спершу `InMemoryUserDetailsManager`, потім з бази (`UserDetailsService` + сутність `User`), `BCryptPasswordEncoder`.
- [ ] Thymeleaf: `sec:authorize`, `sec:authentication`.
- [ ] Аудит: слухач `AuthenticationSuccessEvent` / `AbstractAuthenticationFailureEvent`, що логує входи.
- [ ] Не забудь про H2 console (CSRF, frames) і `/swagger-ui/**`.

**Теорія:** filter chain; автентифікація і авторизація; як проходить логін (AuthenticationManager → Provider → UserDetailsService).

## Lab 9 — Шар сервісів, профілі, транзакції

- [ ] `BookService` (`@Service`): контролер → сервіс → репозиторій. Логіка не повинна жити в контролері.
- [ ] Сервіс з інформацією про поточного користувача (`SecurityContextHolder`).
- [ ] `@Transactional` (і `readOnly = true`). Перевір відкат при `RuntimeException` і при checked exception.
- [ ] Профілі: уже є `dev` і `postgres`. Додай бін, що існує лише в одному профілі (`@Profile`).
- [ ] Обробка винятків: `@ExceptionHandler`, `@ControllerAdvice`, власний `BookNotFoundException` → сторінка 404.

**Теорія:** `@Component`, `@Service`, `@Repository`; propagation; чому `@Transactional` не спрацьовує при виклику з того ж класу (проксі).

## Lab 10 — Приклади сервісів: i18n, пошта, файли

- [ ] i18n: `messages_pl.properties`, `messages_en.properties`, `LocaleResolver` + `LocaleChangeInterceptor` (`?lang=en`).
- [ ] Пошта: `JavaMailSender`, лист-шаблон Thymeleaf (запусти Mailpit через `docker compose up -d`, профіль `postgres`).
- [ ] Завантаження файлів: `MultipartFile` → обкладинка книжки, збереження на диск і в БД (`@Lob`); віддача картинки назад.
- [ ] Обмеження розміру (`spring.servlet.multipart.max-file-size`).

## Lab 11 — REST API

- [ ] `@RestController` `/api/books`: GET (список, один), POST, PUT, DELETE.
- [ ] Правильні статуси: `ResponseEntity`, `201 Created` + `Location`, `204`, `404`.
- [ ] DTO (`record BookDto`) + мапер; сутності назовні не віддавати. Поясни навіщо.
- [ ] `@Valid` на `@RequestBody`; помилки у форматі `ProblemDetail` через `@RestControllerAdvice`.
- [ ] Типи медіа: `produces`/`consumes`, `Accept: application/xml` (потрібна `jackson-dataformat-xml`).
- [ ] Swagger: http://localhost:8080/swagger-ui.html, анотації `@Operation`, `@Tag`.
- [ ] Порівняй зі Spring Data REST (`@RepositoryRestResource`), він уже є в залежностях.
- [ ] Security для API: окремий `SecurityFilterChain` для `/api/**` (HTTP Basic, без CSRF).

**Теорія:** принципи REST; ідемпотентність методів; формати повідомлень; обробка винятків у REST.

## Lab 12 — Захист індивідуального проєкту

- [ ] Перевір чек-лист: CRUD, валідація, ≥ 1 ManyToOne + ≥ 1 ManyToMany, власні запити, Security з ролями, сервісний шар + транзакції, REST + Swagger.
- [ ] Будь готовий наживо: додати поле до сутності (з валідацією і у формі), новий endpoint, нове правило доступу.
- [ ] Можеш пояснити кожен рядок у проєкті.

---

## Колоквіум: питання з силабусу

- Що таке encja? Цикл життя encji в JPA (діаграма станів).
- Рівні кешування даних у Hibernate.
- Опиши крок за кроком, як Spring MVC обробляє HTTP-запит.
- Як Spring Boot спрощує мапування запитів (`@GetMapping`, `@PostMapping`…)?
- Що таке IoC-контейнер? Його переваги й недоліки.
- Види вприскування залежностей у Spring з прикладами.
- Трирівнева архітектура вебзастосунку; способи доступу до даних у Spring (JDBC, JTA, Hibernate/JPA, Spring Data).
- Jakarta EE: історія, модулі. Spring MVC і WebFlux.
