---
name: offline-data-integrity
description: Use when changing offline document import, external sharing, private storage, or reader restoration where incomplete copies, stale updates, or failed metadata writes could damage user documents.
---

# Offline data integrity

Preserve the last usable copy and durable metadata through import, sharing, migration, reading, deletion, and restart. Treat a successful UI transition as different from a durable commit.

## Prerequisites

- Identify the product's supported input types, its source-access lifetimes (temporary grants, callbacks, security-scoped URLs, or ordinary files), the owned private storage, metadata schema, and any extension-to-app inbox. These facilities are conditional; no specific platform, inbox, or UI architecture is required.
- Record the format-specific safety policy (including archive traversal/expansion guards), available disk capacity, existing library entries and locators, and the publication/recovery mechanism. Work with disposable inputs and preserve pre-existing user data.
- Find every writer and restore path: imports, repairs, migrations, reading checkpoints, settings, open/switch, deletion, backgrounding, snapshot/scene restoration, and any asynchronous callbacks.

## Import or receive a document

1. Acquire provider access and copy the full source into app-owned staging **before** the access expires; for callback-scoped file representations, finish copying during the callback. Stream copying, hashing, decoding, and indexing when inputs can be large; bound working buffers and actual decoded/display windows instead of assigning an arbitrary document-size cap. Confirm real space and surface provider, cancellation, decoding, or disk failures without touching the previous library. Apply the product's documented format-specific limits; a relaxed plain-text limit does not relax archive/path/expanded-size guards.
2. Validate the source and its destination, including canonicalized archive paths and extraction boundaries where archives are supported. If the product deduplicates imports, compute identity from content bytes rather than filename or provider URL, and preserve an existing entry and locator for identical content. If its managed original is missing, stage and atomically replace only that original; retain derivatives, metadata, and reader position if repair fails.
3. Build prepared content and any locator migration in staging. Decode and validate existing metadata before permitting any mutation; unreadable metadata must not become an empty library on the next save. Preserve the old original, usable cache, and durable metadata until replacement preparation succeeds. Do not publish partially migrated locators or incomplete caches.
4. Publish the staged bytes and metadata with checked completion and recoverable ordering (for example, same-directory staging, sync/close/atomic rename, or an equivalent transactional store). Serialize overlapping import, repair, and delete operations. A failed metadata publication must not advance the in-memory committed state; retry must remain possible and the last durable state must still reopen.
5. For an extension or external sharing inbox **when present**, use collision-safe, visible destination names and publish complete batches only after all copies finish. Do not expose half-copied entries. Treat plain text as semantic text if the provider offers a serialized-object representation instead of literal file bytes; choose a supported representation even when an alternate unsupported one is also advertised. A shared remote URL alone is not permission to fetch its contents. Receiving a share need not force-launch the host or auto-open a document: honor the product's explicit receive/open contract.

## Persist and restore reading state

1. Choose stable, content-relative locators and a fallback appropriate to each supported format: for example, a normalized-text byte anchor plus within-line position and byte-progress fallback; a chapter/block anchor with within-block fraction and scroll fallback; or an image-page index with within-page position. Persist the library, preferences, selected document, and per-document locator at coherent transaction boundaries. An explicitly requested start is not an old saved locator.
2. Checkpoint periodically during reading, **before** explicit navigation/switch/deletion boundaries, and on backgrounding. A termination callback alone or an unbounded debounce cannot guarantee durability. Coalesce only superseded ordinary updates: never reorder across checkpoint, open, delete, or snapshot boundaries. Reject stale callbacks and older async writes that would overwrite a newer locator, undo a deletion, or replace an explicit shelf selection.
3. On launch, restore the durable selection and locator only after layout and required local assets are ready. Do not persist a renderer's initial zero/empty position during restoration. If the selected item was deleted, restore the library instead. If multiple windows/scenes can select independently, use durable per-window selection rather than allowing an older OS UI snapshot to override the repository.
4. On font, viewport, orientation, window-size, or appearance changes, retain the content anchor through reflow. Ignore transient zero-size layouts as position updates. Preserve a saved exact start (including leading padding), while still honoring deliberate fragment/page jumps. Delete confirms that both the library entry and any startup/window references are cleared; a failed deletion must leave the active reader and durable data usable.

## Round-trip and boundary checks

Use disposable fixtures through the **real** picker, share sheet, reader, and restart path where those surfaces exist; a parser/unit test or successful build alone cannot prove access grants, inbox publication, or startup restoration.

- Import a fixture, navigate to a recognizable mid-document location, checkpoint, force-stop while the reader is foregrounded, relaunch, and confirm the same document and content anchor. Repeat after backgrounding, a setting-driven reflow, and a viewport resize; do not mistake temporary zero layout for saved progress. Verify an explicit jump to the start remains the start.
- If deduplication is supported, reimport identical bytes under a different filename: the existing entry and locator must remain unchanged. Test a different document with the same suggested name, a dot-prefixed name, and a multi-file share where supported: no overwrite, hidden entry, partial batch, or unrequested auto-open. Verify that a supported file/text item remains importable when its provider also advertises an unsupported representation. After provider access ends, the owned copy must still open.
- Interrupt copying, preparation, metadata publication, repair, migration, and deletion independently (injected failures where needed). At every boundary, reopen the previous original, prepared cache, and saved locator; ensure no partial/new startup target or lost user data. Restore capacity and retry the pending operation. Test corrupt metadata without saving an empty replacement.
- Race rapid position/preference changes with backgrounding, an explicit open, and deletion; relaunch and verify that the last committed selection, preference, and locator win, while deleted entries stay deleted. If there are multiple windows, switch independently, discard one session, and verify the others remain intact.
- Exercise a large input and a huge logical line using bounded memory; verify full-content hash or equivalent fidelity after preparation. Inject a late decode failure and confirm that any fallback restarts from the source instead of splicing differently decoded fragments. Check archive traversal/escape and oversize rejection separately from plain-text capacity, with the existing library intact.

Stop and report the failing phase if ownership of source bytes, metadata-load readiness, checked durable publication, or recovery of the previous good state cannot be demonstrated. Do not declare success from a staged file, a completion toast, an installation, or a compile alone.
