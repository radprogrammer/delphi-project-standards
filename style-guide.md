# RADProgrammer Delphi Style Guide
- Source: https://github.com/radprogrammer/delphi-projects-standards/style-guide.md
- Version: 2.0

## Changelog
- 2.0 (2026-02-20): Converted to markdown instruction file for AI Agents
- 1.0 (2021-03-14): radteam wiki initial release

---

# Delphi Coding Standards

The initial purpose is for Claude Code instructions, imported via `@.delphi/style-guide.md` in the project's `CLAUDE.md`.
AI Agents should apply these conventions to all generated and modified Delphi code unless a project-level `CLAUDE.md` explicitly overrides a rule.

---

## Naming

All identifiers use PascalCase. No underscores. No Hungarian notation.

| Identifier            | Prefix / Suffix     | Example                  |
|-----------------------|---------------------|--------------------------|
| Class                 | `T`                 | `TMyClass`               |
| Exception class       | `E`                 | `EInvalidOp`             |
| Interface             | `I`                 | `IMyInterface`           |
| Field                 | `F`                 | `FUserName`              |
| Pointer type          | `P`                 | `PInteger`               |
| Method parameter      | `A`                 | `AUserName`              |
| Event handler         | `On`                | `OnMyExampleEvent`       |
| Event executor        | `Do`                | `DoMyExampleEvent`       |
| [Class constructor](https://docwiki.embarcadero.com/RADStudio/en/Methods_(Delphi)#Class_Constructors)     | `CreateClass`       | `class constructor CreateClass` |
| Class destructor      | `DestroyClass`      | `class destructor DestroyClass` |
| Test class            | suffix `Test`       | `TMyClassTest`           |
| Versioned interface   | suffix `2`, `3`...  | `IMyInterface2`          |
| Custom Attributes     | end with 'Attribute' | `MyExampleAttribute`    |


### Additional Naming Rules

- **Enums:** PascalCase, no prefix. Prefer fully qualified references everywhere. Bare names are acceptable within the declaring unit but should be avoided for consistency.
- **Constants:** If inside a class, no prefix — reference fully qualified externally. If outside a class, use UPPERCASE.
- **Booleans:** Positive naming only, using `is`, `has`, `can`, or `should` prefixes (e.g. `IsActive`, not `IsNotActive`).
- **Methods:** verb+noun form (`CalculateTotal`, `ValidateInput`).
- **Form components:** Descriptive suffix-based names (`DeleteFileButton`, not `Button1` or `butDelete`).
- **Arrays/Lists:** Pluralized names; use singular form for the iteration variable.

### Event Handler vs Event Executor

- `OnButtonClick` — the published event property; wired to an external handler.
- `DoButtonClick` — the `virtual` method containing the actual logic; called by the event machinery.
- All `Do`-prefix event executor methods must be declared `virtual` to allow subclass override of logic without replacing event wiring.

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
- `goto` statments
- Inline variables (`var x := ...` in Delphi 10.3+)
- Multi-line string literals (Delphi 12+)
- `System.DateUtils`: `YearsBetween`, `MonthsBetween`, `YearSpan`, `MonthSpan` 

---

## Version Control

- Refactoring-only changes must be committed separately from logic changes.
- Tests must pass after any refactoring commit before logic changes are introduced.
  
---

## Code Rules

- Access fields via properties, not directly. Exception: inside the owning class constructor.
- Use `const` for parameters that are not modified.
- Use guard clauses with `Exit` to reduce nesting. Use `Exit` sparingly outside of guard clauses.
- `case` statements on enums must list all values explicitly. Use `Assert(False, 'Unhandled enum value')` in the `else` branch.
- Functions that perform operations which can fail must return an enumerated result type rather than `Boolean`.
- `uses` clause order: RTL -> System Library -> VCL/FMX -> Third Party -> Shared -> Project-specific.
- Add units to the `implementation` uses clause unless they are required in the `interface`.
- The **Unit Scope Names** project setting should be empty. All unit references should be fully qualified (e.g. `System.SysUtils`, not `SysUtils`). Auto-generated and third-party code are exempt.

---
