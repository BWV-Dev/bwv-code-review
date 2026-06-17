# NodeJs Coding Rules & Security BWV

## Table of Contents
- [1. Naming](#1-naming)
- [2. Styling](#2-styling)
- [3. Comment](#3-comment)
- [4. Usage](#4-usage)
- [5. Security](#5-security)
- [6. Clean Code & Architecture Principles](#6-clean-code--architecture-principles)

---

## 1. Naming

### [TS-NAMING-001] Use meaningful names
- **Severity**: REQUIRED
- **Description**: Names must be descriptive. Avoid single letters (e.g., a, b) unless in small loops
- **Examples**:
  ```typescript
    // Bad
    let a = 'John';
    let b = 20;

    // Good
    let firstName = 'John';
    let age = 20;
    ```

### [TS-NAMING-002] Avoid overly long variable names
- **Severity**: RECOMMENDED
- **Description**: Variable names should be concise but descriptive.
- **Examples**:
  ```typescript
  // Bad
  let thisIsAVariableThatContainsTheFirstNameOfTheUser = 'John';

  // Good
  let firstName = 'John';
  ```

### [TS-NAMING-003] No leading underscores
- **Severity**: REQUIRED
- **Description**: Do not start variable names with an underscore _ unless it is a specific framework requirement or private field convention (though private keyword is preferred).
- **Examples**:
  ```typescript
  // Bad
  let _firstName = 'John';
  ```

### [TS-NAMING-004] Hungarian Notation / Data Type Prefix
- **Severity**: OPTIONAL
- **Description**: Can use prefixes to indicate data types if it helps clarity (e.g., str, num, is).
- **Examples**:
  ```typescript
  // Acceptable
  let strName = 'John';
  let numValue = 10;
  ```

### [TS-NAMING-005] No unclear abbreviations
- **Severity**: REQUIRED
- **Description**: Avoid abbreviations that are not universally understood.
- **Examples**:
  ```typescript
  // Bad
  let fn = 'John';

  // Good
  let firstName = 'John';
  ```

### [TS-NAMING-006] Constants must be UPPER_SNAKE_CASE
- **Severity**: REQUIRED
- **Description**: Constants (especially global/config constants) must use UPPER_CASE with underscores.
- **Examples**:
  ```typescript
  // Bad
  const bucketUpload = 'folder'

  // Good
  const BUCKET_UPLOAD = 'folder';
  ```

## 2. Styling

### [TS-STYLE-001] Explicit Type Annotations
- **Severity**: REQUIRED (parameters) / OPTIONAL (return type)
- **Description**:
  - **Parameters**: REQUIRED to annotate explicitly.
  - **Return type**: OPTIONAL when the function returns a single, simple shape — let TypeScript infer it.
  - **Return type**: REQUIRED when the function can return multiple shapes/formats (union types, conditional responses, etc.) so callers know exactly what to handle.
- **Examples**:
  ```typescript
  // Bad - parameters not annotated
  function calculate(price, discount) { ... }

  // Good - parameters typed, return type inferred
  function calculate(price: number, discount: number) {
    return price * (discount / 100);
  }

  // Good - multiple return shapes → declare return type explicitly
  function findUser(id: number): User | { error: string } | null {
    ...
  }
  ```

### [TS-STYLE-002] Async/Await over Callbacks
- **Severity**: REQUIRED
- **Description**: Avoid callback hell. Use async/await syntax.
- **Examples**:
  ```typescript
  // Bad
  fetch(url).then(res => res.json()).then(data => ...);

  // Good
  const res = await fetch(url);
  const data = await res.json();
  ```

### [TS-STYLE-003] Use Interfaces for Object Shapes
- **Severity**: RECOMMENDED
- **Description**: Define object structures using interface.
- **Examples**:
  ```typescript
  interface User {
    firstName: string;
    age: number;
  }
  ```

## 3. Comment

### [TS-DOC-001] JSDoc for Functions
- **Severity**: REQUIRED (For public APIs/Utils)
- **Description**: Use JSDoc /** ... */ for complex logic or exported functions.
- **Examples**:
  ```typescript
  /**
  * Adds two numbers together.
  * @param {number} a - The first number to add.
  * @param {number} b - The second number to add.
  * @returns {number} - The sum of a and b.
  */
  function add(a: number, b: number): number { ... }
  ```
### [TS-DOC-002] Comment Screen name or API url
- **Severity**: REQUIRED
- **Description**: Should comment Screen name or API url before doing something.
- **Examples**:
  ```typescript
  /**
  * S306_1 締め処理
  */
  <script lang=ts setup>...</script>

  /**
  * api/customer
  */
  export const search = async (req: Request, res: Response, next: NextFunction) => {...};
  ```

## 4. Usage & Best Practices

### [TS-USAGE-001] Use Lodash/Utils for safety
- **Severity**: RECOMMENDED
- **Description**: Use libraries like Lodash for safe Object/Array manipulation to avoid runtime exceptions.
- **Examples**:
  ```typescript
  // Better
  import { filter } from "lodash";
  const expensive = filter(products, p => p.price > 100);
  ```

### [TS-LINT-001] Specific Imports
- **Severity**: RECOMMENDED
- **Description**: Import only what you need to reduce bundle size.
- **Examples**:
  ```typescript
  // Bad
  import * as _ from 'lodash';

  // Good
  import { get } from 'lodash';
  ```

## 5. Security

### [SEC-DB-001] Parameterized Queries (SQL Injection)
- **Severity**: CRITICAL
- **Description**: NEVER concatenate strings into SQL queries. Use binding parameters/ORM methods.
- **Examples**:
  ```typescript
  // Bad
  const query = `SELECT * FROM users WHERE id=${userId}`;

  // Good
  const query = "SELECT * FROM users WHERE id=?";
  ```

### [SEC-API-001] Rate Limiting
- **Severity**: RECOMMENDED
- **Description**: Implement rate limiting on public endpoints to prevent brute-force/DDoS.

### [SEC-XSS-001] Escape HTML (XSS)
- **Severity**: CRITICAL
- **Description**: Always escape user input before rendering to HTML.
- **Examples**:
  ```typescript
  // Use libraries like 'escape-html' or framework features
  const safe = escapeHtml(userInput);
  ```

### [SEC-FILE-001] No User Input in File Paths
- **Severity**: CRITICAL
- **Description**: Do not use raw user input to determine file paths or URLs (Path Traversal). Use IDs/Maps instead.
- **Examples**:
  ```typescript
  // Bad
  res.redirect(req.body.hiddenInputUrl);

  // Bad
  const dataFileDetail = fs.readFileSync(req.body.filePath);

  // Good 👍
  const redirectUrl = getUrlFromInputId(req.body.hiddenInputUrlId);
  res.redirect(redirectUrl);
  ```

### [SEC-DATA-001] Encrypt Sensitive Data
- **Severity**: CRITICAL
- **Description**: Passwords and PII (Personally Identifiable Information) must be hashed (Bcrypt) or encrypted at rest.
- **Examples**:
  ```typescript
  const hash = await bcrypt.hash(password, salt);
  ```

## 6. Clean Code & Architecture Principles

### Instructions for AI
Focus heavily on these principles during review. Prioritize readability and maintainability over cleverness.

- **[CLEAN-DRY] Don't Repeat Yourself**:
  - Detect duplicated logic across files or functions.
  - Suggest extracting repeated code into reusable utility functions or hooks.

- **[CLEAN-KISS] Keep It Simple**:
  - Flag over-engineered solutions (e.g., using `reduce` where `map` suffices, complex regex where string methods work).
  - Suggest the simplest implementation possible.

- **[CLEAN-YAGNI] You Aren't Gonna Need It**:
  - Strict check on unused parameters, dead code, or "future-proofing" features that are not currently used.
  - Suggest removing fields in Classes/Interfaces that serve no immediate purpose.

- **[CLEAN-SRP] Single Responsibility Principle**:
  - **Severity: REQUIRED**.
  - A function must do only one thing. If a function name has "And" (e.g., `validateAndSave`), suggest splitting it.
  - Separate Business Logic from UI/View Logic.

- **[CLEAN-NESTING] Early Return / Guard Clauses**:
  - **Severity: RECOMMENDED**.
  - Flatten deep nesting (>3 levels).
  - Suggest inverting `if` statements to return early (Guard Clauses) instead of wrapping logic in big `else` blocks.