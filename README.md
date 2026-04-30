# Netplay OS Bridge

Netplay OS Bridge is a plan for cloud-hosted retro co-op where one authoritative
cloud machine runs the emulator or desktop app, while two players stream the
same session and send controller input back to that host.

The first build should avoid emulator-specific netplay as the default path. The
play loop should be simple and latency-sensitive:

1. Start one GPU-backed Windows host.
2. Launch RetroArch, Playnite, or another target app on that host.
3. Let both players connect with Parsec.
4. Treat each remote controller as a local controller on the host.
5. Keep uploads, library management, cost controls, and session orchestration in
   a separate browser control plane.

## Current Recommendation

Use a hybrid architecture:

- **Play plane:** Windows GPU VM on AWS, Parsec, RetroArch or Playnite.
- **Control plane:** Browser app for login, uploads, game library, session start,
  session stop, and health/cost visibility.
- **Library plane:** Start with S3-backed uploads and host sync; add RomM as the
  richer metadata/library layer after the streaming proof of concept is stable.
- **Compatibility plane:** Add Tailscale or another overlay network only for
  admin/private service access or rare apps that truly need peer networking.

## Planning Docs

- [Architecture Plan](docs/architecture-plan.md)
- [Cross-Tool Sync Plan](docs/cross-tool-sync-plan.md)
- [MVP Backlog](docs/mvp-backlog.md)

## Decision Snapshot

The MVP optimizes for stable, low-latency remote couch co-op over browser-only
purity. Browser-only streaming options like Amazon DCV or Kasm stay on the
roadmap as explicit spikes, not as the default path.

