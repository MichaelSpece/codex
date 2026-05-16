# Threadline dogfood TUI

Threadline is an independent dogfood build of the Codex TUI fork. It starts as
a TUI-first experiment for session-management workflows that are not yet
available upstream.

Threadline is not OpenAI Codex and is not published by OpenAI. The initial
dogfood build intentionally keeps the existing app-server, configuration,
authentication, and session formats so it can be evaluated without migrating
local state.

## Build and run

From `codex-rs`:

```shell
cargo build -p codex-tui --bin threadline
./target/debug/threadline
```

The original `codex-tui` binary remains available for comparison:

```shell
cargo build -p codex-tui --bin codex-tui
./target/debug/codex-tui
```

## First differentiator

Threadline's first dogfood feature is keyboard-first chat archiving:

```text
/archive
/archive <saved-chat-id-or-name>
/unarchive
/unarchive <archived-chat-id-or-name>
```

`/archive` opens the active-chat picker, while `/unarchive` opens the archived
chat picker. Supplying an id or saved chat name targets that session directly.
Archive and unarchive operations use the existing app-server thread archive
APIs and report success or failure in the TUI.

## Current status

The archive/unarchive patch is tracked in the fork PR:

- https://github.com/MichaelSpece/codex/pull/2

An upstream feature request already exists:

- https://github.com/openai/codex/issues/14076

The branch compares against upstream, but `openai/codex` currently limits pull
request creation to collaborators, so the fork PR is the durable review and
dogfood artifact for now.

## Dogfood checks

```shell
cargo build -p codex-tui --bin threadline
cargo test -p codex-tui archive
```

Manual smoke test:

1. Launch `threadline`.
2. Create or open a chat.
3. Run `/archive` and confirm the chat leaves the active resume flow.
4. Run `/unarchive` and confirm the chat can be restored and resumed.
