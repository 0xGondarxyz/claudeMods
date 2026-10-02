# claudeMods

Free mods for Claude Code. MIT licensed.

## Install

Inside Claude Code:

```
/plugin marketplace add 0xGondarxyz/claudeMods
/plugin install dashless@claudemods
```

Or try one without installing:

```
git clone https://github.com/0xGondarxyz/claudeMods
claude --plugin-dir claudeMods/dashless
```

Mods are not sandboxed. They run with the same access as Claude Code. Read the source before you install any mod, including these.

## dashless

Removes em dashes from what Claude writes, before it lands. About 80 lines of TypeScript.

| Where | What gets fixed |
| --- | --- |
| Write tool | the file content |
| Edit tool | `new_string` only (`old_string` must match the file, so it is never touched) |
| Bash | only `git commit` and `gh pr` / `gh issue` `create`, `edit`, `comment`; every other command runs as is |
| Replies | text blocks in Claude's responses, before they are stored |

Rewrite rules (`, ` is the em dash, `–` the en dash):

| Input | Output |
| --- | --- |
| `a, b`, `a, b` | `a, b` |
| `, item` at line start | `item` (indentation kept) |
| `done, ` at line end | `done` |
| `note: x` | `note: x` |
| `a, b` (spaced en dash) | `a, b` |
| `1990–2000` (range) | unchanged |
| `--force`, `-- file` | unchanged |

The status line shows a running count: `dashless: 3 em dashes fixed`.

Tested by running the same prompt twice in a throwaway git repo (write a file, commit it, reply with a sentence, each with an em dash). Without the mod 4 em dashes landed; with it, 0.

Run its tests: `claude plugin test dashless`
