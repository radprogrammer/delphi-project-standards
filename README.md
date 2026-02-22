# RADProgrammer Delphi Project Standards
https://github.com/radprogrammer/delphi-project-standards

Coding standards and formatting guidelines for Delphi projects, maintained as AI agent
instruction files for use with Claude Code and similar tools.

<img width="1536" height="1024" alt="Delphi Project Standards logo" src="https://github.com/user-attachments/assets/a6e9206a-7c22-4521-a895-76485efc006c" />

---

## Contents

- `style-guide.md` -- Naming conventions, code rules, project structure conventions,
  and language usage guidance. Closely matches Embarcadero's published style guidelines
  with deliberate enhancements for consistency and maintainability.
- `code-formatting-guide.md` -- Indentation, spacing, line break, and capitalization
  rules for generated and modified code.
- `CHANGELOG.md` -- Version history for both files.

---

## Usage as a Submodule

This repo is intended to be consumed as a `.delphi` submodule, as configured in
[delphi-project-template](https://github.com/radprogrammer/delphi-project-template).
Projects created from that template include this repo at `.delphi/` and import the
standards into Claude Code via `CLAUDE.md`:

```markdown
@.delphi/style-guide.md
@.delphi/code-formatting-guide.md
```

To update a project to the latest standards, run `update-standards.bat` from the
project root. This pulls the latest commit from this repo, stages the submodule
update, and commits it. Review `CHANGELOG.md` before committing to understand
what has changed.

---

## Adding to an Existing Project Manually

```bash
git submodule add https://github.com/radprogrammer/delphi-project-standards .delphi
git commit -m "Add .delphi standards submodule"
```

Then add the following to your project's `CLAUDE.md`:

```markdown
@.delphi/style-guide.md
@.delphi/code-formatting-guide.md
```

---

## Provenance

These standards originated in the radteam internal wiki (2021) and were migrated
and reformatted for AI agent use in 2026. See [CHANGELOG.md](CHANGELOG.md) for
full version history.


---

## Contributing

Suggestions and feedback are welcome via
[GitHub Issues](https://github.com/radprogrammer/delphi-project-standards/issues).
These standards are curated by Delphi MVPs with input from the broader community.
Significant changes are discussed openly before adoption.

--- 

<div align="center">
<a href="https://www.embarcadero.com/products/delphi" target="_blank">
  <img src="https://www.embarcadero.com/images/logos/delphi-logo-128.webp"
       alt="Modern Delphi Logo"
       width="64" />
</a>
<br/>
Built for Delphi Developers
</div>
