# RADProgrammer Delphi Style Guide
- Source: https://github.com/radprogrammer/delphi-project-standards/style-guide.md
- Version: 2.3

## Changelog
- 2.3 (2026-02-22): Expanded spacing and indentation rules; added explicit begin/end and if statement rules; added assertions policy; discouraged multiple declarations on one line
- 2.2 (2026-02-22): Added Project Structure Conventions section
- 2.1 (2026-02-20): Added source file conventions (CRLF, ASCII), constants typing rules, magic number rules, Cardinal/Int64 safety rules, platform unit guidance
- 2.0 (2026-02-20): Converted to markdown instruction file for AI Agents
- 1.0 (2021-03-14): radteam wiki initial release

---

# Delphi Coding Standards

The initial purpose is for Claude Code instructions, imported via `@.delphi/style-guide.md` in the project's `CLAUDE.md`.
AI Agents should apply these conventions to all generated and modified Delphi code unless a project-level `CLAUDE.md` explicitly overrides a rule.

---

## Naming

All identifiers use PascalCase. No underscores. No Hungarian notation.

| Identifier            | Prefix / Suffix      | Example                         |
|-----------------------|----------------------|---------------------------------|
| Class                 | `T`                  | `TMyClass`                      |
| Exception class       | `E`                  | `EInvalidOp`                    |
| Interface             | `I`                  | `IMyInterface`                  |
| Field                 | `F`                  | `FUserName`                     |
| Pointer type          | `P`                  | `PInteger`                      |
| Method parameter      | `A` (recommended)    | `AUserName`                     |
| Event handler         | `On`                 | `OnMyExampleEvent`              |
| Event executor        | `Do`                 | `DoMyExampleEvent`              |
| [Class constructor](https://docwiki.embarcadero.com/RADStudio/en/Methods_(Delphi)#Class_Constructors) | `CreateClass` | `class constructor CreateClass` |
| Class destructor      | `DestroyClass`       | `class destructor DestroyClass` |
| Test class            | suffix `Test`        | `TMyClassTest`                  |
| Versioned interface   | suffix `2`, `3`...   | `IMyInterface2`                 |
| Custom Attributes     | end with `Attribute` | `MyExampleAttribute`            |

### Additional Naming Rules

- **Enums:** PascalCase, no prefix. Prefer fully qualified references everywhere. Bare names are acceptable within the declaring unit but should be avoided for consistency.
- **Constants:** If inside a class, no prefix — reference fully qualified externally. If outside a class, use UPPERCASE.   Constants should be class-scoped where possible.
- **Booleans:** Positive naming only, using `is`, `has`, `can`, or `should` prefixes (e.g. `IsActive`, not `IsNotActive`).
- **Methods:** verb+noun form (`CalculateTotal`, `ValidateInput`).
- **Form components:** Descriptive suffix-based names (`DeleteFileButton`, not `Button1` or `butDelete`).
- **Arrays/Lists:** Pluralized names; use singular form for the iteration variable.

### Event Handler vs Event Executor

- `OnButtonClick` — the published event property; wired to an external handler.
- `DoButtonClick` — the `virtual` method containing the actual logic; called by the event machinery.
- All `Do`-prefix event executor methods must be declared `virtual` to allow subclass override of logic without replacing event wiring.

---

## Source File Conventions

- **Line endings:** All Delphi source files use CRLF (`\r\n`) line endings.
- **Encoding:** UTF-8 without BOM, ASCII characters only in source code. AI agents must not use Unicode characters (em dashes, curly quotes, ellipsis, arrows, emoji, or other non-ASCII characters) in identifiers, string literals, or comments unless the feature explicitly requires them — do not substitute fancy Unicode punctuation for plain ASCII equivalents.
- **Default parameter values** must be repeated verbatim in the implementation section, matching the interface declaration exactly.

---

## Spacing and Indentation

- **Indentation unit:** 2 spaces per level. Never use tabs. The Delphi IDE converts tabs to spaces by default -- leave this setting on.
- **No space** before the colon in a variable or parameter declaration:
  ```delphi
  MyInteger: Integer;   // correct
  MyInteger : Integer;  // incorrect
  ```
- **No space** before the opening parenthesis of a function or procedure call or declaration:
  ```delphi
  MyFunc(42);                        // correct
  procedure DoSomething(A: Integer); // correct
  MyFunc (42);                       // incorrect
  ```
- **No space** inside parentheses or square brackets:
  ```delphi
  MyFunc(A, B);    // correct
  MyArray[5];      // correct
  MyFunc( A, B );  // incorrect
  MyArray[ 5 ];    // incorrect
  ```
- **Space after** commas and semicolons in parameter lists.
- **Spaces before and after** the `:=` assignment operator and all binary operators.
- **Line length:** aim to keep lines within screen width to avoid horizontal scrolling. 120 columns is a practical upper bound for generated code.

---

## begin / end and Block Formatting

- **`begin` and `end` always on their own lines.** Never place `begin` on the same line as `then`, `do`, or `else`:
  ```delphi
  // Correct
  if A < B then
  begin
    DoSomething;
  end
  else
  begin
    DoThat;
  end;

  // Incorrect
  if A < B then begin
    DoSomething;
  end else begin
    DoThat;
  end;
  ```
- Each line contains at most one statement.

---

## if Statements

- **Always span at least two lines.** Never place the statement on the same line as the condition:
  ```delphi
  // Correct
  if A < B then
    DoSomething;

  // Incorrect
  if A < B then DoSomething;
  ```
- **No extra parentheses** around boolean conditions:
  ```delphi
  if A < B then    // correct
  if (A < B) then  // incorrect
  ```
- **`else if` stays on one line** -- no line break between `else` and `if`:
  ```delphi
  if Condition1 then
    DoThis
  else if Condition2 then
    DoThat;
  ```

---

## Source File Layout Order

Most of your Delphi code will reside in .pas source files. These files have a few requirements determined by the compiler and also some preferences dictated by this style guide.

1. Comment header (description, copyright, history)
2. `interface` section — one class per unit; paired classes are the exception
3. Class body order: `const` -> `Fields` -> `Methods` -> `Properties`, grouped by visibility
4. Declaration block order: `const` -> `type` -> `var` -> methods
5. `implementation` section: instance `constructor` and `destructor` listed first
6. `class constructor CreateClass` and `class destructor DestroyClass` implementations at the bottom of the `implementation` section, above any `initialization`/`finalization` blocks
7. `initialization` / `finalization` always last

---

## Avoid these language items
- `with` statements
- `goto` statements
- Inline variables (`var x := ...` in Delphi 10.3+)
- Multi-line string literals (Delphi 12+)
- `System.DateUtils`: `YearsBetween`, `MonthsBetween`, `YearSpan`, `MonthSpan`

---

## Project Structure Conventions

- Every `.dpr` project must live in its own dedicated directory.
- Each project directory must contain a `readme.md` serving as its entry-point documentation.
- The corresponding `.dproj` must reference the readme.md via the `WelcomePageFile`
  element so it is displayed when the project is opened within the IDE:
  ```xml
  <Delphi.Personality>
    <Source>
      <Source Name="MainSource">MyProject.dpr</Source>
    </Source>
    <WelcomePageFile Path="readme.md"/>
  </Delphi.Personality>
  ```
  This can also be set via `Project -> Project Page Options... -> Project page`
  within the IDE.

---

## Version Control

- Refactoring-only changes must be committed separately from logic changes.
- Tests must pass after any refactoring commit before logic changes are introduced.

---

## Code Rules

- Access fields via properties, not directly. Exception: inside the owning class constructor.
- Use `const` for parameters of managed types (strings, interfaces) that are not modified. For unmanaged types, const is not required but is recommended when it improves clarity about intent.
- Use guard clauses with `Exit` to reduce nesting. Use `Exit` sparingly outside of guard clauses.
- `case` statements on enums must list all values explicitly. Use `Assert(False, 'Unhandled enum value')` in the `else` branch.
- Functions that perform operations which can fail must return an enumerated result type rather than `Boolean`.
- `uses` clause order: RTL -> System Library -> VCL/FMX -> Third Party -> Shared -> Project-specific.
- Add units to the `implementation` uses clause unless they are required in the `interface`.
- The **Unit Scope Names** project setting should be empty. All unit references should be fully qualified (e.g. `System.SysUtils`, not `SysUtils`). Auto-generated and third-party code are exempt.
- Do not declare multiple identifiers of the same type on a single line. Each declaration belongs on its own line. Multiple declarations on one line make version control diffs harder to read and review:
  ```delphi
  // Correct
  var
    MyData: Integer;
    MyString: string;

  // Incorrect
  var
    MyData, MyOtherData: Integer;
  ```

### Assertions

- Use assertions to test logical invariants -- conditions that must always be true regardless of input or execution state, such as parameter preconditions and object invariants.
- Always include an explanatory string describing which expected condition is failing:
  ```delphi
  Assert(AValue > 0, 'AValue must be positive');
  ```
- Do not use assertions to test conditions that depend on program execution, user input, OS configuration, or any general error condition. Use exceptions for those cases.
- Assertions may be disabled in release builds -- never rely on them for runtime error handling in production code.

### Constants

- Magic numbers in code must be replaced with named constants. Each named constant should include a comment explaining why that specific value was chosen over alternatives.
- Constants should always be explicitly typed **except** when used as a default parameter value, in which case they must be untyped. Add a comment at the declaration explaining the exception:
  ```delphi
  const
    MAX_RETRIES = 3;  // Untyped -- required because used as a default parameter value.
                      // Delphi requires default parameter values to be constant expressions;
                      // typed constants do not satisfy this requirement.
  ```

### Integer and Timing Arithmetic

- `TStopwatch` is the preferred timing mechanism — it is monotonic and has no wraparound. Never use `GetTickCount` or `TThread.GetTickCount` for elapsed time measurement.
- Never perform arithmetic on `TStopwatch.ElapsedMilliseconds` (which is `Int64`) using `Cardinal` variables. Perform all arithmetic in `Int64` space and only narrow to `Cardinal` at the final assignment, after a guard confirms the value fits:
  ```delphi
  Elapsed := Sw.ElapsedMilliseconds;       // Int64
  if Elapsed >= MaxMs then Exit(False);
  Remaining := Cardinal(MaxMs - Elapsed);  // safe: Elapsed < MaxMs guaranteed
  ```
- `Cardinal` subtraction can underflow silently (or raise `EIntOverflow` with overflow checking enabled). Always guard before subtracting two `Cardinal` values.

### Platform Targeting

- Do not use platform-specific units (e.g. `Winapi.*`) unless the feature explicitly requires them. Code should target all RTL-supported platforms unless the project `CLAUDE.md` states otherwise.

---
