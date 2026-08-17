# AX Downloader — Engineering Handoff

**Date:** 2026-08-18  
**Authoritative current checkpoint:** **C# Core CP-021 — WINDOWS ACCEPTED**  
**Next checkpoint:** **CP-022 — Runtime Progress & Telemetry Contract**

## Current authority

CP-021 passed on Windows with:
- `CSHARP_CP021_BUILD_OK`
- `CSHARP_CP021_CONFORMANCE_OK`

The same run kept the complete CP-001→CP-020 regression chain green.

## Accepted C# sequence

- CP-009 — no-Range single-session fallback
- CP-010 — automatic Range/no-Range dispatch
- CP-011 — controlled Stop + safe Resume
- CP-012 — destructive Cancel cleanup
- CP-013 — job lifecycle state machine
- CP-014 — runtime job controller
- CP-015 — durable job persistence + restart recovery
- CP-016 — persistent sequential queue foundation
- CP-017 — sequential queue worker
- CP-018 — persistent scheduled queue start (accepted with R2 after fixing nullable compile errors in the test harness; no test bypass)
- CP-019 — persistent named queues + isolation
- CP-020 — named queue runtime control + global execution lease
- CP-021 — named queue Pause/Resume runtime

## Current runtime model

### Download mode
- Range-capable source → segmented/parallel engine
- no-Range source → single full-body engine

### Job controls
- Start
- Pause
- Resume
- Stop
- Cancel

### Queue/runtime
- persistent ordering
- sequential worker
- named queues
- per-queue scheduling
- global single active queue lease
- pause releases lease
- another queue may run while first queue is paused
- paused queue resume is blocked while another queue owns the lease

### Persistence/recovery
- durable job records
- queue persistence
- named queue registry
- schedule persistence
- restart recovery
- atomic writes
- lock retry
- corruption rejection/preservation

## Frozen semantics

### Pause
- non-destructive
- preserves verified/recoverable work
- state = `Paused`
- releases global queue lease
- tail queued job must not auto-run

### Stop
- non-destructive
- preserves verified/recoverable work
- state = `Stopped`
- explicit Resume required

### Cancel
- destructive only after transfer is quiescent
- removes state/partial/temp artifacts for the job
- preserves already completed final file
- state = `Canceled`

### Failed
- persistent, resumable/retriable
- never treated as Completed

### Completed
- terminal
- exact final integrity required by acceptance fixture
- deterministic cleanup

## Frozen engineering workflow

1. One small checkpoint at a time.
2. Every checkpoint reruns all previous accepted regressions.
3. A checkpoint is not accepted without `BUILD_OK` + `CONFORMANCE_OK`.
4. Real Windows/.NET 10 execution is the acceptance authority.
5. On first failure: stop, do not rerun, analyze exact first failure.
6. Never bypass a test by removing assertions, skipping branches, unsafe null suppression, or early-returning around failure.
7. Every candidate ZIP is extracted to a new clean folder.
8. Preserve sequence; do not jump to UI or unrelated features.
9. In normal Chat, avoid broad risky refactors; prefer isolated changes.

## Next checkpoint — CP-022

**Title:** Runtime Progress & Telemetry Contract

Planned fields per snapshot/event:
- Queue ID
- Job ID
- state
- trusted/downloaded bytes
- total bytes when known
- percentage when meaningful
- speed
- ETA when meaningful
- mode: Parallel / Single
- ordered timestamp/sequence

Required tests:
- correct parallel aggregation
- correct single-mode progress
- monotonic percentage during normal transfer
- Pause/Stop freeze after quiescence
- Resume continuity without bogus jumps
- exact 100% only on successful completion
- Failed/Canceled never emit false 100%
- no-Range semantics do not pretend old partial bytes were network-resumed
- throttled/UI-safe reporting
- correct Queue ID + Job ID tagging
- no cross-job/cross-queue telemetry leakage
- deterministic terminal snapshot

Explicitly not part of CP-022:
- WinUI
- SQLite migration
- background Worker process
- IPC
- browser extension
- update system
- UI localization
- release signing

## Resume phrase

`AX Downloader devam — CP-021 accepted, CP-022 Runtime Progress & Telemetry Contract’tan başla.`
