---
name: shared-cloud-access
description: Use when implementing authenticated shared-cloud access, owner/editor/viewer permissions, invitation and revocation, local-to-cloud cutover, cloud restore, or an independent local copy of shared data.
---

# Shared cloud access

Keep authorization on the server and keep local/remote storage transitions explicit. A local role or hidden button is not an access-control boundary. Use [local-first-event-sync](../local-first-event-sync/SKILL.md) for event validation, push/pull, conflict detection, and local materialization; this skill covers access, ownership, and cutover around that pipeline.

## Establish the access contract

1. Inventory every server document reachable from a shared resource: parent metadata, membership, discovery references, invitations, events, and version pointers **where those concepts exist**. Identify who owns each document and which operations need a multi-document batch or transaction. Compare client writes with deployed security rules, not only design documents or UI state.
2. Require an authenticated identity for all private reads and writes. Base role checks on server-held membership, not a client-supplied role or cached local metadata. Define the product's role capabilities per operation and resource type; deny unrecognized roles and unlisted paths by default.
3. Bind newly created resource ownership to the authenticated identity according to the product's authority model. If invitations exist, require a valid invitation bound to its intended authenticated recipient and constrain role assignment; identifiers such as email are invitation constraints, not substitutes for authenticated membership.
4. If the product maintains per-user discovery references, derive their resource IDs and roles from authoritative membership. Review **each** write in owner-driven batches: allowing users to update only their own references does not authorize an owner to update another user's reference. Change rules and client/server write paths together under a narrowly validated authority model; never leave a role change half-applied.
5. Define who may invite, change roles, revoke access, leave, or delete; enforce that model on the server for each operation. Restrict self-editable membership fields and deny privilege escalation. If invitations are single-use, consume them atomically with membership creation. A copied invitation token or stale local role must not bypass access checks.

## Change storage ownership without losing data

### Local → cloud

1. Ask for explicit opt-in and establish the cloud auth session before mutation. Require one remote write authority; distinguish an export artifact from a live second remote writer to prevent split-brain.
2. Create the remote parent, owner membership, and owner discovery reference as a coherent operation. Only after remote creation succeeds, attach the local resource to the remote identity, persist the authenticated account binding, and queue existing local data for upload. Make failures at each boundary recoverable: do not report a fully backed-up resource solely because the parent exists or local provider metadata changed.
3. Run the event pipeline from [local-first-event-sync](../local-first-event-sync/SKILL.md). Surface incomplete upload, permission denial, or conflicts separately from successful cloud setup. Confirm the account that owns the local connection is the account now signed in before subsequent sync or restore.

### Cloud → independent local copy

1. Explain that this makes a device-only copy and **does not** delete the cloud original, other members' access, invitations, or remote data. Offer a sync-first choice for the latest available remote state and a clearly labeled current-device-state choice if sync is unavailable or deliberately skipped. Never automatically call the latter “up to date.” Block automatic sync-first conversion on incomplete sync, push failures, conflicts, or read-only blocks.
2. Snapshot active local data inside a recoverable local transaction: allocate a fresh local resource identity and fresh child identities wherever later re-import or export could collide; remap dependent references, copy optional settings and metadata, and make local-only records ineligible for cloud upload. Preserve the intended data and actor display information without carrying over cloud owner/account bindings, membership, or pending cloud queue entries.
3. Select the new local copy and remove only the old *device-side connection* after successful copy. Leave the remote resource intact. Do not treat a provider flag flip on the original identity as a copy. Distinguish this action from owner-only remote deletion in naming, confirmation, and tests.

### Restore and revoked access

1. For a signed-in account, list only its authorized discovery references, fetch selected resource metadata and the caller's membership from the server, then connect it locally with the server role. Materialize remote state through the event-sync workflow before presenting it as fully restored. Importing the same resource again must have a defined identity/collision policy; do not accidentally merge an independent local copy with the remote original.
2. After revocation, deny fresh parent/child reads and all writes even if the client retains a local role, cached reference, invitation code, or old session. Define whether existing offline bytes remain as a local copy, are hidden, or are purged, and tell users before destructive cleanup. A recipient leaving should remove only their membership/reference; an owner deleting the entire remote resource is a separate destructive operation with explicit impact on other accounts.
3. For a genuinely destructive owner action, remove dependent documents and references in bounded, retryable batches, then remove the owner's own membership and parent in a valid final operation. Check authorization for every batch state, including the final owner/parent removal; do not assume deleting a parent automatically deletes its subcollections. If account deletion combines owned-resource deletion and leaving shared resources, keep those outcomes distinct.

## Consumer-visible verification

Use disposable data and at least two genuinely different authenticated accounts against the **actual security rules in an emulator**. Client fakes and tests that search rule text verify wiring only; they cannot establish server-side denial. Exercise the complete UI/repository path as well as rules tests.

- Owner creates a resource; owner and invited recipient see only authorized discovery entries. Recipient accepts an invitation for their own normalized identity once; a different account, expired/reused invitation, forged role, or forged discovery reference is denied. Unauthenticated and unrelated accounts cannot list, read, create, or change private documents.
- Editor can write only explicitly allowed data; viewer cannot write; neither may manage sharing, edit owner-only metadata, forge an actor, or self-promote. Owner can change/revoke a non-owner and cancel invitations, but cannot demote/remove the active owner; member self-profile updates touch only permitted fields. For each role-change/revocation batch, verify both membership and recipient discovery reference after commit and after injected failure: no contradictory roles or lingering access.
- After revocation, the recipient's fresh server read/write fails and the discovery entry disappears. Previously cached local data follows the stated offline-retention policy, rather than masquerading as live shared access. A member leaving must not delete the server original or another member's data; owner deletion must explicitly remove dependent server data and invalidate other members' references.
- Convert a populated local resource, interrupt remote creation, local attachment, and initial upload separately, then retry. Confirm no false backup claim, duplicate live writer, lost local content, or accidental cross-account sync. On a fresh device/session of the authorized account, restore role and resource state; the unrelated account remains denied.
- From a shared resource, test successful sync-first copy and an unavailable-sync current-state copy. Compare local content and optional metadata, verify new identities and no pending cloud uploads, restart to confirm durable selection, then re-import the untouched remote original under its separate identity. Revoke a member afterward and confirm the already independent local copy does not alter the remote original.
