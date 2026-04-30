# MVP Backlog

## M0 Planning Sync

- Create Notion project page and decision log.
- Create Figma architecture file with initial system and sequence diagrams.
- Create Linear project and milestones.
- Open a GitHub planning PR that links all artifacts.
- Decide naming conventions for branches, PRs, ADRs, and Linear issues.

## M1 Streaming Proof Of Concept

- Prep docs:
  - [Streaming POC Runbook](runbooks/streaming-poc.md)
  - [M1 Play-Test Checklist](checklists/m1-playtest.md)
  - [AWS Region And Instance Notes](research/aws-region-instance-notes.md)
- Pick the first AWS region based on both players' locations.
- Provision a single Windows GPU EC2 instance manually, without committed
  Terraform.
- Install NVIDIA driver, Parsec, RetroArch, and baseline audio/display support.
- Validate Parsec host access for owner and guest.
- Validate two distinct controller mappings in RetroArch.
- Run a 30-minute co-op play test and record latency, bitrate, instance type,
  region, controller model, and failure notes.
- Decide whether `g4dn.xlarge` is enough or whether `g5.xlarge` is worth the
  extra cost.

## M2 Infrastructure Baseline

- Add Terraform project structure.
- Define VPC, security groups, IAM roles, S3 buckets, and EC2 profile.
- Add SSM Session Manager access for administration.
- Remove public RDP from the normal path.
- Add start/stop scripts or AWS Systems Manager documents.
- Add idle shutdown protection.
- Add budget alarm or cost alerting.

## M3 Control Plane Shell

- Scaffold a Next.js TypeScript app.
- Add authentication.
- Add owner dashboard with host status.
- Add start/stop controls.
- Add session instructions for a friend.
- Add basic event/audit log.

## M4 Upload And Library Pipeline

- Add presigned S3 upload flow.
- Add import worker triggered by object-created events.
- Validate file size, extension, checksum, and target platform.
- Sync imported files to the Windows host library path.
- Add RetroArch or Playnite scan procedure.
- Spike RomM integration and decide whether it becomes the canonical library UI.

## M5 Streaming Hardening

- Lock down Parsec permissions and approved apps.
- Document host setup automation.
- Add virtual display/audio setup to provisioning.
- Add controller mapping recovery notes.
- Add region/instance comparison notes.
- Add recovery process for failed imports and stuck host sessions.

## M6 Browser-Only Alternative Spike

- Test Amazon DCV session collaboration and web client behavior.
- Test Kasm gamepad pass-through and shared session behavior.
- Compare controller reliability, latency feel, browser friction, and app
  compatibility against Parsec.
- Decide whether browser-only becomes a later mode or stays out of scope.
