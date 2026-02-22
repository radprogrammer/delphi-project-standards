# RADProgrammer Delphi Formatting Guide
- Source: https://github.com/radprogrammer/delphi-project-standards
- Last updated: 2026-02-20

---

# Delphi Coding Standards

Imported via `@.delphi/code-formatting-guide.md` in the project's `CLAUDE.md`.
AI Agents should apply these conventions to all generated and modified Delphi code
unless a project-level `CLAUDE.md` explicitly overrides a rule. For whole-file
reformatting of existing code, prefer dedicated formatting tools over manual edits.

---

## Indentation

- Use **2 spaces** for continuation lines when an expression spans multiple lines
- Maximum indent width: **120 columns**
- Indent `asm` section contents
- `begin`/`end` are **not** indented relative to their controlling statement — they align with it:
  ```delphi
  if i > 10 then
  begin
    j := 3;
  end;
  ```
- Indent statements between `begin`/`end`
- Class visibility sections (`public`, `private`, etc.) align with the class name — no extra indent:
  ```delphi
  type
  a = class
    i: Integer;
  public
    j: Integer;
  end;
  ```
- Indent comments as normal statements
- Indent compiler directives as normal statements (not at left margin):
  ```delphi
  if i > 10 then
  begin
    {$IFDEF debug}
    operator;
    {$ENDIF}
  end;
  ```
- Do **not** indent function/procedure bodies (var, begin at column 0 relative to proc)
- Indent inner (nested) functions relative to their parent
- Do **not** indent `interface`/`implementation` section contents
- Do **not** add extra indent for nested brackets/parentheses

### Case Statements
- Indent case labels relative to `case` keyword
- Indent case contents relative to case labels
- `else` in case statements is **not** indented as a case label — it aligns one level out:
  ```delphi
  case i of
    value1:
      statement1;
    value2:
      statement2;
  else
    statement3;
  end;
  ```

### Labels
- Place labels at **one indent less** than the current level (not at left margin, not at current level)

---

## Spacing

### Punctuation
- Colons: **space after only** (`i: Integer`, `function Foo: Boolean`)
- Colons in format parameters (`write`/`writeln`): **no spaces**
- Commas: **space after only** (`a, b, c`)
- Semicolons: **space after only**
- No space before opening parenthesis in function calls or declarations:
  ```delphi
  procedure f(i: Integer);
  myFunc(42);
  ```

### Operators
- Assignment operators (`:=`): **space before and after** (`a := b`)
- Binary operators (`+`, `-`, `*`, `/`, `=`, `<>`, `<`, `>`, `<=`, `>=`, `or`, `and`, `xor`, `div`): **space before and after** (`a := b + c * d`)
- Unary prefix operators (`+`, `-`, `not`): **space before only** (`a := b + -c`)

### Comments
- `//` comments: **space before and after** the `//` token (`// comment text`)
- `{` and `(*` comments: **inner and outer spaces** (space after `{`, space before `}`)

### Parentheses and Brackets
- No spaces inside `()` parentheses
- No spaces inside `[]` square brackets
- No spaces inside `<>` angle brackets in generics

### Conflict Resolution
- When spacing rules conflict: insert **one space**

---

## Line Breaks

### General
- Right margin: **240 columns**
- Do **not** preserve existing user line breaks — let the formatter decide
- Trim trailing whitespace from all lines

### Begin/End
- Line break **before** `begin` in control statements (`if`, `case`, `while`, `for`):
  ```delphi
  if i > 10 then
  begin
    j := 3;
  end;
  ```
- Line break **after** `begin` always
- `else` on its own line — do **not** collapse `end else begin`:
  ```delphi
  if True then
  begin
    a := 1
  end
  else
  begin
    a := 2
  end;
  ```

### Control Statements
- Single instructions in `if`/`case`/loops go on their own line:
  ```delphi
  if i > 10 then
    j := 3;
  ```
- `else if` stays on **one line** — no line break between `else` and `if`:
  ```delphi
  if condition1 then
    operation1
  else if condition2 then
    operation2;
  ```
- Single instructions in `try`/`except`/`finally` go on their own line
- `then` stays on the **same line** as the condition

### Functions and Parameters
- Return type stays on the **same line** as the function name
- Parameters in function calls stay on **one line** when possible
- Parameters in function definitions stay on **one line** when possible
- Anonymous function assignments break to the **next line**:
  ```delphi
  func :=
    function(x: Integer): Integer
    begin
      Result := x + y;
    end;
  ```

### Uses Clauses
- Each unit in a `uses` clause goes on its **own line**

### Var and Const Sections
- Each element in `var` and `const` sections goes on its **own line**

### Empty Lines
Empty line counts below are **minimums between declarations** and **ceilings within code** — do not add blank lines beyond these counts, and do not remove user-added blank lines that fall within the allowed maximum.

- **Maximum 2** adjacent empty lines anywhere in the file — this is a ceiling; never add more than 2, but preserve up to 2 if the author placed them
- **0** empty lines around compiler directives
- **1** empty line around section keywords (`interface`, `implementation`)
- **2** empty lines between method/function declarations in the `implementation` section — the formatter enforces this minimum between methods regardless of what the author wrote
- **2** empty lines between method/function declarations in the `interface` section
- **0** empty lines before subsections (`var`, `const`, `label`) — do not insert blank lines before these keywords
- **1** empty line before `type` keyword
- **0** empty lines before visibility modifiers (`public`, `private`, `protected`, `strict private`)

---

## New Language Elements

The original RAD Studio formatter configuration predates several modern Delphi language features.
These rules extend the guide to cover them consistently.

### Multi-line String Literals (Delphi 12+)
Multi-line string literals are used sparingly — see `style-guide.md`. When they do appear:
- The opening `{` of the literal aligns with the surrounding statement's indent level
- Do not alter whitespace or indentation inside the string content — it is data, not code
- The closing `}` aligns with the opening `{`:
  ```delphi
  Msg := {'''
    Hello
    World
    '''};
  ```

### Anonymous Methods
Anonymous methods as **assignments** break to the next line (covered in Line Breaks above):
  ```delphi
  FProc :=
    procedure
    begin
      DoWork();
    end;
  ```

Anonymous methods passed as **parameters** indent the body one level relative to the call:
  ```delphi
  TThread.CreateAnonymousThread(
    procedure
    begin
      DoWork();
    end).Start();
  ```

Anonymous methods in **inline calls** where the call is a statement (not an assignment):
  ```delphi
  TTask.Run(
    procedure
    begin
      DoWork();
    end);
  ```

**Chained anonymous method calls** — each chained call goes on its own line, indented 2 spaces
as a continuation:
  ```delphi
  TTask.Run(
    procedure
    begin
      DoWork();
    end)
    .ContinueWith(
      procedure(aTask: ITask)
      begin
        DoMore();
      end);
  ```

---

## Capitalization

- Reserved words and directives (`if`, `then`, `begin`, `end`, `array`, `abstract`, etc.): **lowercase**
- Compiler directives (`{$IFDEF}`, `{$WARNINGS}`, etc.): **UPPERCASE**
- Hexadecimal and float number literals: **UPPERCASE** (`$FF`, `1.23E-8`)
- All other identifiers (method names, type names, etc.): **as first occurrence** in the file — do not alter casing of user-defined identifiers

---

## Alignment

All alignment options are **disabled**. Do not align:
- `=` in constants, initializations, or type declarations
- `:=` assignment operators
- End-of-line comments
- Type names in `var` sections or records
- Fields in property declarations
- Parameter types in function definitions
- `:` before type names

Each identifier stands at its natural indent with no extra spaces added for column alignment.
