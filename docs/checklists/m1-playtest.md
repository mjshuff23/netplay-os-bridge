# M1 Play-Test Checklist

Use this checklist for each manual streaming proof-of-concept run.

## Session Metadata

- Date:
- Owner:
- Guest:
- AWS region:
- Instance type:
- AMI / OS image:
- Root disk size:
- Instance start time:
- Instance stop time:
- Approximate runtime:

## Access And Security

- [ ] Host tagged as temporary M1 POC infrastructure.
- [ ] Admin access path recorded.
- [ ] RDP is disabled or restricted to the owner IP.
- [ ] No secrets, IPs, account IDs, ROMs, or BIOS files committed to the repo.
- [ ] Instance stopped or terminated after the test.

## Host Setup

- [ ] Windows updates completed enough for the test.
- [ ] NVIDIA driver installed.
- [ ] GPU visible in Device Manager.
- [ ] Parsec installed.
- [ ] Parsec hosting enabled.
- [ ] Parsec Virtual Display Driver installed if needed.
- [ ] Virtual audio device installed if needed.
- [ ] RetroArch installed.
- [ ] Test game launches locally on the host.

Notes:

```text

```

## Controller Setup

- Player 1 controller model:
- Player 2 controller model:
- [ ] Owner controller visible to Windows.
- [ ] Guest controller visible to Windows.
- [ ] Controllers appear as distinct devices.
- [ ] RetroArch maps Player 1 to Port 1.
- [ ] RetroArch maps Player 2 to Port 2.

Controller notes:

```text

```

## Game Test

- Game:
- Platform:
- RetroArch core:
- Session length:
- Parsec bandwidth setting:
- Display resolution:
- Target FPS / refresh rate:

Observed metrics, if available:

- Encode latency:
- Decode latency owner:
- Decode latency guest:
- Network latency owner:
- Network latency guest:
- Packet loss:
- Dropped frames:

Subjective notes:

```text

```

## Stability

- [ ] No controller remapping during the session.
- [ ] No player lost input.
- [ ] No audio loss.
- [ ] No stream disconnect.
- [ ] No host sleep/lock interruption.
- [ ] Playability was acceptable to owner.
- [ ] Playability was acceptable to guest.

Failures or interruptions:

```text

```

## Instance Decision

- [ ] `g4dn.xlarge` is sufficient for the next phase.
- [ ] Retest with `g5.xlarge`.
- [ ] Retest in a different region.
- [ ] Retest after display/audio/controller changes.
- [ ] Revise the streaming strategy before Terraform.

Decision rationale:

```text

```

## Next Step

Choose exactly one:

- [ ] Proceed to M2 Infrastructure Baseline.
- [ ] Repeat M1 With Adjustments.
- [ ] Revise Streaming Strategy.

Follow-up links:

- GitHub issue:
- Linear issue:
- Notes or screenshots location:
