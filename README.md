# Recursive Init AGENTS.md

A small skill that **recursively initializes/updates `AGENTS.md`** for every *first-level* package directory inside a provided folder. **Well-suited for monorepos** with many packages.

It’s designed for agent/LLM workflows: it scans each package briefly (purpose, structure, nuances) and writes a concise `AGENTS.md` that helps future agents and developers work effectively in that package.

## Install

Download the `skills/recursive-init` into your skills (`<project-root>/skills/recursive-init`).

OR

```bash
npx skills add antonio-ivanovski/agents-recursive-init --skill "recursive-init"
```

## Usage

> [!WARNING]  
> This skill will dispatch multiple subagents. They will be either of the same model as the primary agent or of you explicitly configured model for `@general` subagents. Using a high-cost model may incur significant costs. Recommended to use a low-cost model or temporary configure such for for the `@general` subagents. Personally have used Claude Haiku 4.5 with good results.

Prompt your coding tool with: `Use recursive-init skill for @packages/`

It shall respond with something like:

```
> I'll use the recursive-init skill to initialize/update AGENTS.md files for all first-level packages in the @packages/ directory.
>> ⚙ skill [name=Recursive Init AGENTS.md]
>I'll now execute the recursive-init skill for the packages directory.
>> # List first-level package directories
$ ls -d <parent-dir>/packages/*/
<parent-dir>/packages/accounts/
<parent-dir>/packages/api/
<parent-dir>/packages/auth/
<parent-dir>/packages/authorizer-config/
<parent-dir>/packages/authorizer/
<parent-dir>/packages/crypto/
# ....
Click to collapse
# Todos
[ ] accounts
[ ] api
[ ] auth
[ ] authorizer-config
[ ] authorizer
[ ] crypto
# ....
Found 31 packages. Spawning 5 parallel @general subagents (batch 1/7):

````

## What It Does

- Enumerates **only first-level subdirectories** of a provided directory (no nested discovery).
- Creates/updates `{package}/AGENTS.md` for each package.
- Uses **one subagent per package** to keep work isolated and fast. It is specified to run a maximum of 5 subagents concurrently, but this is not guaranteed as the actual model might decide otherwise
- Verifies that every package received an `AGENTS.md`.

## Generated `AGENTS.md` Content

Each package’s `AGENTS.md` is intentionally concise and focuses on:

- **Role in project** (how it fits with adjacent packages)
- **What it contains** (key modules, entry points)
- **Nuances** (gotchas, edge cases, non-obvious behavior)

It avoids duplicating root docs, avoids dependency lists, and skips fluff.

## Compatibility

- Agent type: `general`
- Compatibility: `opencode`

## Contributing

- Update the skill contract in `skills/recursive-init/SKILL.md`.
- Keep the execution steps deterministic and easy to verify.
- Prefer small, surgical improvements (the goal is reliable automation, not prose).

## References

- Agent Skills: https://agentskills.io/
- OpenCode Documentation: https://docs.opencode.com/
- OpenCode Init Template: https://github.com/anomalyco/opencode/blob/dev/packages/opencode/src/command/template/initialize.txt
- [@mattpocock](https://github.com/mattpocock) Guide To AGENTS.md: https://www.aihero.dev/a-complete-guide-to-agents-md
- ClaudeCode Skills Guide: https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview