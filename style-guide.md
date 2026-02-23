# RADProgrammer Delphi Style Guide
- Source: https://github.com/radprogrammer/delphi-project-standards
- Last updated: 2026-02-22

These rules closely match Embarcadero's published style guidelines:
https://docwiki.embarcadero.com/RADStudio/en/Delphi%E2%80%99s_Object_Pascal_Style_Guide

Noted differences:
- We use `UPPERCASE` for constants outside classes to preserve historical convention and flag them as candidates for encapsulation. Embarcadero uses `PascalCase` for all constants.
- We explicitly prohibit `with` and `goto`. Embarcadero does not address these.
- We recommend using multi-line string literals sparingly due to tooling maturity. Embarcadero does not address this.

For indentation, spacing, block formatting, and line break rules see `code-formatting-guide.md`.

---

# Delphi Coding Standards

Imported via `@.delphi/style-guide.md` in the project's `CLAUDE.md`.
AI Agents should apply these conventions to all generated and modified Delphi code
unless a project-level `CLAUDE.md` explicitly overrides a rule. For whole-file
reformatting of existing code, prefer dedicated formatting tools over manual edits.

---

## How Rules Are Presented

Numbered rules are checkable constraints an agent can evaluate from visible code alone.
They carry a severity:
- **Fail** — objective and unambiguous. Must not be violated.
- **Warn** — strongly preferred. Flag violations but do not block.

**Guidance** blocks cover contextual preferences, rationale, and advice that require
judgment rather than mechanical checking. No rule number is assigned.

---

## Naming

### STY-001 (Fail): PascalCase for all identifiers
All identifiers must use PascalCase. No underscores between words. No Hungarian notation.
Exception: constants declared outside a class use UPPERCASE -- see STY-010.

```delphi
MyClassName    // correct
my_class_name  // incorrect
lpstrMyName    // incorrect
```

### STY-002 (Fail): Class names prefix T
All class type declarations must begin with `T`.

```delphi
TMyClass  // correct
MyClass   // incorrect
```

### STY-003 (Fail): Interface names prefix I
All interface type declarations must begin with `I`.

```delphi
IMyInterface  // correct
MyInterface   // incorrect
```

### STY-004 (Fail): Exception class names prefix E
All exception class declarations must begin with `E`.

```delphi
EInvalidOp  // correct
InvalidOp   // incorrect
```

### STY-005 (Fail): Field names prefix F
All class and record field declarations must begin with `F`.

```delphi
FUserName  // correct
UserName   // incorrect
```

### STY-006 (Fail): Pointer type names prefix P
All pointer type declarations must begin with `P`.

```delphi
PMyRecord   // correct
MyRecordPtr // incorrect
```

### STY-007 (Warn): Method parameter prefix A
Method parameters should begin with `A` to distinguish them from fields and locals.

```delphi
procedure SetName(AName: string);  // recommended
procedure SetName(Name: string);   // acceptable
```

### STY-008 (Fail): Language keywords, reserved words, and language directives are lowercase
All language keywords and reserved words must be lowercase. Compiler switch directives
in `{$...}` are UPPERCASE -- see FMT-046.

```delphi
begin end if then else abstract virtual override  // correct
Begin End If Then Else Abstract Virtual Override  // incorrect
```

### STY-009 (Warn): Boolean identifier positive naming
Boolean-returning functions, properties, and variables should begin with `Is`, `Has`,
`Can`, or `Should`. Always use positive form -- never negate in the name itself.

```delphi
IsActive      // correct
HasChildren   // correct
Active        // warn -- missing prefix
IsNotActive   // incorrect -- negated form
```

### STY-010 (Fail): Constants outside classes use UPPERCASE
Constants declared outside a class must use UPPERCASE with underscores between words.
Constants declared inside a class use PascalCase and are referenced fully qualified.

```delphi
const
  MAX_RETRY_COUNT: Integer = 3;  // correct -- outside class

type
  TMyClass = class
  const
    DefaultTimeout: Integer = 30;  // correct -- inside class
  end;
```

Guidance: Prefer class-scoped constants over unit-level constants. UPPERCASE outside a
class is a deliberate signal that the constant is a candidate for encapsulation.

### STY-011 (Warn): Event handler prefix On
Published event properties must begin with `On`.

```delphi
OnButtonClick  // correct
ButtonClick    // warn
```

### STY-012 (Fail): Event executor prefix Do, declared virtual
Methods that implement event logic must begin with `Do` and be declared `virtual`.
This allows subclasses to override logic without replacing event wiring.

```delphi
procedure DoButtonClick; virtual;  // correct
procedure ButtonClickHandler;      // incorrect
```

### STY-013 (Fail): Class constructor named CreateClass
Class constructors must use the name `CreateClass`.
See: https://docwiki.embarcadero.com/RADStudio/en/Methods_(Delphi)#Class_Constructors

```delphi
class constructor CreateClass;  // correct
class constructor Create;       // incorrect
```

### STY-014 (Fail): Class destructor named DestroyClass
Class destructors must use the name `DestroyClass`.

```delphi
class destructor DestroyClass;  // correct
class destructor Destroy;       // incorrect
```

### STY-015 (Fail): Test fixture class suffix Tests
DUnitX test fixture classes must end with `Tests` for clarity in multi-unit test suites.

```delphi
TMyClassTests  // correct
TMyClassTest   // incorrect
TTestMyClass   // incorrect
```

---

Guidance — additional naming conventions (contextual, not mechanically checkable):
- **Enums:** PascalCase, no prefix. Prefer fully qualified references to avoid ambiguity in
  large units. Bare names are acceptable when scope is unambiguous.
- **Methods:** verb+noun form: `CalculateTotal`, `ValidateInput`.
- **Form components:** descriptive suffix-based names: `DeleteFileButton` not `Button1`.
- **Arrays/Lists:** pluralized names; singular form for the iteration variable.
- **Versioned interfaces:** append `2`, `3`... to the base name: `IMyInterface2`.
- **Custom Attributes:** end with `Attribute`: `MyExampleAttribute`.

---

## Source File Conventions

### STY-016 (Fail): CRLF line endings
All Delphi source files must use CRLF (`\r\n`) line endings.

### STY-017 (Fail): ASCII only in source, UTF-8 without BOM encoding
Source files must be saved as UTF-8 without BOM. Source file content (identifiers,
string literals, and comments) must contain only ASCII characters. Do not substitute
Unicode punctuation (em dashes, curly quotes, ellipsis, arrows, emoji) for plain ASCII
equivalents.

```delphi
// correct  -- use double dash for emphasis
// incorrect -- do not use em dash
```

### STY-018 (Fail): Default parameter values repeated in implementation
Default parameter values must be repeated verbatim in the implementation section,
matching the interface declaration exactly.

```delphi
// interface
function Fetch(ATimeout: Cardinal = 5000): Boolean;

// correct implementation
function TMyClass.Fetch(ATimeout: Cardinal = 5000): Boolean;

// incorrect implementation
function TMyClass.Fetch(ATimeout: Cardinal): Boolean;
```

---

## Language Restrictions

### STY-019 (Fail): with statements forbidden
`with` statements are prohibited. They obscure scope, create ambiguous identifiers,
and make refactoring unreliable.

```delphi
// correct
Canvas.Brush.Color := clRed;
Canvas.FillRect(Rect);

// incorrect
with Canvas do
begin
  Brush.Color := clRed;
  FillRect(Rect);
end;
```

### STY-020 (Fail): goto statements forbidden
`goto` statements are prohibited without exception.

### STY-021 (Fail): Prohibited DateUtils functions
The following `System.DateUtils` functions must not be used -- they return imprecise
floating-point approximations and produce non-calendar-accurate results, especially
around month and year boundaries:
- `YearsBetween`
- `MonthsBetween`
- `YearSpan`
- `MonthSpan`

Use calendar arithmetic via `TDateTime` decomposition or `System.DateUtils` functions
that operate on whole days (`DaysBetween`, `DayOf`, etc.) instead.

```delphi
// incorrect
Age := YearsBetween(BirthDate, Today);

// correct
Age := YearOf(Today) - YearOf(BirthDate);
// note: adjust for whether the birthday has occurred yet this year
```

### STY-022 (Warn): Multi-line string literals used sparingly
Multi-line string literals (Delphi 12+) are permitted but should be used sparingly.
Tooling support (search, diff, static analysis) remains uneven. Prefer conventional
string concatenation for content that must be searchable or machine-processed.

---

## Declarations

### STY-023 (Fail): One declaration per line
Each variable or constant declaration must appear on its own line. Multiple identifiers
on one line make version control diffs harder to read and review.

```delphi
// correct
var
  MyData: Integer;
  MyString: string;

// incorrect
var
  MyData, MyOtherData: Integer;
```

### STY-024 (Fail): Fully qualified unit references
All unit references must use fully qualified names. The **Unit Scope Names** project
setting must be empty. Auto-generated and third-party code are exempt.

```delphi
uses System.SysUtils;  // correct
uses SysUtils;         // incorrect
```

### STY-025 (Fail): Units in implementation uses unless required in interface
Add units to the `implementation` uses clause unless any declaration in the `interface`
section requires it -- this includes types, constants, variables, and procedure
signatures that reference identifiers from that unit.

---

## Constants

### STY-026 (Fail): Untyped constants when used as default parameter values
Constants used as default parameter values must be untyped. Add a comment explaining why.

```delphi
const
  DEFAULT_TIMEOUT = 5000;  // Untyped -- required as default parameter value.
                            // Typed constants do not satisfy Delphi's constant
                            // expression requirement for default parameters.
```

### STY-027 (Fail): Named constants replace magic numbers
Magic numbers must be replaced with named constants. Each constant must include a comment
explaining why that specific value was chosen.

```delphi
const
  MAX_RETRY_COUNT: Integer = 3;  // Three retries balances reliability and latency.
```

### STY-028 (Fail): Explicitly typed constants except default parameter values
Constants must always be explicitly typed, except when used as a default parameter value
(see STY-026).

---

## Integer and Timing Arithmetic

### STY-029 (Fail): TStopwatch for elapsed time measurement
Use `TStopwatch` for all elapsed time measurement. Never use `GetTickCount` or
`TThread.GetTickCount` -- they are not monotonic and wrap after ~49 days.

### STY-030 (Fail): Int64 arithmetic for TStopwatch values
Never cast `TStopwatch.ElapsedMilliseconds` (which is `Int64`) to `Cardinal` before
arithmetic. Perform all arithmetic in `Int64` space and narrow only at the final
assignment after a guard confirms safety.

```delphi
// correct
Elapsed := Sw.ElapsedMilliseconds;
if Elapsed >= MaxMs then Exit(False);
Remaining := Cardinal(MaxMs - Elapsed);  // safe: Elapsed < MaxMs guaranteed

// incorrect
Remaining := Cardinal(Sw.ElapsedMilliseconds) - MaxMs;  // overflow risk
```

### STY-031 (Fail): Guard Cardinal subtraction
Always guard before subtracting two `Cardinal` values. Unsigned underflow is silent
without overflow checking and raises `EIntOverflow` with it enabled.

---

## Code Rules

### STY-032 (Fail): No platform-specific units without explicit requirement
Do not use platform-specific units (e.g. `Winapi.*`) unless the feature explicitly
requires them. Code should target all RTL-supported platforms unless the project
`CLAUDE.md` states otherwise.

### STY-033 (Fail): Fields accessed via properties
Access fields via properties, not directly. Exception: inside the owning class
constructor.

### STY-034 (Warn): const parameters for managed types
Use `const` for parameters of managed types (strings, interfaces) that are not modified.
For unmanaged types, `const` is not required but is recommended when it improves clarity
about intent.

```delphi
procedure Log(const AMessage: string);     // correct -- managed type, not modified
procedure SetValue(AValue: Integer);       // acceptable -- unmanaged type
procedure SetValue(const AValue: Integer); // also acceptable -- signals intent
```

### STY-035 (Fail): case enum exhaustion
`case` statements on enum types must list all values explicitly. Raise `ENotImplemented`
(or project-specific exception) in the `else` branch. Do not use `Assert` alone --
as assertions may be disabled in release builds, leaving the else branch silently doing nothing.

```delphi
// correct
case AStatus of
  sActive:   HandleActive;
  sInactive: HandleInactive;
  sPending:  HandlePending;
else
  raise ENotImplemented.CreateFmt('Unhandled TStatus value: %d', [Ord(AStatus)]);
end;

// incorrect -- Assert alone is not safe if assertions are disabled in release
case AStatus of
  sActive:   HandleActive;
  sInactive: HandleInactive;
  sPending:  HandlePending;
else
  Assert(False, 'Unhandled TStatus value');
end;

// incorrect -- missing else branch entirely
case AStatus of
  sActive:   HandleActive;
  sInactive: HandleInactive;
end;
```

### STY-036 (Fail): Failure-returning functions use enum not Boolean
Functions that perform operations which can fail must return an enumerated result type
rather than `Boolean`. A function returning `Boolean` to signal success or failure
should instead return an enum that names each possible outcome, allowing callers to
handle specific failure modes rather than a generic true/false.

```delphi
// correct
type
  TFetchResult = (frSuccess, frTimeout, frNotFound, frUnauthorised);

function Fetch(const AURL: string): TFetchResult;

// incorrect
function Fetch(const AURL: string): Boolean;
```

### STY-037 (Warn): Guard clauses with Exit
Use guard clauses with `Exit` to reduce nesting. Use `Exit` sparingly outside of
guard clause context.

```delphi
// correct -- guard clause reduces nesting
procedure Process(AValue: Integer);
begin
  if AValue <= 0 then
    Exit;
  DoWork(AValue);
end;

// avoid -- unnecessary nesting
procedure Process(AValue: Integer);
begin
  if AValue > 0 then
  begin
    DoWork(AValue);
  end;
end;
```

---

## Assertions

### STY-038 (Fail): Assertions include explanatory string
Every `Assert` call must include a string explaining which expected condition is failing.

```delphi
Assert(AValue > 0, 'AValue must be positive');  // correct
Assert(AValue > 0);                              // incorrect
```

### STY-039 (Fail): Assertions for invariants only
Use assertions only for logical invariants -- conditions that must always be true
regardless of input or execution state (parameter preconditions, object invariants).
Do not use assertions for input validation, OS conditions, or general error handling.
Assertions may be disabled in release builds -- never rely on them as the sole
enforcement mechanism.

---

## Project Structure

### STY-040 (Fail): One project per directory
Every `.dpr` project must live in its own dedicated directory. No two projects may
share a directory.

### STY-041 (Fail): readme.md required per project
Each project directory must contain a `readme.md` serving as its entry-point
documentation.

### STY-042 (Fail): .dproj references readme.md as WelcomePageFile
The `.dproj` must reference `readme.md` via the `WelcomePageFile` element so it is
displayed when the project is opened in the IDE:

```xml
<Delphi.Personality>
  <Source>
    <Source Name="MainSource">MyProject.dpr</Source>
  </Source>
  <WelcomePageFile Path="readme.md"/>
</Delphi.Personality>
```

This can also be set via `Project -> Project Page Options... -> Project page` in the IDE.

---

## Source File Layout Order

Guidance: The compiler enforces some ordering; this guide enforces the rest for
consistency. Recommended unit layout:

1. Comment header (description, copyright, history)
2. `unit` declaration
3. `interface` uses clause
4. `type` / `const` / `var` -- one class per unit; paired classes are the exception
5. Class body order: `const` -> fields -> methods -> properties, grouped by visibility
6. Declaration block order: `const` -> `type` -> `var` -> methods
7. `implementation` uses clause
8. Instance `constructor` and `destructor` listed first
9. `class constructor CreateClass` and `class destructor DestroyClass` at the bottom
   of the implementation section, above any `initialization`/`finalization` blocks
10. `initialization` / `finalization` always last

---

## Version Control

Guidance:
- Refactoring-only changes must be committed separately from logic changes.
- Tests must pass after any refactoring commit before logic changes are introduced.

---

## uses Clause Order

Guidance: Order units in `uses` clauses as follows:
RTL -> System Library -> VCL/FMX -> Third Party -> Shared -> Project-specific.

---
