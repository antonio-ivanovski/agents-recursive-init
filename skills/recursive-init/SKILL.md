---
name: recursive-init
description: Initialize/update AGENTS.md for all first-level packages within the provided directory.
agent: general
compatibility: opencode
---

## What I do

- Recursively initializes or updates the `AGENTS.md` files for all first-level packages within the specified directory
- Scans each package to understand its purpose, structure, and nuances
- Generates a concise AGENTS.md file that documents these aspects for developers.

## Execution

1. `ls -d {PROVIDED_DIR}/*/` → get first-level subdirectories only (not nested). Create to-do list of packages to keep track of created/updated AGENTS.md.
2. Report to the user the to-do list of packages found.
3. Spawn ONE @general subagent per package in SINGLE message (parallel Task calls). Max 5 subagents at once.
4. Verifies all packages received AGENTS.md files against to-do list.
5. Verify again with command template `find {PROVIDED_DIR}/* -maxdepth 0 -type d ! -exec test -f {}/AGENTS.md \; -print` returning any packages missing AGENTS.md.
6. Collect summaries, report which packages got AGENTS.md

---

## Subagent Prompt Template

```
Create or update `{PACKAGE_PATH}/AGENTS.md`.

## Explore First
1. package.json → name, description, exports
2. src/ → structure, main modules, patterns
3. How this package relates to other packages
4. README if exists

## Write AGENTS.md

### Template

# {package-name}

One sentence: what it does and WHY it exists in this project.

## Role in Project
How this package fits with others. What depends on it? What does it enable?

## What It Contains
- Key modules/files and their purpose
- Main abstractions/patterns used
- Entry points

## Nuances
- Non-obvious behavior
- Important implementation details
- Edge cases / gotchas
- Anything a developer should know before modifying

## See Also
- [Root AGENTS.md](../../AGENTS.md)
- [Relevant Topic Guide](../../docs/agents/{topic}.md) ← only link if genuinely relevant

---

## Rules
- CONCISE. Sacrifice grammar for brevity.
- Tiny package = tiny AGENTS.md (even 5-10 lines is fine)
- Focus on NUANCES and RESPONSIBILITY, not generic info
- Link to /docs or root AGENTS.md, never duplicate their content
- No commands section (covered by root)
- No dependencies list (visible in package.json)
- Skip sections with nothing meaningful to add
- No fluff ("write clean code", "follow best practices")

## Output
1. Write the file directly
2. Return SHORT summary: "{package-name} - [what you documented, 1 sentence]"
```

---

## Primary Agent Reporting

After all subagents complete, output:

```
## AGENTS.md Generation Complete

Generated/Updated for {N} packages:

- {example-crypto-package-name} - Key generation, signing, encryption primitives
- {example-jwt-package-name} - JWT creation/validation with DID support
- {example-resolver-package-name} - DID resolution infrastructure
... (one line per package)
```
