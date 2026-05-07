# Coding Standards Reference

Source: *Programming Guidelines for NPARC Alliance Software Development*, v2.0, Charles E. Towne, NASA Glenn Research Center. Universal principles extracted and adapted for Ruby, TypeScript, JavaScript, Python, and Dart.

---

## UNIVERSAL RULES

These apply to all five languages.

### Organization

- One primary class or logical unit per file; filename reflects what it contains
- Shared constants and config in dedicated files — not scattered across modules
- Group related files in directories whose names reflect their purpose
- Imports/requires organised: stdlib → third-party packages → local files

### Functions & Modules

- Functions should be pure where possible — no hidden side effects on shared state
- Standard order within each unit: header comment → imports → constants → declarations → executable code
- Keep functions short enough that their full body is visible without scrolling

### Naming

- Names must be descriptive; don't abbreviate unless the abbreviation is universally understood in the domain
- Do not shadow outer-scope names with local variables
- Do not reuse language keywords or built-in names as identifiers
- `UPPER_CASE` for named constants in all five languages

### Line Length & Statement Form

- Lines ≤100 characters (match project linter config; flag anything over 120 regardless)
- One statement per line
- Consistent indentation — spaces only, no tabs (except where the language mandates otherwise)

### Constants & Magic Numbers

- No bare magic numbers or strings in logic — name them
- Keep constant definitions in a module-level or shared constants file, not inline

### Control Flow

- Prefer guard clauses / early returns over deeply nested conditionals
- Nesting deeper than 3 levels is a flag — consider extracting a function
- Long blocks (>~50 lines) should have a closing comment echoing the opening condition

### Comments

- Explain *why*, not *what* — don't restate what well-named code already says
- Every public function needs a header comment: purpose, parameters, return value
- Use consistent section-header style throughout a file
- Inline comments sit at least 2 spaces after the statement; align vertically on related lines

### Resource Management

- Close file handles, network connections, and streams explicitly — use language-idiomatic patterns (blocks, `with`, `using`, `finally`) rather than relying on GC
- Release any resource you acquire in the same logical scope where possible

### Input/Output

- Always handle I/O errors explicitly; never silently swallow exceptions
- Don't hard-code file paths or environment-specific strings — use config or constants
- Validate all external input at the boundary; trust nothing from outside the process

### Forbidden Patterns (all languages)

| Pattern | Why |
|---|---|
| Magic numbers / unnamed constants | Brittle, unclear intent |
| Shadowing outer-scope variables | Confusing, error-prone |
| Functions with hidden side effects on globals | Hard to test and reason about |
| Silently swallowing exceptions (`rescue nil`, bare `except:`, `catch {}`) | Masks bugs |
| Unreachable / dead code | Misleads readers |
| Deeply nested control flow (>3 levels) | Hard to follow; extract a function |
| Mutable global state | Causes unpredictable behaviour across call sites |

---

## LANGUAGE-SPECIFIC RULES

### Ruby

**Package management**
- Dependencies declared in `Gemfile`; always commit `Gemfile.lock`
- Use Bundler (`bundle exec`) to run anything — never rely on system gems
- Pin versions for production apps; use pessimistic constraints (`~>`) for libraries

**Organization**
- One class or module per file; filename is `snake_case` matching the constant name (e.g. `MyClass` → `my_class.rb`)
- Use `require_relative` for local files, `require` for gems
- Avoid `require_all` or directory-level requires that load everything implicitly

**Naming**

| Kind | Convention |
|---|---|
| Constants / modules / classes | `PascalCase` |
| Methods / variables | `snake_case` |
| Predicate methods | `snake_case?` (must return boolean) |
| Mutating / dangerous methods | `snake_case!` |
| Private / internal | Prefix with `_` only when disambiguation is needed; prefer `private` keyword |

**Typing**
- Ruby is dynamically typed; use Sorbet (`sig`) or RBS annotations on public interfaces in larger codebases
- Avoid relying on implicit truthiness for non-boolean values in conditionals — be explicit

**Resource management**
- Always use block form for IO: `File.open(...) { |f| ... }` — this guarantees `close`
- Use `ensure` for cleanup in any method that acquires a resource

**Forbidden**
- `eval`, `send` with user-supplied input (security risk)
- `rescue Exception` — rescue `StandardError` or more specific classes only
- Modifying built-in classes (monkey-patching) outside of a clearly scoped and documented module

---

### TypeScript

**Package management**
- Dependencies in `package.json`; always commit the lockfile (`package-lock.json`, `yarn.lock`, or `pnpm-lock.yaml`)
- Separate `dependencies` (runtime) from `devDependencies` (build/test only) — don't blur the line
- Don't commit `node_modules`

**Organization**
- One exported class or tightly related group of functions per file
- Use `index.ts` barrel files to re-export a directory's public surface — but don't create deep barrel chains
- Prefer named exports over default exports (easier to refactor, better IDE support)

**Naming**

| Kind | Convention |
|---|---|
| Constants | `UPPER_CASE` |
| Types / interfaces / classes / enums | `PascalCase` |
| Functions / variables / parameters | `camelCase` |
| Private class members | `_camelCase` or `#camelCase` (private field) |

**Typing**
- `strict: true` in `tsconfig.json` — non-negotiable
- No `any`; use `unknown` when the type is genuinely unknown and narrow it explicitly
- Avoid type assertions (`as SomeType`) except at verified boundaries; never use non-null assertion (`!`) without a comment explaining why it's safe
- Prefer `interface` for object shapes that may be extended; `type` for unions, intersections, and aliases

**Imports**
- Order: Node built-ins → third-party packages → local files; blank line between groups
- Use `import type` for type-only imports

**Forbidden**
- `var` — use `const` by default, `let` only when the binding must be reassigned
- `any` without a `// eslint-disable` comment and justification
- `@ts-ignore` — use `@ts-expect-error` with a comment explaining what's expected and why

---

### JavaScript

**Package management**
- Same as TypeScript: `package.json` + committed lockfile; no `node_modules` in version control
- Use ESM (`import`/`export`) over CommonJS (`require`/`module.exports`) in new code

**Organization**
- One primary export per file; filename in `kebab-case`
- Avoid implicit globals — always use strict mode (`'use strict'` or ESM, which is strict by default)

**Naming**

| Kind | Convention |
|---|---|
| Constants | `UPPER_CASE` |
| Classes | `PascalCase` |
| Functions / variables | `camelCase` |

**Typing**
- JavaScript has no static types; compensate with JSDoc (`@param`, `@returns`, `@type`) on all public functions
- Use a linter (ESLint) to catch implicit coercions and common pitfalls

**Forbidden**
- `var` — use `const` or `let`
- `==` — always use `===`
- Implicit type coercion in conditionals (e.g. `if (someNumber)` when you mean `if (someNumber !== 0)`) — be explicit
- `with` statements
- Modifying `Object.prototype` or `Array.prototype`

---

### Python

**Package management**
- Use a virtual environment for every project — never install project dependencies globally
- Declare dependencies in `pyproject.toml` (preferred) or `requirements.txt`; pin versions for applications, use ranges for libraries
- Commit lockfiles (`poetry.lock`, `uv.lock`) for applications; not for libraries
- Preferred tools (in order): `uv` > `poetry` > `pip` + `venv`

**Organization**
- One class or tightly related group of functions per module; filename is `snake_case`
- Use `__init__.py` to define the public API of a package — re-export only what callers should use
- Never use `from module import *` — it pollutes the namespace silently

**Naming**

| Kind | Convention |
|---|---|
| Constants | `UPPER_CASE` |
| Classes | `PascalCase` |
| Functions / variables / modules | `snake_case` |
| Internal / private | `_single_leading_underscore` |
| Name-mangled (class-private) | `__double_leading_underscore` |

**Typing**
- Add type hints (PEP 484) to all function signatures and class attributes in new code
- Use `from __future__ import annotations` at the top of files with forward references
- Run `mypy` (or `pyright`) in strict mode as part of CI
- Prefer `X | None` over `Optional[X]` (Python 3.10+)

**Imports**
- Order: stdlib → third-party → local; blank line between groups (enforced by `isort`)
- Use absolute imports; relative imports only within a package and only one level deep

**Forbidden**
- Bare `except:` — always name the exception class
- Mutable default arguments (`def f(x=[])`) — use `None` and assign inside the function
- `from module import *`
- Using `type: ignore` without a comment explaining why

---

### Dart

**Package management**
- Dependencies in `pubspec.yaml`; always commit `pubspec.lock` for applications (not for published packages)
- Separate `dependencies` from `dev_dependencies`
- Run `dart pub get` (or `flutter pub get`) after any `pubspec.yaml` change — don't commit without a fresh lock

**Organization**
- One class per file; filename is `snake_case` matching the class name (e.g. `UserProfile` → `user_profile.dart`)
- Use `library` and `part`/`part of` sparingly — prefer independent files with explicit imports
- Organise imports: `dart:` → `package:` → relative; use `show`/`hide` to limit what's imported

**Naming**

| Kind | Convention |
|---|---|
| Constants (`const`) | `lowerCamelCase` (Dart convention, not `UPPER_CASE`) |
| Classes / enums / typedefs | `PascalCase` |
| Variables / functions / parameters | `lowerCamelCase` |
| Libraries / files | `snake_case` |
| Private members | `_lowerCamelCase` (leading underscore = library-private) |

**Typing & null safety**
- Dart has sound null safety — use it fully; no `// ignore: null_safety` suppressions
- Mark types nullable (`T?`) only when `null` is a genuinely valid value
- Avoid the null assertion operator (`!`) unless you are certain the value cannot be null at that point — add a comment if you use it
- Prefer `??` (null coalescing) and `?.` (null-aware access) over explicit null checks

**Async**
- Always `await` Futures that must complete before continuing — don't fire-and-forget unless intentional (and comment it)
- Prefer `async`/`await` over raw `.then()`/`.catchError()` chains
- Handle errors in async code with `try`/`catch`, not `.catchError()`

**Forbidden**
- `dynamic` — use a specific type or `Object?`; treat `dynamic` like `any` in TypeScript
- Null assertion (`!`) without a comment
- Ignoring `unawaited` Futures silently — wrap intentional fire-and-forget in `unawaited()`
- `part`/`part of` for code organisation (only use for generated code)
