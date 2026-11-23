# Diagnostic and App Group compatibility update — 2026-11-23

Base source: `Top1-Make 3-audited(2).zip`

## Changes

1. `worse_query.h` / `worse_query.m`
   - Added `worse_query_app_groups()` to expose every App Group identifier present in the running process's signed `com.apple.security.application-groups` entitlement.
   - Kept `worse_query_app_group()` and its existing first-usable-group behavior for compatibility with existing call sites.
   - This is entitlement inspection only. It does not grant access to groups absent from the signed entitlement.

2. `SandboxAccess.swift`
   - Added `appGroups` as the Swift-facing inventory of all entitled App Group identifiers.
   - Kept the existing `appGroup` property unchanged for legacy consumers.

3. `SandboxDiagnostics.swift`
   - Added a bounded ring of container-resolution stage records.
   - Diagnostic export now lists all signed-entitlement App Groups and recent resolution stages separately from sandbox grant records.

4. `ContainerStore.swift`
   - Records bundle-ID validation failures, MCM misses, rejected MCM paths, metadata-scan unavailability/no-match, and successful resolution.
   - Existing resolution order and path validation remain unchanged. No new container-discovery or privilege-escalation fallback was added.

5. `DevicePatchService.swift`
   - Records whether resolution failed before any target grant, whether a path was rejected, and whether a target grant succeeded or failed.
   - Keeps the existing transaction and grant flow. An App Group probe is not treated as proof that a target container was resolved or granted.

6. `Tests/run_worse_query_guardrails.sh`
   - Added static checks for the new App Group inventory and resolution diagnostics.

## Validation performed

- `Tests/run_worse_query_guardrails.sh`: PASS.
- Swift parser checks (`swiftc -frontend -parse`) for the four modified Swift files: PASS.
- ZIP structure and changed-file list inspected.

## Not performed / limitations

- No Xcode build was run because `xcodebuild` is unavailable in this environment.
- No iPhone/iOS 27 runtime test, signing, provisioning-profile validation, or sandbox grant test was performed.
- The `worse_query` grant implementation, query flags, token exchange, MCM bridge implementation, and patch target discovery behavior were not expanded or altered to bypass iOS sandbox restrictions.
- App Group compatibility remains bounded by the groups actually present in the app's signed entitlement and available at runtime.
