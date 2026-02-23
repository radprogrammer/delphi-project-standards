## Changelog
https://github.com/radprogrammer/delphi-project-standards


### 2.5 (2026-02-22)
- `style-guide.md` and `code-formatting-guide.md`: Major restructure -- all checkable
  constraints converted to numbered rules (STY-001 to STY-042, FMT-001 to FMT-049) with
  Fail/Warn severity; prose guidance retained for contextual advice; overlap between files
  eliminated; spacing, indentation, and block formatting rules consolidated into
  `code-formatting-guide.md` exclusively
- `style-guide.md`: Added prohibited DateUtils functions (STY-021); added class
  constructor/destructor naming rules (STY-013/014); added const parameters guidance
  (STY-034); changed unhandled case else branch from Assert to raise ENotImplemented
  (STY-035); added enum-not-Boolean rule with example (STY-036)
- `code-formatting-guide.md`: Added record field indentation (FMT-012); added except
  on E: clause indentation (FMT-027); added property declaration line-break rule
  (FMT-037); clarified bare keywords vs compiler switch directive casing distinction
  (FMT-045/046); added tie-breaker for FMT-039 vs FMT-041 conflict; line length target
  set to 200 columns (FMT-031)

### 2.4 (2026-02-22)
- `style-guide.md`: Remove restriction on using inline variables (much of the remaining tooling issues are
no longer applicable after code formatting and refactoring removals in 13+)

### 2.3 (2026-02-22)
- `style-guide.md`: Expanded spacing and indentation rules; added explicit begin/end and if statement rules;
added assertions policy; discouraged multiple declarations on one line

### 2.2 (2026-02-22)
- `style-guide.md`: Added Project Structure Conventions section

### 2.1 (2026-02-20)
- `style-guide.md`: Added source file conventions (CRLF, ASCII), constants typing rules, magic number rules,
Cardinal/Int64 safety rules, platform unit guidance

### 2.0 (2026-02-20)
- Converted to markdown instruction file for AI Agents

### 1.0 (2021-03-14)
- radteam wiki initial release (https://github.com/radprogrammer/radteam)
(now read-only)


