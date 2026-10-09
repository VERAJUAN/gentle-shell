# Reviewer Transport Settings Propagation

Forward effective Pi transport preferences into individual and grouped native reviewer side-calls, preserving existing model routing, absent-transport defaults, and API compatibility.

Related public issue: https://github.com/Gentleman-Programming/gentle-shell/issues/1970

## Context

- Target branch: `fix/reviewer-transport-settings-public`
- Base commit: `8a7aa1c9b313c79a70f5d9f3f8071816f0c9e124`

## Implemented Flow

1. Capture effective settings transport preferences at execution time from the Pi session, preserving method receiver semantics.
2. Forward the transport value unchanged into single-reviewer and grouped-reviewer relay requests when defined, omitting undefined values.
3. Pass transport through in-process reviewer options into both composed-provider and compatibility completion callers.
4. Maintain default fallback behavior when transport is unset, without forcing any transport or altering model selection, timeouts, or cancellation admission.

## Historical Verification Evidence

Historical checks below were executed against a byte-identical implementation and recorded for tracking; they were not rerun during public candidate preparation. Prior review approval was historical to the upstream source work unit and does not represent approval for this public candidate branch.

### 1. Test-Driven Development (RED / GREEN)
- **Initial RED test run**: 30 new deterministic tests added across in-process reviewer and relay test suites.
  - Observed 24 missing-propagation failures (8 completion-option explicit-transport cases, 8 capture-relay execution-time cases, 4 fresh effective-settings reads, 4 relay-to-reviewer transport cases).
  - 6 compatibility and omission cases passed as expected on unchanged baseline.
- **Implementation GREEN run**: All 30 new tests passed after wiring transport inheritance.

### 2. Focused Regression Suite
- Total tests: 272 (269 passed, 3 known baseline failures unchanged, 0 skipped, 0 cancelled).
- Known baseline assertion failures (pre-existing in base tree, not regressions):
  - Startup invalid-project-routing notification: `tests/gentle-ai.test.ts:687`
  - Missing review-refuter routing refusal: `tests/review-host-relay-routing.test.ts:405`
  - Missing required group-role refusal: `tests/review-host-relay-routing.test.ts:1317`

### 3. Type Ratchet
- TypeScript no-emit type check reported 186 diagnostics with no regressions and 12 improved file/code diagnostic pairs relative to baseline (does not claim zero-diagnostic compilation).

### 4. Supplemental Verification
- 18 synthetic tests passed (18/18 across relay transport agent and restart parity suites).
- Syntax and git whitespace checks clean (`git diff --check`).

## Scope and Boundaries

### Modified Paths
- `extensions/gentle-ai.ts`: capture effective settings transport and supply to relay calls.
- `lib/review-host-relay.ts`: relay-level transport forwarding.
- `lib/inprocess-reviewer.ts`: reviewer completion options transport forwarding.
- `tests/inprocess-reviewer.test.ts`: in-process reviewer transport test coverage.
- `tests/review-host-relay.test.ts`: host relay transport test coverage.
- `tests/review-host-relay-routing.test.ts`: relay routing and group capture transport tests.
- `docs/readme-reference.md`: documentation note distinguishing transport preference inheritance from network transport guarantees.
- `odd/tasks/reviewer-transport-settings.md`: public tracking document.

### Pending Checks and Limitations
- Full repository test suite (`pnpm test`) was withheld due to unisolated loopback services and package packing requirements.
- Live SSE transport execution against active network endpoints remains unverified.
- Older binary compatibility and role-specific adversarial transport testing remain pending.
