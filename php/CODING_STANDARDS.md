# PHP Coding Rules

## Table of Contents
- [1. Naming](#1-naming)
- [2. Styling](#2-styling)
- [3. Comments](#3-comments)
- [4. Usage](#4-usage)
- [5. Security](#5-security)

---

## 1. Naming Conventions

### [PHP-NAMING-001] Files, Namespaces, Classes, Interfaces, Enums and Traits
- **Severity**: REQUIRED
- **Description**: Files, namespaces, classes, interfaces, enums, and traits must use `UpperCamelCase` format and should be a noun.
- **Examples**:
  ```php
  // Good
  // UserController.php

  namespace App\Http\Controllers;

  class UserController
  {
      // ...
  }

  interface Rule
  {
      // ...
  }

  enum UserType
  {
      case Admin;
      case Support;
  }

  trait CommonTrait
  {
      // ...
  }
  ```

### [PHP-NAMING-002] Functions, Properties and Variables
- **Severity**: REQUIRED
- **Description**: Functions, properties, and variables must use `lowerCamelCase` format. Function names should be a verb.
- **Examples**:
  ```php
  // Good
  function redirectTo($request) {
      // ...
  }

  $routeName = 'abc';
  ```

### [PHP-NAMING-003] Constants
- **Severity**: REQUIRED
- **Description**: Constants must use `UPPER_CASE_UNDERSCORE` format.
- **Examples**:
  ```php
  // Good
  const TABLE_NAME = 'users';
  ```

### [PHP-NAMING-004] Meaningful Variable Names
- **Severity**: REQUIRED
- **Description**: Variables must have descriptive names that clearly indicate their purpose.
- **Examples**:
  ```php
  // Bad
  $fn = 'John';
  $a = 20;

  // Good
  $firstName = 'John';
  $age = 20;
  ```

### [PHP-NAMING-005] Boolean Variables
- **Severity**: REQUIRED
- **Description**: Boolean variables or properties must start with question words like `is`, `can`, `should`, or `has`.
- **Examples**:
  ```php
  // Good
  $isConnected = true;
  $shouldConfirm = true;
  $canResize = true;
  ```

### [PHP-NAMING-006] Data Type Prefixes
- **Severity**: OPTIONAL
- **Description**: You can optionally prefix variable names to indicate the data type.
- **Examples**:
  ```php
  // Good
  $strName = 'John';
  $arrAnimals = [];
  ```

## 2. Styling

### [PHP-STYLE-001] Class Layout
- **Severity**: REQUIRED
- **Description**: Class elements must be ordered as: Traits → Constants (public → protected → private) → Properties (public → protected → private) → Constructor → Destructor → Magic methods → Methods (public → protected → private). Separate each section with 1 line break.
- **Examples**:
  ```php
  // Good
  class Example
  {
      use BarTrait;
      use BazTrait;

      public const PUBLIC_CONSTANT = 1;
      protected const PROTECTED_CONSTANT = 2;
      private const PRIVATE_CONSTANT = 2;

      public $pubicProperty;
      protected $protectedProperty;
      private $privateProperty;

      public function __construct() {}

      public function __destruct() {}

      public function __toString() {}

      public function pubicFunction() {}

      public static function pubicStaticFunction() {}

      protected function protectedFunction() {}

      protected static function protectedStaticFunction() {}

      private function privateFunction() {}

      private static function privateStaticFunction() {}
  }
  ```

### [PHP-STYLE-002] Maximum Line Length
- **Severity**: RECOMMENDED
- **Description**: Lines should not exceed 80 characters. When exceeding, break after commas and before operators.
- **Examples**:
  ```php
  // Good
  if (
      ($condition1 === true || $condition2 > 0)
      && $condition3 === false // Break before operator
      && $condition4 === 1
  ) {
      callSomeThing(
          $longNameParam, // Break after comma
          $otherLongNameParam,
      );
  }
  ```

### [PHP-STYLE-003] String Quotes
- **Severity**: REQUIRED
- **Description**: Use single quotes for string literals unless including variables or single quotes in the string.
- **Examples**:
  ```php
  // Bad
  $message = "Hello world";

  // Good
  $message1 = 'Hello world';
  $message2 = "Hello {$name}";
  ```

### [PHP-STYLE-004] Indentation
- **Severity**: REQUIRED
- **Description**: Use 4 whitespaces for indentation instead of tabs.
- **Examples**:
  ```php
  // Good
  function multiplyNumbers($a, $b) {
      $result = $a * $b;

      return $result;
  }
  ```

### [PHP-STYLE-005] Trailing Commas
- **Severity**: REQUIRED
- **Description**: Multi-line arrays, arguments, parameters, and match expressions must have a trailing comma on the last item.
- **Examples**:
  ```php
  // Good
  $foo = [
      'bar' => [
          'baz' => true,
          'baq' => true,
      ],
  ];

  foo(
      'bar',
      'baz',
  );

  function foo(
      $x,
      $y,
  ) {
      // ...
  }

  $returnValue = match ($food) {
      'apple' => 'This food is an apple',
      'bar' => 'This food is a bar',
  };
  ```

### [PHP-STYLE-006] One Statement Per Line
- **Severity**: REQUIRED
- **Description**: There must not be more than one statement per line.
- **Examples**:
  ```php
  // Bad
  foo(); bar();

  // Good
  foo();
  bar();
  ```

### [PHP-STYLE-007] Curly Braces for Control Structures
- **Severity**: REQUIRED
- **Description**: Always use curly braces for all flow control statements.
- **Examples**:
  ```php
  // Bad
  if ($isTrue)
      echo 'true';
  if ($arg === null) return true;

  // Good
  if ($isTrue) {
      echo 'true';
  }
  if ($arg === null) {
      return true;
  }
  ```

### [PHP-STYLE-008] Line Break Rules
- **Severity**: REQUIRED
- **Description**: Add 1 line break after each if/loop statement and before return keyword.
- **Examples**:
  ```php
  // Bad
  if ($condition) {
      // ...
  }
  for ($index = 0; $index < $count; $i++) {
      // ...
  }
  return true;

  // Good
  if ($condition) {
      // ...
  }

  for ($index = 0; $index < $count; $i++) {
      // ...
  }

  return true;
  ```

### [PHP-STYLE-009] Strict Comparison Operators
- **Severity**: CRITICAL
- **Description**: Use `===` instead of `==`, and `!==` instead of `!=` for comparisons.
- **Examples**:
  ```php
  // Bad
  $userFlag = $this->User->getUserFlag();
  if ($userFlag == null) {
      return;
  }

  // Good
  $userFlag = $this->User->getUserFlag();
  if ($userFlag === null) {
      return;
  }
  ```

## 3. Comments

### [PHP-COMMENT-001] Single-Line Comments
- **Severity**: REQUIRED
- **Description**: Comments must be formatted like sentences, begin with 1 whitespace, and capitalize the first word.
- **Examples**:
  ```php
  // Good
  // In case no item in list, we do nothing
  if (! $hasItems) {
      return false;
  }
  ```

### [PHP-COMMENT-002] Multi-Line Comments
- **Severity**: REQUIRED
- **Description**: All asterisks in multi-line comments must be aligned.
- **Examples**:
  ```php
  // Good
  /*
   * This is a multi-line comment.
   * It can be used to explain large sections of code.
   */
  $age = 30;
  ```

### [PHP-COMMENT-003] PHPDoc Comments
- **Severity**: REQUIRED
- **Description**: Use PHPDoc for documenting functions, methods, and classes. Include line break between descriptions and tags.
- **Examples**:
  ```php
  // Good
  /**
   * This is a PHPDoc comment.
   * There should be a line break between Descriptions and Tags.
   *
   * @param \Illuminate\Http\Request $request
   * @return string|null
   */
  protected function redirectTo($request) {
      // ...
  }
  ```

### [PHP-COMMENT-004] Language
- **Severity**: REQUIRED
- **Description**: Use English only for all comments.
- **Examples**:
  ```php
  // Bad
  // Mảng chứa các sinh viên
  $students = [];

  // Good
  // Array of students
  $students = [];
  ```

## 4. Usage

### [PHP-USAGE-001] Array Syntax
- **Severity**: REQUIRED
- **Description**: Arrays must be declared using short syntax `[]` instead of `array()`.
- **Examples**:
  ```php
  // Bad
  $numbers = array(1, 2);

  // Good
  $numbers = [1, 2];
  ```

### [PHP-USAGE-002] Explicit Variables in Strings
- **Severity**: REQUIRED
- **Description**: Use explicit curly braces `{$var}` instead of implicit `$var` in double-quoted strings.
- **Examples**:
  ```php
  // Bad
  $name = 'World';
  $message = "Hello $name";

  // Good
  $name = 'World';
  $message = "Hello {$name}";
  ```

### [PHP-USAGE-003] Curly Braces for Indirect Variables
- **Severity**: REQUIRED
- **Description**: Add curly braces to indirect variables for clarity.
- **Examples**:
  ```php
  // Bad
  echo $$foo;
  echo $$foo['bar'];
  echo $foo->$bar['baz'];
  echo $foo->$callback($baz);

  // Good
  echo ${$foo};
  echo ${$foo}['bar'];
  echo $foo->{$bar}['baz'];
  echo $foo->{$callback}($baz);
  ```

### [PHP-USAGE-004] Early Exit (Guard Clauses)
- **Severity**: RECOMMENDED
- **Description**: Use guard clauses to exit early instead of nesting conditions.
- **Examples**:
  ```php
  // Bad
  if ($isTrue) {
      // ...
  } else {
      return;
  }

  // Good
  if (! $isTrue) {
      return;
  }
  ```

### [PHP-USAGE-005] Distinguish Between isset() and !empty()
- **Severity**: REQUIRED
- **Description**: `isset()` checks if variable exists and is not `null`. `empty()` checks for empty values: `""`, `0`, `0.0`, `"0"`, `null`, `false`, `[]`.
- **Examples**:
  ```php
  // Use isset() to check if variable exists
  if (isset($var)) {
      // ...
  }

  // Use empty() to check if variable has a value
  if (! empty($var)) {
      // ...
  }
  ```

### [PHP-USAGE-006] Array Assignment
- **Severity**: REQUIRED
- **Description**: Use `$x[] = $y` instead of `array_push($x, $y)` for simple array additions.
- **Examples**:
  ```php
  // Bad
  $animals = ['tiger', 'lion'];
  array_push($animals, 'cat');

  // Good
  $animals = ['tiger', 'lion'];
  $animals[] = 'cat';
  ```

### [PHP-USAGE-007] Logical NOT Operator
- **Severity**: REQUIRED
- **Description**: Logical NOT operator `!` must have one trailing whitespace.
- **Examples**:
  ```php
  // Bad
  if (!$bar) {
      echo 'Help!';
  }

  // Good
  if (! $bar) {
      echo 'Help!';
  }
  ```

### [PHP-USAGE-008] Group Same Namespaces
- **Severity**: REQUIRED
- **Description**: Same namespaces must be grouped using curly braces syntax.
- **Examples**:
  ```php
  // Bad
  use Foo\Bar;
  use Foo\Baz;

  // Good
  use Foo\{Bar, Baz};
  ```

### [PHP-USAGE-009] Sort Import Statements
- **Severity**: REQUIRED
- **Description**: All `use` statements (imports) must be sorted alphabetically.
- **Examples**:
  ```php
  // Bad
  use Illuminate\Support\Facades\Route;
  use Illuminate\Database\Eloquent\Model;
  use App\Models\{
    User,
    Post,
  };
  use App\Controllers\UserController;

  // Good
  use App\Controllers\UserController;
  use App\Models\{
    Post,
    User,
  };
  use Illuminate\Database\Eloquent\Model;
  use Illuminate\Support\Facades\Route;
  ```

### [PHP-USAGE-010] Named Arguments
- **Severity**: OPTIONAL
- **Description**: Use Named Arguments instead of Positional arguments when ignoring default values (PHP 8+).
- **Examples**:
  ```php
  // Bad
  htmlspecialchars($string, default, default, false);

  // Good
  htmlspecialchars($string, double_encode: false);
  ```

### [PHP-USAGE-011] Nullsafe Operator
- **Severity**: OPTIONAL
- **Description**: Use nullsafe operator `?->` to simplify null checks (PHP 8+).
- **Examples**:
  ```php
  // Bad
  $user = null;
  if ($user !== null) {
      $address = $user->address;
      if ($address !== null) {
          $city = $address->getCity();
      }
  }

  // Good
  $country = $user?->address?->getCity()?->country;
  ```

### [PHP-USAGE-012] Null Coalescing Operator
- **Severity**: REQUIRED
- **Description**: Use `??` operator instead of ternary with `isset()` (PHP 7+).
- **Examples**:
  ```php
  // Bad
  $foo = isset($bar) ? $bar : 'something';

  // Good
  $foo = $bar ?? 'something';
  ```

### [PHP-USAGE-013] Avoid Nested Logic
- **Severity**: RECOMMENDED
- **Description**: Avoid deep nesting; use built-in functions or refactor to flat structure.
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
  } else {
      return false;
  }

  // Good
  if (empty($day)) {
      return false;
  }

  $openingDays = [
      'friday',
      'saturday',
      'sunday',
  ];

  return in_array(strtolower($day), $openingDays, true);
  ```

### [PHP-USAGE-014] Maximum File Length
- **Severity**: REQUIRED
- **Description**: Limit each file to maximum 1000 lines. Apply SRP, modularization, and DRY principle.
- **Examples**:
  ```php
  // Good
  // Split large files into smaller, focused modules
  // Each file handles a single responsibility
  ```

## 5. Security

### [PHP-SECURITY-001] Use Parameterized Queries
- **Severity**: CRITICAL
- **Description**: Always use parameterized queries with parameter binding to prevent SQL injection attacks.
- **Examples**:
  ```php
  // Bad
  $query = $this->query()->whereRaw("user.name LIKE {$nameInput}");

  // Good
  $query = $this->query()->whereRaw('user.name LIKE ?', [$nameInput]);
  ```

### [PHP-SECURITY-002] Choose Libraries with Proven Security
- **Severity**: REQUIRED
- **Description**: For open source libraries, check popularity (1000+ downloads/month), security vulnerabilities (CVE, Snyk), active maintenance, permissive license, audits, and code quality. For proprietary libraries, review security policies and data handling practices.
- **Examples**:
  ```php
  // Good
  // Use well-maintained libraries with active security monitoring
  // Check Snyk, CVE databases for vulnerabilities
  // Prefer MIT or Apache 2.0 licensed libraries
  ```

### [PHP-SECURITY-003] Implement Rate Limiting
- **Severity**: OPTIONAL
- **Description**: Implement rate limiting to prevent brute force attacks (optional based on project size).
- **Examples**:
  ```php
  // Good - Laravel example
  use Illuminate\Cache\RateLimiting\Limit;
  use Illuminate\Support\Facades\RateLimiter;

  public function boot(): void {
      RateLimiter::for('api', function (Request $request) {
          return Limit::perMinute(60)->by($request->user()?->id ?: $request->ip());
      });
  }
  ```

### [PHP-SECURITY-004] Use Logging and Monitoring
- **Severity**: REQUIRED
- **Description**: Implement logging and monitoring to track errors and security events.
- **Examples**:
  ```php
  // Bad - no logging
  public function handleData(Request $request) {
      // Store data
  }

  // Good - with logging
  public function handleData(Request $request) {
      Log::info('Data received');
      Log::info($request->all());
      // Store data
  }
  ```

### [PHP-SECURITY-005] Escape HTML Output
- **Severity**: CRITICAL
- **Description**: Escape HTML output to prevent XSS attacks. Laravel: use `{{ }}`. CakePHP: use `h()` function.
- **Examples**:
  ```php
  // Bad (Laravel)
  <?= $userName; ?>
  {!! $userName !!}

  // Good (Laravel)
  {{ $userName }}

  // Bad (CakePHP)
  <?= $userName; ?>

  // Good (CakePHP)
  <?= h($userName); ?>
  ```

### [PHP-SECURITY-006] Avoid User Input in File Paths and URLs
- **Severity**: CRITICAL
- **Description**: Never use user input directly in file paths or redirect URLs. Use IDs to look up paths/URLs.
- **Examples**:
  ```php
  // Bad
  return redirect($request->input('hiddenInputUrl'));
  $dataFileDetail = Storage::get($request->input('filePath'));

  // Good
  $redirectUrl = getUrlFromInputId($request->input('hiddenInputUrlId'));
  return redirect($redirectUrl);
  ```

### [PHP-SECURITY-007] Validate Input Both Client-Side and Server-Side
- **Severity**: CRITICAL
- **Description**: Always validate input on both client-side and server-side. Server-side validation is critical.
- **Examples**:
  ```php
  // Good - Laravel Form Request Validation
  public function rules(): array {
      return [
          'title' => 'required|unique:posts|max:255',
          'body' => 'required',
      ];
  }
  ```

### [PHP-SECURITY-008] Encrypt Sensitive Information
- **Severity**: CRITICAL
- **Description**: Encrypt sensitive information (e.g., passwords) before storing in database. Use Bcrypt.
- **Examples**:
  ```php
  // Good - Laravel
  use Illuminate\Support\Facades\Hash;
  $hashedPassword = Hash::make($password);

  // Good - CakePHP
  use Cake\Auth\DefaultPasswordHasher;
  $hasher = new DefaultPasswordHasher();
  $hashedPassword = $hasher->hash($password);
  ```