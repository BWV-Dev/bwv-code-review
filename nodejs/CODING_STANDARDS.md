# NodeJs Coding Rules & Security BWV

## Table of Contents
- [1. Naming](#1-naming)
- [2. Styling & TypeScript](#2-styling--typescript)
- [3. Comment](#3-comment)
- [4. Usage & Code Quality](#4-usage--code-quality)
- [5. Security](#5-security)
- [6. Clean Code & Architecture Principles](#6-clean-code--architecture-principles)

---

## 1. Naming

### [TS-NAMING-001] Use camelCase for variables, functions, methods and object properties
- **Severity**: REQUIRED
- **Description**: Use camelCase for variables, functions, methods and object properties.
- **Examples**:
  ```typescript
  // Bad
  let first_name = 'John';
  function get_user_name() {}
  
  // Good 👍
  let firstName = 'John';
  function getUserName() {}
  ```

### [TS-NAMING-002] Use meaningful variable names
- **Severity**: REQUIRED
- **Description**: Names must be descriptive. Avoid single letters (e.g., a, b) unless in small loops
- **Examples**:
  ```typescript
    // Bad
    let a = await repo.find();
    let b = true;

    // Good 👍
    const residents = await residentRepository.findMany();
    const isResidentActive = true;
    ```

### [TS-NAMING-003] Avoid overly long or overly short names
- **Severity**: RECOMMENDED
- **Description**: Avoid overly long or overly short names. Long names often mean the function/object is doing too much.
- **Examples**:
  ```typescript
  // Bad
  const thisIsTheResidentNameReturnedFromTheDatabaseAfterSearch = 'John';
  const r = 'John';

  // Good 👍
  const residentName = 'John';
  ```

### [TS-NAMING-004] Use boolean names that describe state or capability
- **Severity**: RECOMMENDED
- **Description**: Boolean names should read clearly as a predicate, state, capability, or intention. Prefer is, has, can, should where they improve clarity; descriptive adjectives such as enabled, visible, and loading are acceptable.
- **Examples**:
  ```typescript
  const isConnected = true;
  const hasPermission = true;
  const canResize = false;
  const shouldConfirm = true;
  const loading = true;
  ```

### [TS-NAMING-005] Do not use unclear abbreviations in variable names
- **Severity**: REQUIRED
- **Description**: Do not use unclear abbreviations in variable names.
- **Examples**:
  ```typescript	
  // Bad
  let fn = 'John';

  // Good 👍
  let firstName = 'John';
  ```

### [TS-NAMING-006] Use PascalCase for classes, interfaces, type aliases and enums
- **Severity**: REQUIRED
- **Description**: Use PascalCase for classes, interfaces, type aliases and enums.
- **Examples**:
  ```typescript
  // Bad
  class residentService {}
  interface residentDto {}
  type residentStatus = 'active' | 'inactive';

  // Good 👍
  class ResidentService {}
  interface ResidentDto {}
  type ResidentStatus = 'active' | 'inactive';

  enum PaymentStatus {
    Paid = 'paid',
    Unpaid = 'unpaid',
  }
  ```

### [TS-NAMING-007] Use UPPER_CASE only for module-level constants
- **Severity**: REQUIRED
- **Description**: Use UPPER_CASE only for module-level constants or configuration values that behave like real constants. Do not use UPPER_CASE for every local const.
- **Examples**:
  ```typescript
  // Good 👍
  const DEFAULT_PAGE_SIZE = 50;
  const MAX_RETRY_COUNT = 3;

  function buildResidentName(resident: Resident) {
    // Good 👍 local const should stay camelCase
    const residentName = `${resident.firstName} ${resident.lastName}`;
    return residentName;
  }
  ```

### [TS-NAMING-008] Do not use _ to mark private fields
- **Severity**: RECOMMENDED
- **Description**: Do not use _ to mark private fields. Use TypeScript private, protected or JavaScript private fields instead. _ is acceptable for intentionally unused parameters when ESLint is configured for it.
- **Examples**:
  ```typescript
  // Bad
  class UserService {
    private _token = '';
  }

  // Good 👍
  class UserService {
    private token = '';
  }

  // Acceptable 👍 intentionally unused parameter
  function handleError(_err: unknown, req: Request, res: Response) {
    res.status(500).json({ message: 'Internal server error' });
  }
  ```

### [TS-NAMING-009] Use const or let instead of var
- **Severity**: REQUIRED
- **Description**: Use const by default. Use let only when reassignment is needed. Do not use var.
- **Examples**:
  ```typescript
  // Bad
  var count = 1;

  // Good 👍
  const DEFAULT_LIMIT = 50;
  let retryCount = 0;
  retryCount += 1;
  ```

### [TS-NAMING-010] Avoid using variable names that are identical to keywords in the programming language or libraries
- **Severity**: REQUIRED
- **Description**: Avoid using variable names that are identical to keywords in the programming language or libraries.
- **Examples**:
  ```typescript
  // Bad
  let class = 'Mathematics';
  let print  = function() {};

  // Good 👍
  let className = "Mathematics";
  let printStudentName = function() {};
  ```

## 2. Styling & TypeScript

### [TS-STYLE-001] Type function parameters explicitly
- **Severity**: REQUIRED
- **Description**: Function parameters must have explicit types.
- **Examples**:
  ```typescript
  // Bad
  function calculateDiscount(price, discount) {
    return price * (discount / 100);
  }

  // Good 👍
  function calculateDiscount(price: number, discount: number) {
    return price * (discount / 100);
  }
  ```

### [TS-STYLE-002] Declare return types for exported or complex functions
- **Severity**: REQUIRED / RECOMMENDED
- **Description**: Define explicit return types for exported/public functions and functions with complex return shapes. For simple local functions, explicit return types are optional when TypeScript inference is clear.
- **Examples**:
  ```typescript
  // Good 👍 simple local function can infer return type
  function calculateDiscount(price: number, discount: number) {
    return price * (discount / 100);
  }

  // Good 👍 public/complex return should be explicit
  export async function findResident(
    residentNo: string,
  ): Promise<ResidentDto | null> {
    return residentService.findByResidentNo(residentNo);
  }

  // Good 👍 complex union return should be explicit
  function parsePage(value: unknown): number | null {
    if (typeof value !== 'string') return null;
    const page = Number(value);
    return Number.isInteger(page) && page > 0 ? page : null;
  }
  ```

### [TS-STYLE-003] Use optional chaining and nullish coalescing where appropriate
- **Severity**: REQUIRED
- **Description**: Use optional chaining `?.` and nullish coalescing `??` for nullable data. Avoid deeply nested defensive checks when language syntax can express it clearly.
- **Examples**:
  ```typescript
  // Bad
  const city = user && user.address && user.address.city
    ? user.address.city
    : 'Unknown';

  // Good 👍
  const city = user?.address?.city ?? 'Unknown';
  ```

### [TS-STYLE-004] Use async/await instead of callbacks wherever possible
- **Severity**: RECOMMENDED
- **Description**: Use async/await instead of callbacks wherever possible.
- **Examples**:
  ```typescript
  // Not good enough
  function fetchData(callback: (data: any) => void): void   {
    fetch('https://jsonplaceholder.typicode.com/todos/1')
      .then(response => response.json())
      .then(data => callback(data));
  }

  // Good 👍
  async function fetchData(): Promise<any> {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos/1');
    const data = await response.json();
    return data;
  }
  ```

### [TS-STYLE-005] Use interface and type based on the use case
- **Severity**: RECOMMENDED
- **Description**: Use interface for public object contracts that may be extended. Use type for unions, mapped types, utility types and function signatures.
- **Examples**:
  ```typescript
  // Good 👍 object contract
  interface ResidentDto {
    residentNo: string;
    name: string;
  }

  // Good 👍 union type
  type ResidentStatus = 'active' | 'inactive' | 'deleted';

  // Good 👍 mapped/utility type
  type ResidentPatch = Partial<Pick<ResidentDto, 'name'>>;
  ```

### [TS-STYLE-006] Avoid non-null assertion unless there is a clear reason
- **Severity**: RECOMMENDED
- **Description**: Avoid non-null assertion `!`. It disables TypeScript safety. Prefer validation, early return or throwing a clear error. `!` is acceptable only when non-null is guaranteed by an invariant that TypeScript cannot narrow (e.g. map.get(key) right after map.has(key)); add a short comment stating the guarantee.
- **Examples**:
  ```typescript
  // Bad
  const residentName = resident!.name;

  // Good 👍
  if (!resident) {
    throw new NotFoundError('Resident not found');
  }

  const residentName = resident.name;

  // Acceptable 👍 invariant TypeScript cannot narrow
  if (residentsById.has(residentId)) {
    // has() above guarantees the entry exists
    const resident = residentsById.get(residentId)!;
  }
  ```

## 3. Comment

### [TS-DOC-001] JSDoc for Functions
- **Severity**: REQUIRED
- **Scope**:
  - Exported functions that may be consumed by external modules or other packages.
  - Utility functions provided as a library, HTTP handlers, and public methods in the service layer.
- **Description**:
  - Add a JSDoc block to all functions within this scope.
  - The JSDoc block must include:
    - A brief summary of the function’s purpose and behavior (1–3 sentences).
    - The meaning of each parameter, its preconditions, and any important notes (`@param`).
    - The meaning of the return value, and conditions for errors or exceptions (`@returns`).
  - Types must be defined in the TypeScript function signature, and should not be duplicated in JSDoc in principle.
- **Exceptions**: JSDoc may be omitted for private functions and thin wrapper functions that are only used within the module, where the function name and type make the intent sufficiently clear.
- **Examples**:
  ```typescript
  // Good

  /**
   * Calculates the final billing amount after applying tax and discount rules.
   * @param input billing details (amount, tax category, discounts)
   * @returns final invoice amount, including tax
   */
  export function calculateBillingAmount(input: BillingInput): BillingAmount {
    // ...
  }

  // Good

  function isEmpty(value: string) {
    return value.trim().length === 0;
  }
  ```

### [TS-DOC-002] Comments should explain why, not repeat what the code does
- **Severity**: REQUIRED
- **Description**: Comments should explain why, not repeat what the code already says.
- **Examples**:
  ```typescript
  // Bad: repeats what the code does
  // Check if resident is inactive
  if (resident.status === 'inactive') return;

  // Good 👍 explains business reason
  // Inactive residents are kept for audit history and must not receive notifications.
  if (resident.status === 'inactive') return;
  ```

### [TS-DOC-003] TODO and FIXME comments
- **Severity**: RECOMMENDED
- **Description**: TODO/FIXME comments should include enough context to be actionable. If possible, include a ticket number or owner.
- **Examples**:
  ```typescript
  // Bad
  // TODO: fix this

  // Good 👍
  // TODO(BWV-1234): Remove this fallback after partner API v2 is fully migrated.
  const companyCode = input.companyCode ??  legacyCompanyCode;
  ```

## 4. Usage & Code Quality

### [TS-USAGE-001] Prefer native JavaScript/TypeScript APIs before adding utility libraries
- **Severity**: REQUIRED
- **Description**: Prefer native JavaScript/TypeScript APIs before adding utility libraries. Do not add lodash just to use basic map, filter, find, some, every or optional chaining. Use utility libraries only when they make the code clearly simpler or safer.
- **Examples**:
  ```typescript
  // Bad: unnecessary dependency for simple filtering
  import { filter } from 'lodash';
  const expensiveProducts = filter(products, (product) => product.price > 100);

  // Good 👍
  const expensiveProducts = products.filter((product) => product.price > 100);

  // Acceptable 👍 when it improves readability
  import groupBy from 'lodash/groupBy';
  const residentsByCompany = groupBy(residents, 'companyNo');
  ```
  
### [TS-USAGE-002] Type-Safe Comparisons
- **Severity**: REQUIRED
- **Description**: 
  - Use `===` instead of `==`, `!==` instead of `!=` for tight data comparisons.
  - When comparing two values, always ensure they are of the **same data type**. Convert both sides to a common type before comparison to avoid unexpected results (e.g., `'1' === 1` is `false`).
- **Exceptions**:
  - Use `value == null` or `value != null` only when intentionally treating both `null` and `undefined` as the same absence state. Keep this scoped to nullish checks, not general value comparison.
  - Use dedicated JavaScript APIs for edge cases where strict equality is not the correct semantic check, such as `Number.isNaN(value)` for `NaN` or `Object.is(a, b)` when `-0` must be distinguished from `0`.
- **Examples**:
  ```typescript
  const inputValue = '1'; // Value from request, DB, etc.

  // Bad - type mismatch (string vs number)
  const STATUS_ACTIVE = 1;
  if (inputValue === STATUS_ACTIVE) { ... } // '1' === 1 → false

  const VALID_IDS = [1, 2, 3, 4, 5];
  if (VALID_IDS.includes(inputValue)) { ... } // '1' not in [1,2,3,4,5]

  // Good 👍 - Convert to the SAME type before comparing
  // Option 1: Convert to number
  enum YesFlag { Yes = 1, No = 0 }
  if (Number(inputValue) === YesFlag.Yes) { ... }

  // Option 2: Convert to string
  const VALID_STATUSES = ['1', '2', '3'];
  if (VALID_STATUSES.includes(String(inputValue))) { ... }

  // Exception - nullish check
  // Allowed when both null and undefined mean "missing"
  if (optionalValue == null) {
    return defaultValue;
  }

  // Exception - NaN must be checked with Number.isNaN
  const score = Number(request.query.score);
  if (Number.isNaN(score)) {
    throw new Error('Invalid score');
  }

  // Exception - Object.is when the difference between 0 and -0 matters
  Object.is(-0, 0); // false
  ```

### [TS-LINT-001] No unused vars
- **Severity**: REQUIRED
- **Description**: Unused variables usually mean unfinished code, wrong refactor or wrong logic.
- **Examples**:
  ```typescript
  // Bad
  const limit = Number(req.query.limit);
  const page = Number(req.query.page);
  return { limit };

  // Good 👍
  const limit = Number(req.query.limit);
  const page = Number(req.query.page);
  return { page, limit };
  ```

### [TS-LINT-002] No console
- **Severity**: REQUIRED
- **Description**: Do not use console in application code (e.g., APIs, front-end applications, background jobs, and batch processes). Use the project logger instead. Utility and development scripts (e.g., migrations and seed scripts) may use console where appropriate.
- **Examples**:
  ```typescript
  // Bad
  console.log('Resident created', resident);

  // Good 👍
  logger.info({ residentNo: resident.residentNo }, 'Resident created');

  // Good: utility script (e.g., migration or seed)
  console.log('Resident seed completed');
  ```

### [TS-LINT-003] No debugger
- **Severity**: REQUIRED
- **Description**: Do not commit debugger statements.
- **Examples**:
  ```typescript
  // Bad
  function isTruthy(value: unknown) {
    debugger;
    return Boolean(value);
  }

  // Good 👍
  function isTruthy(value: unknown) {
    return Boolean(value);
  }
  ```

### [TS-LINT-004] No var and prefer const
- **Severity**: REQUIRED
- **Description**: Enable no-var and prefer-const. Use const unless reassignment is required.
- **Examples**:
  ```typescript
  // Bad
  var name = 'John';
  let limit = 50;

  // Good 👍
  const name = 'John';
  const limit = 50;
  ```

### [TS-LINT-005] No Explicit Any
- **Severity**: RECOMMENDED
- **Description**: Avoid abusing any. Prefer specific types or unknown. any is acceptable only for exceptional cases, such as legacy untyped libraries, with a clear comment.
- **Examples**:
  ```typescript
  // Bad
  const age: any = 'seventeen';

  // Good 👍 - specific type
  const age: number = 17;

  // Good 👍 - 'unknown' when type is uncertain, narrow before use
  function parsePayload(raw: unknown) { ... }

  // Acceptable with reason 👍
  // eslint-disable-next-line @typescript-eslint/ no-explicit-any -- legacy SDK has no type definitions
  const legacyClient: any = require('legacy-untyped-sdk');
  ```

### [TS-LINT-006] Importing specific functions
- **Severity**: RECOMMENDED
- **Description**: Import only what you need to reduce bundle size.
- **Examples**:
  ```typescript
  // This way can help reduce the overall size of your bundle

  import * as _ from 'lodash'; // <-- Bad

  import { get , set } from 'lodash' // <-- Good 👍

  import get from 'lodash/get' // <-- Best way but may not always be practical 👍
  ```

### [TS-LINT-007] No floating promises
- **Severity**: REQUIRED
- **Description**: Do not start async work without handling errors.
- **Examples**:
  ```typescript
  // Bad
  sendWelcomeEmail(user.email);

  // Good 👍
  await sendWelcomeEmail(user.email);

  // Good 👍 explicitly ignore only with error handling
  void sendAuditLog(event).catch((error) => {
    logger.error({ error }, 'Failed to send audit log');
  });
  ```

### [TS-LINT-008] Function complexity
- **Severity**: RECOMMENDED
- **Description**: Do not judge a function by line count alone. Flag functions that are intellectually difficult to scan. For long functions (more than 50 lines), consider extracting sub-routines with descriptive names.
- **Examples**:
  ```typescript
  // Bad: validation, persistence and notification are mixed together
  async function registerUser(input: RegisterUserInput) {
    // More than 50 lines of validation, data mapping, persistence and email logic...
  }

  // Good: 👍 each step communicates its purpose
  async function registerUser(input: RegisterUserInput) {
    validateRegistration(input);
    const user = await createUser(input);
    await sendWelcomeEmail(user);
    return user;
  }
  ```

### [TS-LINT-009] Parameter object pattern
- **Severity**: RECOMMENDED
- **Description**: Functions with more than three arguments must use a typed parameter object and object destructuring. This makes call sites easier to read and future changes safer.
- **Examples**:
  ```typescript
  // Bad
  function createUser(name: string, email: string, role: Role, companyId: string) {}

  // Good 👍
  interface CreateUserParams {
    name: string;
    email: string;
    role: Role;
    companyId: string;
  }

  function createUser({ name, email, role, companyId }: CreateUserParams) {}
  ```

### [TS-LINT-010] Early returns and guard clauses
- **Severity**: RECOMMENDED
- **Description**: Flatten nesting deeper than three levels. Invert conditions and return early instead of wrapping the main logic in large else blocks.
- **Examples**:
  ```typescript
  // Bad
  function publish(post: Post) {
    if (post.isValid) {
      if (post.author.isActive) {
        return postRepository.publish(post);
      }
    }
    return undefined;
  }

  // Good
  function publish(post: Post) {
    if (!post.isValid) return undefined;
    if (!post.author.isActive) return undefined;

    return postRepository.publish(post);
  }
  ```

## 5. Security

### [SEC-DB-001] Parameterized Queries (SQL Injection)
- **Severity**: CRITICAL
- **Description**: Use parameterized queries or ORM query builders that bind parameters safely. Never concatenate user input directly into SQL.
- **Examples**:
  ```typescript
  // Bad
  const query = `SELECT * FROM users WHERE id = ${userId}`;

  // Good 👍 parameterized query
  const query = 'SELECT * FROM users WHERE id = ?';
  await db.query(query, [userId]);

  // Good 👍 ORM/query builder example
  await db
    .select()
    .from(users)
    .where(eq(users.id, userId));
  ```

### [SEC-API-001] Implement rate limiting
- **Severity**: RECOMMENDED
- **Description**: Implement rate limiting to prevent brute force attacks and other forms of abuse.
However, depend on your project large and the original design, you can choose apply this spec or not.
- **Examples**:
  ```typescript
  import rateLimit from 'express-rate-limit';

  const loginLimiter = rateLimit({
    windowMs: 60 * 1000,
    max: 10,
    standardHeaders: true,
    legacyHeaders: false,
  });

  app.post('/login', loginLimiter, loginController.login);
  ```

### [SEC-LOG-001] Structured Logging
- **Severity**: RECOMMENDED
- **Description**: Use structured logging (e.g., Winston) instead of standard output. Log to files/streams, not just console.
- **Examples**:
  ```typescript
  // Bad example: no logging or monitoring
  app.post("/data", (req, res) => {
    //store data in database
  });

  // Good example: implementing logging and monitoring 👍
  const winston = require("winston");
  const logger = winston.createLogger({
    level: "info",
    format: winston.format.json(),
    defaultMeta: { service: "my-app" },
    transports: [
      new winston.transports.File({ filename: "error.log", level: "error" }),
      new winston.transports.File({ filename: "combined.log" })
    ]
  });
  app.post("/data", (req, res) => {
    logger.info("Data received", { payload: req.body });
    // store data in database
  });
  ```

### [SEC-XSS-001] Escape HTML (XSS)
- **Severity**: CRITICAL
- **Description**: Use context-aware output encoding to prevent XSS. Do not manually insert untrusted input into HTML, JavaScript, CSS or URLs. When rendering user-provided rich text, sanitize it with an approved sanitizer.
- **Examples**:
  ```typescript
  // Bad
  res.send(`<div>${req.query.name}</div>`);

  // Better 👍 template engines/frameworks usually escape by default
  res.render('profile', {
    name: user.name,
  });

  // If HTML is intentionally allowed, sanitize it first.
  const safeHtml = sanitizeHtml(userProvidedHtml);
  ```

### [SEC-FILE-001] No User Input in File Paths
- **Severity**: CRITICAL
- **Description**: Avoid using direct user input for file paths, redirects, outbound URLs or shell commands. Use IDs, allowlists and safe mapping functions.
- **Examples**:
  ```typescript
  // Bad: open redirect
  res.redirect(req.body.redirectUrl);

  // Good 👍
  const redirectUrl = getAllowedRedirectUrl(req.body. redirectUrlId);
  res.redirect(redirectUrl);

  // Bad: path traversal risk
  const file = await fs.readFile(req.body.filePath);

  // Good 👍
  const file = await fileStorage.readById(req.body.fileId, currentUser.companyNo);
  ```

### [SEC-DATA-001] Hash passwords, do not encrypt them
- **Severity**: CRITICAL
- **Description**: Hash passwords. Do not encrypt passwords with reversible encryption. Use a strong password hashing algorithm such as Argon2id, bcrypt or PBKDF2 depending on project requirements.
- **Examples**:
  ```typescript
  import bcrypt from 'bcrypt';

  const SALT_ROUNDS = 12;

  export async function hashPassword(password: string): Promise<string> {
    return bcrypt.hash(password, SALT_ROUNDS);
  }

  export async function verifyPassword(
    password: string,
    passwordHash: string,
  ): Promise<boolean> {
    return bcrypt.compare(password, passwordHash);
  }
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