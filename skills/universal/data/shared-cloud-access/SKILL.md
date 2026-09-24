---
name: shared-cloud-access
description: Use when securing access to shared cloud resources, roles or invitations where supported, revocation, or local/cloud ownership transitions without losing user data.
---

# Shared cloud access

Enforce access at the server and keep local/remote ownership transitions explicit. A cached role or hidden control is not an access boundary. If the product uses a local-first event log, follow [local-first-event-sync](../local-first-event-sync/SKILL.md) for event validation, push/pull, and conflict handling; otherwise use its actual storage/sync protocol. This skill owns access, authority, and safe cutover, not the choice of synchronization engine.

## Establish the access contract

1. Inventory every server record reachable from a shared resource: resource metadata, access grants, discovery references, invitations, events, and version pointers **only where those concepts exist**. Identify the authority and ownership of each record and which operations need an atomic batch or transaction. Compare client operations with the deployed server authorization rules, not only design documents or UI state.
2. Require an authenticated identity for private data. Derive authorization from server-held resource access or claims validated by the server, never from a client-supplied role or cached local metadata. Define the product's allowed operations for each principal and resource; deny unrecognized authority and paths.
3. Where resources have an owner, bind creation to the authenticated identity under the product's ownership model. If invitations exist, require a valid invitation bound to its intended authenticated recipient and limit the permissions it grants; an email address or token alone is not proof of membership.
4. If the product maintains per-user discovery references, derive their resource IDs and capabilities from authoritative access records. Review **each** write in a multi-user batch: a rule allowing a user to edit only their own reference does not authorize an owner to edit another user's reference. Change server rules and client/server writes together under a narrowly verified authority model; do not leave access changes half-applied.
5. Define who may invite, change access, revoke, leave, or delete under the actual product model. Restrict self-editable grant fields and deny privilege escalation. If invitations are single-use, consume them atomically with granting access. Stale local state and copied tokens must not bypass current server checks.

## Change local/cloud ownership only when the product supports both

### Local → cloud

1. Require explicit opt-in and establish cloud authentication before changing ownership. Identify the authoritative writer and protect against two live writers publishing divergent state; an export artifact is not automatically a sync source.
2. Create the remote resource and its required authority/discovery records as a coherent operation. Only after remote creation succeeds, associate local content with the remote identity and account binding, then upload or reconcile existing data through the product's actual protocol. Make every boundary recoverable; a remote parent or changed local connection flag alone does not prove a completed backup.
3. If syncing events, run [local-first-event-sync](../local-first-event-sync/SKILL.md); for another protocol, check its confirmation/retry and conflict semantics. Surface incomplete upload, access denial, and conflicts separately from successful cloud setup. Verify that the locally connected account still matches the account authenticated for subsequent sync or restore.

### Cloud → independent local copy

1. Explain that an independent device copy does **not** delete the cloud original or other members' access. Offer the latest confirmed remote state when sync succeeds and a clearly labeled current-device-state choice if sync is unavailable or deliberately skipped. Never call the latter “up to date.” Block an automatic sync-first conversion on incomplete upload, unresolved conflict, or access denial.
2. Snapshot local content in a recoverable transaction: allocate new resource and child identities where later import, export, or reconnection could collide; remap references; copy applicable settings and metadata; and keep the independent copy out of the cloud upload queue. Preserve intended display data but not cloud ownership or account bindings.
3. Select the new local copy and remove only the old *device-side connection* after a successful copy. Leave remote data intact. Do not treat a connection flag flip on the original identity as a copy. Distinguish this from destructive remote deletion in naming, confirmation, and checks.

### Restore and revoked access

1. For a signed-in account, list resources using the product's authorized discovery mechanism, verify the caller's server-held access, and connect a selected resource locally. Materialize or fetch remote state through the actual sync protocol before presenting it as fully restored. Reconnecting the same resource must have a defined collision policy; never merge an independent local copy with the remote original by accident.
2. After revocation, deny fresh private reads and writes even if a client retains a cached grant or old session. Decide whether previously cached bytes remain locally, are hidden, or are purged, and explain destructive cleanup first. Leaving a resource should remove only the departing member's access where that action exists; deleting the entire shared resource is a separate, explicitly authorized operation with visible impact on others.
3. For a destructive whole-resource deletion, remove dependent data and references in bounded, retryable operations, then remove the final authority and parent in an authorized order. Check access for every intermediate state. In document databases, do not assume deleting a parent cascades to subcollections; on other backends, confirm actual cascading/retention behavior. Keep owned-resource deletion distinct from leaving other people's resources during account deletion.

## Consumer-visible verification

Use disposable resources and two distinct authenticated accounts against the **actual backend authorization configuration** in a test environment or emulator. Client fakes and tests that search rule text verify wiring only; they cannot establish server-side denial. Exercise the complete UI and repository path alongside authorization checks.

- Create a resource and grant a second identity access through the product's actual flow; an unauthenticated or unrelated identity cannot discover, read, create, or change private data. If invitations exist, test acceptance by the intended recipient only, expired/reused credentials, forged grants, and attempts to accept for someone else.
- For each configured role or capability, exercise permitted and forbidden operations. A read-only principal cannot write; a writer cannot change ownership unless explicitly authorized; self-edits cannot grant more power. If the product stores per-user discovery references, test access changes after success and injected failure: no contradictory roles or lingering discovery entries.
- After revocation, fresh backend reads and writes fail, while previously cached data follows the stated retention policy rather than masquerading as live access. A departing member must not delete the original or another member's data. Destructive resource deletion must remove dependent server data according to the backend's actual semantics.
- If local-to-cloud conversion exists, interrupt remote creation, local attachment, and initial upload separately, then retry. Confirm no false backup claim, split-brain writer, lost local content, or cross-account sync. Restore on a fresh authorized session; an unrelated account stays denied.
- If an independent device copy exists, compare a completed sync-first copy with an unavailable-sync current-state copy. Verify new identities, no cloud-pending operations for the independent copy, and durable selection after restart. Reconnect the untouched remote original separately and check that later revocation of one member does not alter a previously independent local copy.
