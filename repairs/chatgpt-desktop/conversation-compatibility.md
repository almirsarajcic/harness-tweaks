# Reopen local conversations after a desktop rollback

**App:** ChatGPT desktop for macOS. **Source:** `26.1120.50957` (Codex `0.171.0-alpha.13.2`). **Destination:** `26.1111.71025` (Codex `0.169.0-alpha.11.1`).

## Diagnose

In the older build, open the affected local chat and page back to the failing message. The old App Server logged `failed to deserialize stored thread item … missing field action`. Compare the two bundled App Server schemas and the corresponding saved item. In this case the old schema required `AgentMessageDisplay.action` inside `moderation.display`. The newer item stored `mode`, `version`, and `trustedContacts` there.

## Repair a copy

1. Stop all app processes that write to the store. Take and verify a consistent snapshot of the catalog (`state_5.sqlite`), history index (`thread_history_1.sqlite`), and active and archived session records. Keep a separate untouched copy. SQLite's write-ahead log can contain committed data. [SQLite backup guidance](https://sqlite.org/backup.html)
2. Put one affected chat and its catalog and index in an isolated test profile. Check that catalog paths resolve to the copied files before launching the older build.
3. For a version-1 `show` item with `trustedContacts: "check"`, add the old `action` field as `true`, retaining the original fields. Apply the same translation to the source event and its indexed item, and recompute affected byte offsets from the rewritten file. Stop on unknown variants, conflicting fields, or missing catalog entries. The `skip` variant still needs a desktop test.

## Verify and undo

Check database integrity, record/index agreement, and offsets. In the isolated older build, open the chat, page through the previously failing message, send a disposable turn, then restart and confirm the new turn persisted.

In the original repair, an isolated `check` example passed an old-server rebuild, and a previously failing chat reopened in the old app. The live repair updated 394 conversation files and 1,777 indexed items. Those counts measure the scope of the repair. The original backup was assembled while the app ran, so it did not capture every component at the same instant.

To undo the staged test, discard the test copy. Use the untouched snapshot to compare original records. Any recovery after a live edit must be scoped to the affected records, index entries, and offsets. If a chat has changed since the repair, preserve the current state and reconcile it before restoring anything.
