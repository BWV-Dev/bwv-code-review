# PHP Coding Rules & Security BWV

These rules assume **PHP 8.5** (Laravel 13 / CakePHP 5.4).

## Table of Contents
- [1. Naming](#1-naming)
- [2. Styling & Types](#2-styling--types)
- [3. Comment](#3-comment)
- [4. Usage & Code Quality](#4-usage--code-quality)
- [5. Security](#5-security)
- [6. Clean Code & Architecture Principles](#6-clean-code--architecture-principles)

---

## 1. Naming

### [PHP-NAMING-001] Files, Namespaces, Classes, Interfaces, Enums and Traits
- **Severity**: REQUIRED
- **Description**: Use PascalCase (UpperCamelCase) for class files, namespaces, classes, interfaces, enums, enum cases and traits. The name should be a noun.
- **Examples**:
  ```php
  // UserController.php
  namespace App\Http\Controllers;

  class UserController
  {
      // ...
  }

  interface PaymentRule
  {
      // ...
  }

  enum UserType: string
  {
      case Admin = 'admin';
      case Support = 'support';
  }

  trait CommonTrait
  {
      // ...
  }
  ```

### [PHP-NAMING-002] Functions, Properties and Variables
- **Severity**: REQUIRED
- **Description**: Use camelCase for functions, methods, properties and variables. Function and method names should start with a verb.
- **Examples**:
  ```php
  // Bad
  function user_name() {}
  $route_name = 'abc';

  // Good 👍
  function getUserName(): string {}
  function redirectTo(Request $request): ?string {}

  $routeName = 'abc';
  ```

### [PHP-NAMING-003] Constants
- **Severity**: REQUIRED
- **Description**: Use `UPPER_CASE_UNDERSCORE` for class constants and global constants.
- **Examples**:
  ```php
  const TABLE_NAME = 'users';

  class Invoice
  {
      public const int MAX_RETRY_COUNT = 3;
  }
  ```

### [PHP-NAMING-004] Meaningful Variable Names
- **Severity**: REQUIRED
- **Description**: Use meaningful names. Do not use unclear abbreviations, single letters (except loop indexes) or data-type prefixes.
- **Examples**:
  ```php
  // Bad
  $fn = 'John';
  $a = 20;
  $strName = 'John';   // type prefix is redundant
  $arrAnimals = [];

  // Good 👍
  $firstName = 'John';
  $age = 20;
  $animals = [];
  ```

### [PHP-NAMING-005] Boolean Variables
- **Severity**: RECOMMENDED
- **Description**: Boolean names should read clearly as a predicate, state, capability or intention. Prefer `is`, `has`, `can`, `should` where they improve clarity; descriptive adjectives such as `enabled`, `visible`, `active` are acceptable.
- **Examples**:
  ```php
  $isConnected = true;
  $hasPermission = true;
  $canResize = false;
  $shouldConfirm = true;
  $enabled = true;
  ```

## 2. Styling & Types

### [PHP-STYLE-002] Class Layout
- **Severity**: REQUIRED
- **Description**: Order the elements in a class: `use` trait → Enum cases → Constants (public → protected → private) → Properties (public → protected → private) → Constructor → Destructor → Magic methods → PHPUnit methods (`setUp`, `tearDown`, …) → Methods (public → protected → private).
- **Examples**:
  ```php
  final class InvoiceService implements Stringable
  {
      use LoggableTrait;

      public const int MAX_RETRY_COUNT = 3;

      private const string CACHE_KEY = 'invoice';

      public int $version = 1;

      private int $retryCount = 0;

      public function __construct(
          private readonly InvoiceRepository $repository,
      ) {}

      public function __toString(): string {
          return self::CACHE_KEY;
      }

      public function issue(int $invoiceNo): Invoice {
          // ...
      }

      protected function buildLines(Invoice $invoice): InvoiceLines {
          // ...
      }

      private function normalize(InvoiceLines $lines): InvoiceLines {
          // ...
      }
  }
  ```

### [PHP-STYLE-003] Maximum Line Length
- **Severity**: RECOMMENDED
- **Description**: Prefer a maximum line length of 80 characters. Wrap a longer line after a comma and before an operator. Do not split a long string, URL or fully qualified class name just to fit.
- **Examples**:
  ```php
  if (
      ($condition1 === true || $condition2 > 0)
      && $condition3 === false // Break before an operator
      && $condition4 === 1
  ) {
      callSomething(
          $longNameParam, // Break after a comma
          $otherLongNameParam,
      );
  }
  ```

### [PHP-STYLE-004] Strict Types and Type Declarations
- **Severity**: REQUIRED
- **Description**:
  - Add `declare(strict_types=1);` to every PHP file except view templates.
  - Type every parameter, return value, property and class constant.
  - Narrow `mixed` values (request input, config, JSON, database rows) once, at the boundary, with `is_*()`, `instanceof` or a typed accessor (`$request->integer()` in Laravel, `toInt()` in CakePHP).
  - Missing types are also reported by PHPStan. In review, focus on `mixed` values that are used without being narrowed — PHPStan does not check that.
- **Examples**:
  ```php
  // Bad — no types at all
  function calculateDiscount($price, $discount) {
      return $price * ($discount / 100);
  }

  // Bad — a mixed value reaches the service without being narrowed
  $limit = $request->input('limit');
  $users = $this->userService->paginate($limit);
  ```
  ```php
  <?php
  // Good 👍
  declare(strict_types=1);

  namespace App\Services;

  final class PriceService
  {
      private ?Customer $customer = null;

      public function calculateDiscount(
          float $price,
          float $discount,
      ): float {
          return $price * ($discount / 100);
      }

      public function findCustomer(int $customerNo): ?Customer {
          // ...
      }
  }

  // Good 👍 — narrowed once, at the boundary
  $limit = $request->integer('limit', 50);
  $users = $this->userService->paginate($limit);
  ```

### [PHP-STYLE-005] Constructor Property Promotion and Readonly
- **Severity**: RECOMMENDED
- **Description**: Promote constructor parameters instead of assigning them by hand. Mark a property `readonly` when it must not change after construction, and the class `readonly` when all its properties are.
- **Examples**:
  ```php
  // Bad
  final class InvoiceService
  {
      private InvoiceRepository $repository;

      public function __construct(InvoiceRepository $repository) {
          $this->repository = $repository;
      }
  }

  // Good 👍
  final readonly class InvoiceService
  {
      public function __construct(
          private InvoiceRepository $repository,
          private LoggerInterface $logger,
      ) {}
  }
  ```

### [PHP-STYLE-006] Curly Braces for Control Structures
- **Severity**: REQUIRED
- **Description**: Use curly braces for all flow control statements, even single-line bodies.
- **Examples**:
  ```php
  // Bad
  if ($isTrue)
      echo 'true';
  if ($arg === null) return true;

  // Good 👍
  if ($isTrue) {
      echo 'true';
  }

  if ($arg === null) {
      return true;
  }
  ```

### [PHP-STYLE-007] Line Break Rules
- **Severity**: RECOMMENDED
- **Description**:
  - Add 1 blank line **after** each `if` or loop block, unless it is the last statement of its block.
  - Add 1 blank line **before** `return`, unless it is the first statement of its block (e.g. a guard clause).
- **Examples**:
  ```php
  // Bad
  if ($condition) {
      // ...
  }
  foreach ($items as $item) {
      // ...
  }
  return true;

  // Good 👍
  if ($condition) {
      // ...
  }

  foreach ($items as $item) {
      // ...
  }

  return true;
  ```

### [PHP-STYLE-008] Type-Safe Comparisons
- **Severity**: REQUIRED
- **Description**:
  - Use `===` / `!==` instead of `==` / `!=`.
  - Pass `true` as the strict flag of `in_array()`, `array_search()` and `array_keys()`.
  - Both sides must have the **same type** (`'1' === 1` is `false`): convert once, at the boundary ([PHP-STYLE-004]), and cast only after `is_numeric()` — `(int) 'abc'` silently becomes `0`.
  - When a `switch` is turned into `match`, check the types: `match` compares strictly.
  - Loose comparisons are also reported by PHPStan. In review, focus on type mismatches between the two sides.
- **Examples**:
  ```php
  // Example 1: check NULL column data from database
  // Bad — when $userFlag = 0, it also returns early
  $userFlag = $this->user->getUserFlag();
  if ($userFlag == null) {
      return;
  }

  // Good 👍
  $userFlag = $this->user->getUserFlag();
  if ($userFlag === null) {
      return;
  }

  // Example 2: type mismatch (string vs number)
  // Bad (Laravel)
  $status = $request->input('status'); // returns string '1'
  if ($status === 1) { ... }           // '1' === 1 → false

  // Good 👍 (Laravel) — convert to the SAME type once, at the boundary
  $status = $request->integer('status');
  if ($status === 1) { ... }

  // Good 👍 (CakePHP) — toInt() returns null, not 0, for a value
  // that is not an integer ('abc', '1.5', '')
  use function Cake\Core\toInt;

  $status = toInt($this->request->getData('status'));
  if ($status === 1) { ... }

  // Good 👍 — strict in_array
  $validStatuses = [1, 2, 3];
  if (in_array($status, $validStatuses, true)) { ... }
  ```

### [PHP-STYLE-009] Nullsafe and Null Coalescing Operators
- **Severity**: REQUIRED
- **Description**: Use nullsafe `?->` and null coalescing `??` / `??=` instead of nested `null` checks and `isset()` ternaries, but not on a value that must never be `null` — fail early instead.
- **Examples**:
  ```php
  // Bad
  if ($user !== null) {
      $address = $user->address;

      if ($address !== null) {
          $city = $address->getCity();

          if ($city !== null) {
              $country = $city->country;
          }
      }
  }

  $foo = isset($bar) ? $bar : 'something';

  // Good 👍
  $country = $user?->address?->getCity()?->country;
  $foo = $bar ?? 'something';
  $options['limit'] ??= 50;
  ```

## 3. Comment

### [PHP-COMMENT-001] Comment Purpose (Why, Not What)
- **Severity**: REQUIRED
- **Description**: Comments should explain **why**, not repeat **what** the code already says.
- **Examples**:
  ```php
  // Bad: repeats what the code does
  // Check if user is inactive
  if ($user->status === UserStatus::Inactive) {
      return;
  }

  // Good 👍 explains the business reason
  // Inactive users are kept for audit history and must not receive notifications.
  if ($user->status === UserStatus::Inactive) {
      return;
  }
  ```

### [PHP-COMMENT-002] Single-Line Comments
- **Severity**: RECOMMENDED
- **Description**: Use `//` (never `#`), begin with 1 whitespace, capitalize the first word and write it like a sentence.
- **Examples**:
  ```php
  // Good 👍
  // In case no item in list, we do nothing
  if (! $hasItems) {
      return false;
  }
  ```

### [PHP-COMMENT-003] Multi-Line Comments
- **Severity**: RECOMMENDED
- **Description**: Use multi-line comments only for complex business logic, temporary migration notes or non-obvious technical constraints.
- **Examples**:
  ```php
  // Good 👍
  /*
   * This migration must keep old user numbers because external invoices
   * still reference them. Do not regenerate userNo here.
   */
  $this->migrateUserContracts();
  ```

### [PHP-COMMENT-004] PHPDoc Comments
- **Severity**: REQUIRED
- **Description**:
  - Write PHPDoc only for what the signature cannot say: a description, `@throws`, and the element type of an `array`, `iterable` or generic (`@param list<string> $paths`).
  - **Do not** repeat a declared type.
  - Separate the description and each group of tags with a blank line.
- **Examples**:
  ```php
  // Bad — every tag only repeats the signature
  /**
   * @param Request $request
   * @return string|null
   */
  public function redirectTo(Request $request): ?string {}

  // Good 👍 — adds what the signature cannot say
  /**
   * Builds the redirect target after login.
   * Guest users are sent back to the page they requested.
   *
   * @param array<int, string> $allowedPaths
   *
   * @throws InvalidRedirectException when the target host is not whitelisted
   */
  public function redirectTo(
      Request $request,
      array $allowedPaths,
  ): ?string {}
  ```

### [PHP-COMMENT-005] Comment Language
- **Severity**: REQUIRED
- **Description**: Comments should be written in English. Japanese is allowed only for business terms, configuration values, labels, or domain-specific names, and should be enclosed in quotes if possible.
- **Examples**:
  ```php
  // Bad
  // Mảng chứa các sinh viên
  $students = [];

  // Good
  // Array of students
  $students = [];

  // Exception: Laravel migration
  // This comment in DB, not source code comment
  $table->string('name', 60)->comment('氏名');

  // Exception: Comment for configuration value name
  ValueUtil::constToValue('common.pdf_type.STANDARD'); // 1:規格書
  ```

### [PHP-COMMENT-006] TODO and FIXME Comments
- **Severity**: RECOMMENDED
- **Description**: TODO/FIXME comments should include enough context to be actionable. If possible, include a ticket number or owner.
- **Examples**:
  ```php
  // Bad
  // TODO: fix this

  // Good 👍
  // TODO(#123456): Remove this fallback after the partner API v2 migration.
  $companyCode = $input['companyCode'] ?? $legacyCompanyCode;
  ```

## 4. Usage & Code Quality

### [PHP-USAGE-001] Early Exit (Guard Clauses)
- **Severity**: RECOMMENDED
- **Description**: When certain criteria must be met to continue execution, exit early. Flatten nested conditions: invert the condition and return instead of wrapping the main logic in a large `else` block.
- **Examples**:
  ```php
  // Bad
  public function publish(Post $post): bool {
      if ($post->isValid()) {
          if ($post->author->isActive()) {
              return $this->repository->publish($post);
          } else {
              return false;
          }
      } else {
          return false;
      }
  }

  // Good 👍
  public function publish(Post $post): bool {
      if (! $post->isValid()) {
          return false;
      }

      if (! $post->author->isActive()) {
          return false;
      }

      return $this->repository->publish($post);
  }
  ```

### [PHP-USAGE-002] Avoid Nested Logic
- **Severity**: RECOMMENDED
- **Description**: Avoid hand-rolled nested logic — look for a built-in function (`in_array`, `array_filter`, `array_column`, `str_contains`, …) or a `match` expression instead.
- **Examples**:
  ```php
  // Bad
  if ($day) {
      if (is_string($day)) {
          $day = strtolower($day);
          if ($day === 'friday') {
              return true;
          } elseif ($day === 'saturday') {
              return true;
          } elseif ($day === 'sunday') {
              return true;
          } else {
              return false;
          }
      } else {
          return false;
      }
  }

  return false;

  // Good 👍
  if (! is_string($day)) {
      return false;
  }

  $openingDays = [
      'friday',
      'saturday',
      'sunday',
  ];

  return in_array(strtolower($day), $openingDays, true);

  // Good 👍 — match for value mapping (strict comparison by design)
  $label = match ($food) {
      'apple' => 'This food is an apple',
      'cake' => 'This food is a cake',
      default => 'Unknown food',
  };
  ```

### [PHP-USAGE-003] Do Not Use empty()
- **Severity**: REQUIRED
- **Description**:
  - Do not use `empty()` — it treats `0`, `'0'`, `''`, `false` and `[]` as missing, so a valid `0` is rejected.
  - Use `isset()` when only missing / `null` matters (it is `true` for `0`, `''` and `[]`); otherwise check exactly what you mean: `=== null`, `=== ''`, `=== []`, `=== 0` or `count($items) === 0`.
- **Examples**:
  ```php
  $data = ['quantity' => 0];

  // Bad — a legit quantity of 0 is treated as "not provided"
  if (empty($data['quantity'])) {
      throw new InvalidArgumentException('quantity is required');
  }

  // Good 👍 — only "missing / null" is rejected
  if (! isset($data['quantity'])) {
      throw new InvalidArgumentException('quantity is required');
  }

  // Good 👍 — an empty list means nothing to do: say so explicitly
  if ($items === []) {
      return;
  }
  ```

### [PHP-USAGE-004] Named Arguments
- **Severity**: RECOMMENDED
- **Description**: Use named arguments instead of positional ones when you want to skip default values, or when a bare `true` / `null` at the call site says nothing about its meaning.
- **Examples**:
  ```php
  // Bad
  htmlspecialchars($string, ENT_QUOTES | ENT_SUBSTITUTE | ENT_HTML401, 'UTF-8', false);

  // Good 👍
  htmlspecialchars($string, double_encode: false);
  ```

### [PHP-USAGE-005] Enums for Fixed Sets of Values
- **Severity**: RECOMMENDED
- **Description**: Replace magic strings/numbers and loose class constants with a backed `enum`. The type declaration then guarantees only valid values reach the function. At the boundary (request, database), validate and convert once.
- **Examples**:
  ```php
  // Bad
  const STATUS_ACTIVE = 1;
  const STATUS_INACTIVE = 0;

  public function updateStatus(int $status): void {}

  // Good 👍
  enum UserStatus: int
  {
      case Active = 1;
      case Inactive = 0;

      public function getLabel(): string {
          return match ($this) {
              self::Active => 'Active',
              self::Inactive => 'Inactive',
          };
      }
  }

  public function updateStatus(UserStatus $status): void {}

  // At the boundary (request, DB), validate and convert once
  // (Laravel)
  $request->validate(['status' => ['required', Rule::enum(UserStatus::class)]]);
  $status = $request->enum('status', UserStatus::class)
      ?? throw new InvalidArgumentException('Invalid status');
  ```
  ```php
  // (CakePHP) src/Model/Table/UsersTable.php
  use Cake\Database\Type\EnumType;

  public function initialize(array $config): void {
      parent::initialize($config);

      // $user->status is a UserStatus from here on
      $this->getSchema()->setColumnType('status', EnumType::from(UserStatus::class));
  }

  public function validationDefault(Validator $validator): Validator {
      // EnumType silently turns an invalid value into null —
      // this rule is what rejects it
      return $validator->enum('status', UserStatus::class);
  }
  ```

### [PHP-USAGE-006] Maximum File Length
- **Severity**: REQUIRED
- **Description**: Limit each file to a maximum of **1000 lines**. Apply the Single Responsibility Principle, modularization and the DRY principle (inheritance, composition or utility functions) to keep files small.
- **Examples**:
  ```php
  // Good 👍
  // Split large files into smaller, focused modules
  // Each file handles a single responsibility
  ```

### [PHP-USAGE-007] Pin PHP Version
- **Severity**: REQUIRED
- **Description**:
  - Every PHP project must declare an exact PHP version and keep it consistent across `composer.json`, CI configuration and Docker/runtime configuration.
  - In `composer.json`, `require.php` declares the supported version constraint, and `config.platform.php` locks Composer's dependency resolution to an exact version, regardless of the PHP binary actually installed.
  - Keep the CI workflow and the Dockerfile base image pinned to the same exact version.
- **Examples**:
  ```json
  // composer.json
  {
      "require": {
          "php": "^8.5"
      },
      "config": {
          "platform": {
              "php": "8.5.11"
          }
      }
  }
  ```
  ```dockerfile
  # Good 👍 Docker base image aligned with composer.json platform
  FROM php:8.5.11-fpm
  ```

## 5. Security

### [SEC-DB-001] Use Parameterized Queries
- **Severity**: CRITICAL
- **Description**: Use parameterized queries or ORM query builders that bind parameters safely. Never concatenate user input directly into SQL.
- **Examples**:
  ```php
  // Bad
  $query = $this->query()->whereRaw("user.name LIKE {$nameInput}");

  // Good 👍 (Laravel)
  $query = $this->query()->whereRaw('user.name LIKE ?', [$nameInput]);

  // Good 👍 (CakePHP) — values in an array condition are bound
  $query = $this->Users->find()->where(['Users.name LIKE' => $nameInput]);
  ```

### [SEC-API-001] Implement Rate Limiting
- **Severity**: RECOMMENDED
- **Description**: Implement rate limiting to prevent brute force attacks and other forms of abuse. Depending on the project size and the original design, the project may choose not to apply this rule.
- **Examples**:
  ```php
  // Good 👍 (Laravel) — rate limit for all /api routes
  // app/Providers/AppServiceProvider.php
  use Illuminate\Cache\RateLimiting\Limit;
  use Illuminate\Http\Request;
  use Illuminate\Support\Facades\RateLimiter;

  public function boot(): void {
      RateLimiter::for(
          'api',
          fn (Request $request): Limit => Limit::perMinute(60)
              ->by($request->user()?->id ?? $request->ip()),
      );
  }
  ```

### [SEC-LOG-001] Use Logging and Monitoring
- **Severity**: RECOMMENDED
- **Description**: Log errors and security events so problems on the server can be detected. Log identifiers, not the whole request — it may contain passwords or personal data.
- **Examples**:
  ```php
  // Bad — no logging or monitoring
  public function cancel(Order $order): void {
      $order->cancel();
  }

  // Bad — the whole request may contain passwords or personal data
  Log::info('Data received', $request->all());

  // Good 👍
  public function cancel(Order $order): void {
      $order->cancel();
      Log::info('Order cancelled', ['orderNo' => $order->orderNo]);
  }
  ```

### [SEC-XSS-001] Escape HTML Output
- **Severity**: CRITICAL
- **Description**:
  - Use context-aware output encoding to prevent XSS. Do not manually insert untrusted input into HTML, JavaScript, CSS or URLs. When rendering user-provided rich text, sanitize it with an approved sanitizer.
  - Laravel: use Blade `{{ }}` — output is automatically sent through `htmlspecialchars`. Do not use `{!! !!}` for untrusted data.
  - CakePHP: output is not escaped automatically, so escape it with `h()`.
- **Examples**:
  ```php
  // Bad (Laravel)
  <?= $userName; ?>
  {!! $userName !!}

  // Good 👍 (Laravel)
  {{ $userName }}

  // Bad (CakePHP)
  <?= $userName; ?>

  // Good 👍 (CakePHP)
  <?= h($userName); ?>
  ```

### [SEC-FILE-001] Avoid User Input in File Paths and URLs
- **Severity**: CRITICAL
- **Description**: Avoid using direct user input for file paths, redirects, outbound URLs or shell commands. Use IDs, allowlists and safe mapping functions.
- **Examples**:
  ```php
  // Bad — open redirect
  return redirect($request->input('hiddenInputUrl'));

  // Good 👍
  $redirectUrl = getUrlFromInputId($request->integer('hiddenInputUrlId'));

  return redirect($redirectUrl);

  // Bad — path traversal
  $dataFileDetail = Storage::get($request->input('filePath'));

  // Good 👍
  $dataFileDetail = $this->fileStorage->readById(
      $request->integer('fileId'),
      $user->companyNo,
  );

  // Bad — command injection
  exec('convert ' . $request->input('fileName') . ' output.pdf');

  // Good 👍 — the path comes from our own record, passed as an argument list
  Process::run(['convert', $document->path, 'output.pdf']);
  ```

### [SEC-INPUT-001] Validate Input Both Client-Side and Server-Side
- **Severity**: CRITICAL
- **Description**: Always validate input on the server side, in addition to the client side. Client-side validation can be bypassed by attackers; server-side validation is what makes submitted data safe, complete and accurate.
- **Examples**:
  ```php
  // Bad — request data is saved without server-side validation
  Post::create($request->all());

  // Good 👍 (Laravel) — Form Request validation
  /**
   * @return array<string, ValidationRule|array<mixed>|string>
   */
  public function rules(): array {
      return [
          'title' => 'required|unique:posts|max:255',
          'body' => 'required',
      ];
  }
  ```
  ```php
  // Good 👍 (CakePHP) — validation set in the Table class
  public function validationUpdate(Validator $validator): Validator {
      return $validator
          ->notEmptyString('title', __('You need to provide a title'))
          ->notEmptyString('body', __('A body is required'));
  }

  $article = $this->Articles->newEntity(
      $this->request->getData(),
      ['validate' => 'update'],
  );
  ```

### [SEC-DATA-001] Hash Passwords, Encrypt Sensitive Information
- **Severity**: CRITICAL
- **Description**: Hash passwords. Do not encrypt passwords with reversible encryption, and do not use fast hashes such as `md5()` or `sha1()`. Use a strong password hashing algorithm such as Argon2id or bcrypt. Other sensitive information that must be stored in the database and read back later (not passwords) should be encrypted.
- **Examples**:
  ```php
  // Bad
  $user->password = md5($password);
  $user->password = Crypt::encryptString($password);

  // Good 👍 (Laravel)
  use Illuminate\Support\Facades\Hash;

  $hashedPassword = Hash::make($password);

  // Good 👍 (CakePHP) — cakephp/authentication plugin
  use Authentication\PasswordHasher\DefaultPasswordHasher;

  $hashedPassword = new DefaultPasswordHasher()->hash($password);
  ```

## 6. Clean Code & Architecture Principles

### Instructions for AI
Focus heavily on these principles during review. Prioritize readability and maintainability over cleverness.

- **[CLEAN-DRY] Don't Repeat Yourself**:
  - Detect duplicated logic across files or functions.
  - Suggest extracting repeated code into reusable utility functions, traits, or service classes.

- **[CLEAN-KISS] Keep It Simple**:
  - Flag over-engineered solutions.
  - Suggest the simplest implementation possible.

- **[CLEAN-YAGNI] You Aren't Gonna Need It**:
  - Strict check on unused parameters, dead code, or "future-proofing" features that are not currently used.
  - Suggest removing fields in Classes/Interfaces that serve no immediate purpose.

- **[CLEAN-SRP] Single Responsibility Principle**:
  - **Severity: REQUIRED**.
  - A function must do only one thing. If a function name has "And" (e.g., `validateAndSave`), suggest splitting it.
  - Separate Business Logic from Presentation Logic.

- **[CLEAN-FUNC-SIZE] Function Complexity**:
  - **Severity: RECOMMENDED**.
  - Do not judge a function by line count alone. Flag functions that are intellectually difficult to scan.
  - For long functions (more than 50 lines), consider extracting sub-routines with descriptive names.
  - Example:
    ```php
    // Bad: validation, persistence and notification are mixed together
    public function register(RegisterUserData $data): User {
        // More than 50 lines of validation, data mapping, persistence and email logic...
    }

    // Good 👍 each step communicates its purpose
    public function register(RegisterUserData $data): User {
        $this->validateRegistration($data);
        $user = $this->userRepository->create($data);
        $this->mailer->sendWelcomeMail($user);

        return $user;
    }
    ```

- **[CLEAN-PARAMS] Parameter Object Pattern**:
  - **Severity: RECOMMENDED**.
  - Functions with more than three arguments should take a typed parameter object: a DTO / value object (a `final readonly class` with promoted properties, see [PHP-STYLE-005]).
  - Do not use an associative array as the parameter object — it loses the types and needs an array-shape PHPDoc for PHPStan.
  - Example:
    ```php
    // Bad
    public function createUser(string $name, string $email, Role $role, int $companyNo): User {}

    // Bad — the types are lost
    public function createUser(array $data): User {}

    // Good 👍
    final readonly class CreateUserData
    {
        public function __construct(
            public string $name,
            public string $email,
            public Role $role,
            public int $companyNo,
        ) {}
    }

    public function createUser(CreateUserData $data): User {}
    ```
