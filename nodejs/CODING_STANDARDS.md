# NodeJs Coding Rules & Security BWV

## Table of Contents
- [1. Naming](#1-naming)
- [2. Styling](#2-styling)
- [3. Comment](#3-comment)
- [4. Usage](#4-usage)
- [5. Security](#5-security)

---

## 1. Naming

### [TS-NAMING-001] Use camelCase for variables 
- **Severity**: REQUIRED
- **Description**: Variables must utilize `camelCase`.
- **Examples**:
  ```typescript
  // Bad
  let first_name = 'John';

  // Good
  let firstName = 'John';
  ```

### [TS-NAMING-002] Use meaningful names
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

### [TS-NAMING-003] Avoid overly long variable names
- **Severity**: RECOMMENDED
- **Description**: Variable names should be concise but descriptive.
- **Examples**:
  ```typescript
  // Bad
  let thisIsAVariableThatContainsTheFirstNameOfTheUser = 'John';

  // Good
  let firstName = 'John';
  ```

### [TS-NAMING-004] No leading underscores
- **Severity**: REQUIRED
- **Description**: Do not start variable names with an underscore _ unless it is a specific framework requirement or private field convention (though private keyword is preferred).
- **Examples**:
  ```typescript
  // Bad
  let _firstName = 'John';
  ```

### [TS-NAMING-005] Hungarian Notation / Data Type Prefix
- **Severity**: OPTIONAL
- **Description**: Can use prefixes to indicate data types if it helps clarity (e.g., str, num, is).
- **Examples**:
  ```typescript
  // Acceptable
  let strName = 'John';
  let numValue = 10;
  ```

### [TS-NAMING-006] No unclear abbreviations
- **Severity**: REQUIRED
- **Description**: Avoid abbreviations that are not universally understood.
- **Examples**:
  ```typescript
  // Bad
  let fn = 'John';

  // Good
  let firstName = 'John';
  ```

### [TS-NAMING-007] Constants must be UPPER_SNAKE_CASE
- **Severity**: REQUIRED
- **Description**: Constants (especially global/config constants) must use UPPER_CASE with underscores.
- **Examples**:
  ```typescript
  // Bad
  const bucketUpload = 'folder'

  // Good
  const BUCKET_UPLOAD = 'folder';
  ```

### [TS-NAMING-008] Boolean variables must use verb prefixes
- **Severity**: REQUIRED
- **Description**: Boolean variables should start with is, should, can, has, etc.
- **Examples**:
  ```typescript
  let isConnected = true;
  let shouldConfirm = true;
  let canResize = true;
  ```

### [TS-NAMING-009] Use let/const instead of var
- **Severity**: REQUIRED
- **Description**: Never use var. Use const by default, and let if reassignment is needed.
- **Examples**:
  ```typescript
  // Bad
  var num = 10;

  // Good
  let num = 10;
  const PI = 3.14;
  ```

### [TS-NAMING-010] Avoid keyword collisions
- **Severity**: 
- **Description**: Do not use variable names that clash with language keywords (e.g., class, print, break).
- **Examples**:
  ```typescript
  // Bad
  let class = 'Math';

  // Good
  let className = "Math";
  ```

### [TS-NAMING-011] 
- **Severity**: REQUIRED
- **Description**: Class names, Interfaces, Types, and Enums must use PascalCase.
- **Examples**:
  ```typescript
  // Bad
  class person {}
  enum color {}

  // Good
  class Person {}
  enum Color {}
  ```

## 2. Styling

### [TS-STYLE-001] Use single quotes
- **Severity**: RECOMMENDED
- **Description**: Use single quotes ' for string literals unless interpolating or escaping.
- **Examples**:
  ```typescript
  // Bad
  const message = "Hello";

  // Good
  const message = 'Hello';
  ```

### [TS-STYLE-002] Explicit Type Annotations
- **Severity**: REQUIRED
- **Description**: Use type annotations specifically for function parameters and return types to enhance readability.
- **Examples**:
  ```typescript
  // Bad
  function calculate(price, discount) { ... }

  // Good
  function calculate(price: number, discount: number): number { ... }
  ```

### [TS-STYLE-003] Indentation (2 Spaces)
- **Severity**: REQUIRED
- **Description**: Description: Use 2 spaces for indentation (configured via .editorconfig or Prettier).
- **Examples**:
  ```typescript
  // Bad
  function multiplyNumbers(a: number, b: number): number {
      return a * b;
  }

  // Good
  function multiplyNumbers(a: number, b: number): number {
    return a * b;
  }
  ```

### [TS-STYLE-004] Async/Await over Callbacks
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

### [TS-STYLE-005] Defensive Programming (Null Checks)
- **Severity**: REQUIRED
- **Description**: Always check for null/undefined. Use Optional Chaining (?.) where possible.
- **Examples**:
  ```typescript
  // Bad
  console.log(user.address.city);

  // Good
  console.log(user?.address?.city);
  ```

### [TS-STYLE-006] Use Interfaces for Object Shapes
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

### [TS-LINT-001] No Console.log
- **Severity**: WARNING
- **Description**: Do not leave console.log in production code. Use a logger instead.
- **Examples**:
  ```typescript
  // Bad
  console.log(`Listening on ${bind}`);

  // Good
  logger.info(`Listening on ${bind}`);
  ```

### [TS-LINT-002] Enforce Semicolons
- **Severity**: REQUIRED
- **Description**: Statements must end with a semicolon ;.
- **Examples**:
  ```typescript
  // Bad - required ';' at the end
  const age = 20

  // Good
  const age = 20;
  ```

### [TS-LINT-003] No Debugger
- **Severity**: ERROR
- **Description**: debugger statements are strictly forbidden in committed code.
- **Examples**:
  ```typescript
  function isTruthy(x) {
    debugger; // <-- Error line
    return Boolean(x);
  }
  ```

### [TS-LINT-004] No Explicit Any
- **Severity**: WARNING
- **Description**: Avoid any. Define types or use unknown if absolutely necessary.
- **Examples**:
  ```typescript
  // Bad
  const age: any = '17';

  // Good
  const age: number = 17;
  ```

### [TS-LINT-006] Specific Imports
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

### [SEC-LOG-001] Structured Logging
- **Severity**: REQUIRED
- **Description**: Use structured logging (e.g., Winston) instead of standard output. Log to files/streams, not just console.

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