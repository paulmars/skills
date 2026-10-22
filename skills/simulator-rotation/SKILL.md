---
name: devicehub-simulators
description: Use when inspecting, choosing, reusing, or cleaning up Device Hub simulators across projects and agent sessions.
---

# Device Hub Simulators

Optimize for available RAM, low disk activity, and reuse. Expect roughly six concurrent agents across projects and worktrees. Keep at most 30 agent-managed simulators on disk; create devices only when needed. Let the agent choose suitable reuse or replacement based on the observed state.

## Inspect first

    xcrun simctl list devices --json

Check UDID, runtime/version, device type, availability, boot state, and recorded last boot. Compare this with the shared usage log before choosing a device. Inspection and histogram requests are read-only.

“Last activity” is the most recent actual start or use. Creation, inspection, shutdown, and release do not count as use. Report unknown activity history honestly.

## Keep an inspectable usage log

Use one shared JSON file across all agent types and projects:

    ~/.devicehubsimlog

Key device records by UDID. Record project/worktree, session ID, last-started and last-used UTC timestamps, and whether the session is still using the device. Use a unique session ID that stays stable across commands and conversation turns.

Use an exclusive advisory `flock` on `~/.devicehubsimlog.lock` while checking ownership, claiming a device, or updating records. Keep that lock file in place and write JSON updates atomically. A device can be shut down while still reserved by another session. Verify abandoned ownership before clearing it.

## Choose and use a device

Prefer compatible existing devices and reuse the same UDID during a session when practical. Claim it in the log before use; never interfere with another active session. The agent chooses among free compatible devices using the inventory and usage history.

If a required configuration is unavailable, create it within the 30-device limit. When replacement is needed, this policy permits deleting an agent-managed, unreserved, shutdown device whose last start/use was strictly more than three days ago. Recheck ownership and activity before deletion. Unknown history alone does not establish eligibility. Confirm the required runtime is installed before replacing anything.

Target the chosen UDID explicitly. Reuse a stable DerivedData directory per worktree and avoid concurrent builds writing to the same directory. Avoid extra parallel simulator workers when other agents are testing.

## Shut down after tests

**RAM matters: immediately shut down the simulator after each test invocation, including its retries, finishes.** Arrange cleanup for success, failure, timeout, and interruption. Stop remaining test processes first, preserve the test result, then:

    xcrun simctl shutdown <UDID>

Verify its state is `Shutdown`. Keep the reservation between runs when the session still needs it, but leave the device off while doing other work. Clear ownership when the session is finished. Update actual usage timestamps without counting shutdown or release as new activity.

Report failed cleanup with the UDID. Never shut down all devices or terminate Device Hub or shared services globally.

When reporting usage, show device, configuration, state, owning session, and last activity. Explain significant creation or replacement decisions briefly.
