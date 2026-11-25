# lib-lang-run-compiled

`handl-plugin-lib-lang-run-compiled` — the `run_compiled` meta function
as a composable `plugin-lib-*` leaf crate.

## Layout

| path | role |
| --- | --- |
| `handl-packages/lang_run_compiled/lang_run_compiled.hnl` | the .hnl surface |
| `src/lib.rs`, `src/tests.rs` | the Rust crate |
| `pkg.hnl` | package manifest |
| `scripts/hnl-gate.sh` | shared hnl typecheck/lint/test gate |
| `workflows/gate.yaml` | bupha entry (handl-gates) |

## Contract

- One callable: `run_compiled.run_compiled(handle)` — extracts the id
  and target from a `{"__compiled__": <id>, "target": <target>}` handle
  (produced by `lib-lang-compile`), builds a sub-runtime via
  `RuntimeFactory` with a dummy `Language` (the artifact carries its
  own `lang`/`ast`), executes, returns the result value.
- Leaf crate: `handl-runtime`/`handl-language`/`handl-platform`/
  `handl-plugin-module` only — no engine impl crates, no `handl-kit-*`.
- Same `Rc<dyn RuntimeFactory>` wiring caveat as the other `lang-*`
  meta crates (not `Send + Sync`; see `src/lib.rs`).
- Sibling-repo deps are git deps (`branch = "main"`); `[patch]` tables
  resolve them from sibling checkouts.

## Verification

```bash
cargo test    # the Rust suite
workflows/gate.yaml    # the bupha gate — delegates to scripts/hnl-gate.sh
```

## License

MIT.

<!-- myco:agent-awareness -->

## Agent awareness

- Session start / any work discussion: run `myco status --prompt`
  before proposing next steps — know the board first.
- User describes new work → offer to capture it (`myco todo add`,
  `issue new`, `proposal new`). Don't wait to be asked.
- Picking up a task: `myco resume <ID>` is the briefing.
- Wrapping up: `myco handoff <ID>` anything resumable; keep statuses
  honest (claimed → in-progress → blocked/done).

<!-- /myco:agent-awareness -->
