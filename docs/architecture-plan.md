# Netplay OS Bridge Architecture Plan

## Product Goal

Create a cloud-hosted retro gaming environment that feels like remote couch
co-op:

- A technical-but-not-engineer friend can join with credentials and minimal
  setup.
- Two players appear to the emulator as local players on the same machine.
- The system avoids router setup, emulator netplay lobbies, Kaillera servers,
  and fragile NAT traversal as the normal path.
- Uploading games and surfacing them in a library is automated enough that game
  night does not become infrastructure night.

## Architecture Decision

The default architecture is **one authoritative cloud host**, not a virtual LAN
of two separate emulator clients.

That keeps the frame loop short:

```text
Player 1 controller -> Parsec -> Cloud Windows host
Player 2 controller -> Parsec -> Cloud Windows host
Cloud Windows host -> encoded video/audio -> both players
```

Everything that is not in the frame loop belongs outside it:

- authentication
- uploads
- metadata scraping
- host lifecycle
- cost guardrails
- library scans
- observability
- project/admin access

## Primary Stack

### Play Plane

- AWS EC2 GPU instance, starting with `g4dn.xlarge` for the first proof of
  concept and testing `g5.xlarge` if encoding, CPU headroom, or emulator class
  requires it.
- Windows host OS.
- NVIDIA gaming/GRID driver path appropriate to the selected instance.
- Parsec native client as the primary low-latency play transport.
- Parsec Virtual Display Driver for headless Windows hosting.
- Virtual audio device if Windows/Parsec/audio capture needs it.
- RetroArch as the baseline emulator target.
- Playnite as the optional couch-friendly launcher.

### Control Plane

- Next.js + TypeScript browser app.
- Auth for owner and invited players.
- AWS SDK integration for session start, stop, status, upload URLs, and cost
  guardrails.
- Postgres for app state if/when the UI needs durable session/library metadata.
- Terraform for AWS infrastructure.

### Library Plane

MVP starts simple:

1. User uploads through the control plane.
2. Control plane creates an S3 presigned upload URL.
3. S3 object-created event triggers import processing.
4. Worker validates extension, size, checksum, and target platform.
5. Worker lands the file in the host library path or stages it for host sync.
6. Host scans the directory in RetroArch or Playnite.

After the streaming proof of concept works, add RomM for:

- richer browser library browsing
- metadata matching
- upload/library APIs
- lists and collection management
- a better long-term database-backed library surface

### Compatibility Plane

Overlay networking is not the default netplay strategy. Keep Tailscale or
similar tooling for:

- private admin access
- SSM or internal service reachability alternatives
- rare software that needs stable private addresses
- later experiments with true LAN/broadcast semantics

## MVP Acceptance Criteria

The proof of concept is successful when:

- Two players can connect to the same cloud host.
- RetroArch sees two distinct controllers.
- A couch co-op game can be played for at least 30 minutes without controller
  remapping, audio loss, or session collapse.
- The owner can start and stop the host without entering the AWS console.
- A test game upload can land in the configured library path.
- Idle shutdown prevents an accidentally running GPU instance.
- The selected region and instance type are documented with measured latency,
  bitrate, and subjective playability notes.

## Known Risks

- Cloud GPU capacity can vary by region and time.
- Windows Server images may need extra display/audio/controller setup.
- Browser-only streaming is attractive but needs a separate validation path for
  multi-controller reliability.
- Egress cost can dominate casual usage if bitrate and shutdown policies are not
  managed.
- ROM, BIOS, and firmware handling must assume users only upload content they
  have the right to use.

## Official References To Recheck During Build

- Parsec hardware/software compatibility:
  https://support.parsec.app/hc/en-us/articles/32381568346644-Hardware-and-Software-Compatibility
- Parsec cloud workstation setup:
  https://support.parsec.app/hc/en-us/articles/32381875912468-Cloud-Workstation-Setup-Guide
- Parsec Virtual Display Driver:
  https://support.parsec.app/hc/en-us/articles/32381178803604-Overview-Prerequisites-and-Installation
- AWS EC2 G4 instances:
  https://aws.amazon.com/ec2/instance-types/g4/
- AWS EC2 G5 instances:
  https://aws.amazon.com/ec2/instance-types/g5/
- AWS EC2 on-demand pricing:
  https://aws.amazon.com/ec2/pricing/on-demand/
- AWS S3 presigned uploads:
  https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html
- AWS Lambda S3 event processing:
  https://docs.aws.amazon.com/lambda/latest/dg/with-s3.html
- Amazon DCV Web Client SDK:
  https://docs.aws.amazon.com/dcv/latest/websdkguide/what-is.html
- Amazon DCV collaboration:
  https://docs.aws.amazon.com/dcv/latest/userguide/managing-sessions-session-collaboration.html
- Kasm gamepad pass-through:
  https://kasmweb.com/docs/latest/guide/gamepad_passthrough.html
- RomM API:
  https://docs.romm.app/latest/API-and-Development/API-Reference/
- Tailscale device connectivity:
  https://tailscale.com/docs/reference/device-connectivity

