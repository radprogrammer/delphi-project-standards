# RADProgrammer Delphi Formatting Guide
- Source: https://github.com/radprogrammer/delphi-project-standards
- Last updated: 2026-02-22

---

Imported via `@.delphi/code-formatting-guide.md` in the project's `CLAUDE.md`.
AI Agents should apply these conventions to all generated and modified Delphi code
unless a project-level `CLAUDE.md` explicitly overrides a rule. For whole-file
reformatting of existing code, prefer dedicated formatting tools over manual edits.

---

## How Rules Are Presented

Numbered rules are checkable constraints an agent can evaluate from visible code alone.
They carry a severity:
- **Fail** — objective and unambiguous. Must not be violated.
- **Warn** — strongly preferred. Flag violations but do not block.

**Guidance** blocks cover contextual preferences and rationale that require judgment
rather than mechanical checking. No rule number is assigned.

---

## Indentation

### FMT-001 (Fail): 2 spaces per indentation level, no tabs
Indentation must use exactly 2 spaces per level. Tab characters are not permitted
anywhere in Delphi source files. The Delphi IDE converts tabs to spaces by default --
leave this setting on.

```delphi
// correct
begin
  if A > B then
  begin
    DoSomething;
  end;
end;

// incorrect -- tab characters used
begin
	if A > B then
	begin
		DoSomething;
	end;
end;
```

### FMT-002 (Fail): begin/end align with controlling statement
`begin` and `end` are not indented relative to their controlling statement -- they
align with it.

```delphi
// correct
if i > 10 then
begin
  j := 3;
end;

// incorrect
if i > 10 then
  begin
    j := 3;
  end;
```

### FMT-003 (Fail): Statements between begin/end indented 2 spaces
Statements inside a `begin`/`end` block are indented 2 spaces relative to the
`begin`/`end` keywords.

### FMT-004 (Fail): Comments indented as surrounding statements
Comments are indented at the same level as the statement they accompany. They are
not moved to the left margin or given special indentation.

```delphi
// correct
if Condition then
begin
  // explain what we are doing here
  DoSomething;
end;

// incorrect
if Condition then
begin
// explain what we are doing here
  DoSomething;
end;
```

### FMT-005 (Warn): asm section contents indented
Contents of `asm` sections are indented 2 spaces.

### FMT-006 (Fail): Class visibility sections align with class name
Visibility modifiers (`public`, `private`, `protected`, `strict private`) align with
the class name -- no extra indentation.

```delphi
type
  TMyClass = class
    FField: Integer;
  public
    procedure DoSomething;
  end;
```

### FMT-007 (Fail): Compiler directives indented as statements
Compiler directives (`{$IFDEF}` etc.) are indented at the same level as the
surrounding statements, not at the left margin.

```delphi
// correct
if i > 10 then
begin
  {$IFDEF DEBUG}
  Log('value exceeded');
  {$ENDIF}
end;

// incorrect
if i > 10 then
begin
{$IFDEF DEBUG}
  Log('value exceeded');
{$ENDIF}
end;
```

### FMT-008 (Fail): interface/implementation sections not indented
Contents of `interface` and `implementation` sections are not indented.

### FMT-009 (Fail): Nested brackets/parentheses not extra-indented
Do not add extra indentation for nested brackets or parentheses beyond the standard
2-space rule.

### FMT-010 (Fail): Nested functions indented relative to parent
Inner (nested) functions and procedures are indented 2 spaces relative to their
containing function.

```delphi
procedure TMyClass.DoWork;

  procedure LocalHelper;
  begin
    // nested function body
  end;

begin
  LocalHelper;
end;
```

### FMT-011 (Fail): Function/procedure body not indented
The `var` block and `begin` of a function or procedure body are at column 0 relative to the procedure declaration.
Statements inside the begin/end block are indented 2 spaces relative to begin.
```delphi
procedure TMyClass.DoSomething;
var
  I: Integer;
begin
  I := 0;
end;
```

### FMT-012 (Fail): Record fields indented 2 spaces
Fields inside record type declarations are indented 2 spaces relative to the `record`
keyword, following the same rules as `var` block declarations.

```delphi
// correct
type
  TMyRecord = record
    Name: string;
    Value: Integer;
  end;

// incorrect
type
  TMyRecord = record
  Name: string;
  Value: Integer;
  end;
```

---

Guidance — case statement indentation:
- Case labels indent relative to the `case` keyword
- Case contents indent relative to case labels
- `else` in a case statement aligns one level out from case labels:

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

Guidance — labels: place labels one indent level less than the current level (not at
the left margin, not at the current level).

---

## Spacing

### FMT-013 (Fail): Space after colon, not before
A single space follows the colon in declarations. No space before the colon.

```delphi
i: Integer;            // correct
function Foo: Boolean  // correct
i : Integer;           // incorrect
```

### FMT-014 (Fail): No spaces in format parameter colons
Colons used in `write`/`writeln` format parameters have no spaces.

### FMT-015 (Fail): Space after commas and semicolons
A single space follows each comma and semicolon. No space before.

```delphi
procedure Foo(A, B: Integer);  // correct
procedure Foo(A,B: Integer);   // incorrect
```

### FMT-016 (Fail): No space before opening parenthesis
No space between a function/procedure name and its opening parenthesis, in both
declarations and calls.

```delphi
procedure Foo(i: Integer);   // correct
MyFunc(42);                  // correct
procedure Foo (i: Integer);  // incorrect
MyFunc (42);                 // incorrect
```

### FMT-017 (Fail): Spaces before and after assignment operator
The `:=` operator must have a single space before and after it.

```delphi
A := B;  // correct
A:= B;   // incorrect
A :=B;   // incorrect
```

### FMT-018 (Fail): Spaces before and after binary operators
Binary operators including, but not limited to: (`+`, `-`, `*`, `/`, `=`, `<>`, `<`, `>`, `<=`, `>=`, `or`, `and`,
`xor`, `div`, `mod`) must have a single space before and after.

```delphi
A := B + C * D;  // correct
A := B+C*D;      // incorrect
```

### FMT-019 (Fail): Unary prefix operators space before only
Unary prefix operators (`+`, `-`, `not`) have a space before but not after.

```delphi
A := B + -C;    // correct
A := not Flag;  // correct
```

### FMT-020 (Fail): No spaces inside parentheses or brackets
No spaces inside `()` parentheses, `[]` square brackets, or `<>` angle brackets
in generics.

```delphi
MyFunc(A, B);    // correct
MyArray[5];      // correct
MyFunc( A, B );  // incorrect
MyArray[ 5 ];    // incorrect
```

### FMT-021 (Fail): Space after // in line comments
A single space follows the `//` token in line comments.

```delphi
// correct comment
//incorrect comment
```

### FMT-022 (Fail): Spaces inside block comments
Block comments (`{ }` and `(* *)`) use a space after the opening delimiter and before
the closing delimiter.

```delphi
{ correct }
(* correct *)
{incorrect}
(*incorrect*)
```

Guidance — conflict resolution: when spacing rules conflict, insert one space.

---

## Line Breaks and Length

### FMT-023 (Fail): begin on its own line before controlled block
`begin` must appear on its own line before a controlled block. Never place `begin`
on the same line as `then`, `do`, or `else`.

```delphi
// correct
if A < B then
begin
  DoSomething;
end;

// incorrect
if A < B then begin
  DoSomething;
end;
```

### FMT-024 (Fail): else on its own line
`else` must appear on its own line. Never collapse `end else begin`.

```delphi
// correct
if True then
begin
  A := 1;
end
else
begin
  A := 2;
end;

// incorrect
if True then
begin
  A := 1;
end else begin
  A := 2;
end;
```

### FMT-025 (Fail): else if on one line
No line break between `else` and `if`.

```delphi
// correct
if Condition1 then
  DoThis
else if Condition2 then
  DoThat;

// incorrect
if Condition1 then
  DoThis
else
  if Condition2 then
    DoThat;
```

### FMT-026 (Fail): Single instruction after if/case/loop on its own line
A single instruction following `if`, `case`, or a loop goes on its own line,
indented 2 spaces. It does not share the line with `then` or `do`.

```delphi
// correct
if i > 10 then
  j := 3;

// incorrect
if i > 10 then j := 3;
```

### FMT-027 (Fail): Single instruction in try/except/finally on its own line
A single instruction inside `try`, `except`, or `finally` blocks goes on its own
line, indented 2 spaces. Inside `except`, the `on E:` clause is indented 2 spaces,
and the handler body is indented a further 2 spaces relative to the `on` line.

```delphi
// correct
try
  DoSomething;
except
  on E: EMyException do
    HandleSpecific(E);
  on E: Exception do
    HandleGeneral(E);
end;

try
  DoSomething;
finally
  Cleanup;
end;

// incorrect
try DoSomething; except on E: Exception do HandleGeneral(E); end;
```

### FMT-028 (Fail): then stays on same line as condition
The `then` keyword stays on the same line as the boolean condition.

```delphi
// correct
if A < B then
  DoSomething;

// incorrect
if A < B
  then DoSomething;
```

### FMT-029 (Fail): No extra parentheses around boolean conditions
Do not add parentheses around the boolean condition of an `if` statement.

```delphi
if A < B then    // correct
if (A < B) then  // incorrect
```

### FMT-030 (Fail): One statement per line
Each line contains at most one statement.

### FMT-031 (Warn): Line length target 200 columns
Generated code should aim to stay within 200 columns. Lines beyond 200 columns
should be wrapped. The formatter's hard wrap limit is 240 columns.

### FMT-032 (Fail): Trailing whitespace removed
All lines must have trailing whitespace removed.

### FMT-033 (Fail): Each uses unit on its own line
Each unit in a `uses` clause appears on its own line.

```delphi
uses
  System.SysUtils,
  System.Classes;
```

### FMT-034 (Fail): Each var/const element on its own line
Each element in `var` and `const` sections appears on its own line.

---

## Functions, Properties, and Parameters

### FMT-035 (Fail): Return type on same line as function name
The return type stays on the same line as the function name.

### FMT-036 (Fail): Anonymous method assignments break to next line
Anonymous method assignments break to the next line after `:=`.

```delphi
FProc :=
  procedure
  begin
    DoWork;
  end;

FFunc :=
  function(AValue: Integer): Integer
  begin
    Result := AValue + 1;
  end;
```

### FMT-037 (Fail): Property declarations on single line unless exceeding line length
Declare properties on a single line where possible. When a property declaration exceeds
200 columns, break after the property name and type, and indent each clause (`read`,
`write`, `default`, `stored`) by 2 spaces.

```delphi
// correct -- fits on one line
property MyValue: Integer read GetMyValue write SetMyValue;

// correct -- too long, broken at clauses
property MyLongPropertyName: TMyVeryLongTypeName
  read GetMyLongPropertyName
  write SetMyLongPropertyName;

// incorrect -- broken unnecessarily when it fits on one line
property MyValue: Integer
  read GetMyValue
  write SetMyValue;
```

Guidance — parameters: keep function call and definition parameters on one line when
possible. When wrapping is required, align parameters 2 spaces in from the opening
parenthesis.

Guidance — anonymous methods as parameters:

```delphi
TThread.CreateAnonymousThread(
  procedure
  begin
    DoWork;
  end).Start;

TTask.Run(
  procedure
  begin
    DoWork;
  end);
```

Guidance — chained anonymous method calls: each chained call goes on its own line,
indented 2 spaces as a continuation:

```delphi
TTask.Run(
  procedure
  begin
    DoWork;
  end)
  .ContinueWith(
    procedure(ATask: ITask)
    begin
      DoMore;
    end);
```

Guidance — generic constraints: when a generic type constraint list exceeds 200 columns,
break after the opening `<` and indent continuation lines 2 spaces. There is no single
Delphi convention for this; prefer clarity over strict column alignment:

```delphi
// fits on one line -- preferred
type TMyClass<T: class, constructor, IMyInterface> = class

// too long -- break at constraints
type TMyClass<T: class, constructor,
  IMyInterface, IDisposable> = class
```

---

## Empty Lines

Empty line counts are **minimums between declarations** and **ceilings within code**.
Do not add blank lines beyond the ceiling counts. Do not remove user-added blank lines
that fall within the allowed maximum.

### FMT-038 (Fail): Maximum 2 adjacent empty lines
No more than 2 consecutive empty lines anywhere in the file. This is a ceiling -- never
add more than 2, but preserve up to 2 if the author placed them.

### FMT-039 (Fail): 0 empty lines around compiler directives
No blank lines immediately before or after compiler directives.

### FMT-040 (Fail): 1 empty line around section keywords
One empty line before and after `interface` and `implementation` keywords.

### FMT-041 (Fail): 2 empty lines between method declarations
Two empty lines between method/function declarations in both the `interface` and
`implementation` sections. The formatter enforces this minimum regardless of what
the author wrote.

### FMT-042 (Fail): 0 empty lines before subsection keywords
No blank lines immediately before `var`, `const`, or `label` subsection keywords.

### FMT-043 (Fail): 1 empty line before type keyword
One empty line before the `type` keyword.

### FMT-044 (Fail): 0 empty lines before visibility modifiers
No blank lines immediately before visibility modifiers (`public`, `private`,
`protected`, `strict private`).

---

## Capitalization

### FMT-045 (Fail): Reserved words and language directives lowercase
Reserved words (`if`, `then`, `begin`, `end`, `array`, etc.) and language directives
(`abstract`, `virtual`, `override`, `deprecated`, `platform`, `inline`, etc.)
must be lowercase.

### FMT-046 (Fail): Compiler switch directives UPPERCASE
Compiler switch directives (`{$IFDEF}`, `{$WARNINGS}`, `{$R+}`, etc.) must be
UPPERCASE. These are distinct from language directives and follow the opposite
casing convention.

### FMT-047 (Fail): Hex and float literals UPPERCASE
Hexadecimal and floating point literals must be UPPERCASE.

```delphi
$FF      // correct
1.23E-8  // correct
$ff      // incorrect
1.23e-8  // incorrect
```

### FMT-048 (Fail): Do not alter casing of existing identifiers
When modifying existing code, do not change the casing of user-defined identifiers.
Preserve the casing as first declared in the file.

---

## Alignment

### FMT-049 (Fail): No column alignment
All alignment options are disabled. Do not align:
- `=` in constants, initializations, or type declarations
- `:=` assignment operators
- End-of-line comments
- Type names in `var` sections or records
- Fields in property declarations
- Parameter types in function definitions
- `:` before type names

Each identifier stands at its natural indent with no extra spaces for column alignment.

```delphi
// correct
var
  MyData: Integer;
  MyLongerName: string;

// incorrect -- vertically aligned
var
  MyData      : Integer;
  MyLongerName: string;
```

---

## Multi-line String Literals (Delphi 12+)

Guidance: Multi-line string literals are used sparingly -- see STY-022. When they
do appear:
- The opening `'''` aligns with the surrounding statement's indent level
- Do not alter whitespace or indentation inside the string content -- it is data,
  not code
- The closing `'''` determines the base indentation of the string content:

```delphi
Msg := '''
  Hello
  World
  ''';
```

---
