# 0001: Use One Authoritative Cloud Host

## Status

Accepted

## Context

The project goal is stable remote couch co-op for retro games and similar apps
without depending on emulator-specific netplay, Kaillera servers, router
changes, or per-player local emulator synchronization.

Classic netplay makes each player run a local emulator instance and then tries
to keep those instances synchronized over the network. That makes NAT,
emulator compatibility, latency, and desync part of the core play loop. The
project is trying to remove those sources of failure from the default path.

## Decision

The default play architecture uses one authoritative cloud machine. The
emulator or target app runs once on that host. Both players stream the same
video/audio session and send controller input back to that host.

The MVP should treat "two players on one remote machine" as the primary model.
Virtual LAN, emulator netplay, and browser-only shared desktop approaches remain
secondary compatibility paths until the core play loop is proven.

## Alternatives Considered

- Emulator netplay or Kaillera as the default: rejected because it keeps
  emulator synchronization, NAT traversal, and third-party netplay reliability
  in the critical path.
- Virtual LAN between two player machines: rejected as the default because it
  still requires separate app instances and does not remove desync risk.
- Two cloud machines on the same private network: rejected for MVP because it
  adds cost and orchestration without proving a better play experience.
- Browser-only shared desktop first: deferred because controller behavior and
  latency need to be validated separately from the low-latency Parsec path.

## Consequences

- The first proof of concept can focus on one host, one emulator instance, two
  controller inputs, and one measured streaming session.
- The frame loop stays short: controllers enter the host, the host renders, and
  the stream goes back to both players.
- Library uploads, host lifecycle, cost controls, and session management belong
  outside the frame loop.
- The host becomes the main reliability boundary, so display, audio, controller,
  GPU driver, and shutdown behavior need explicit runbooks.

## Follow-ups

- Run the M1 streaming proof of concept with a temporary Windows GPU host.
- Record region, instance type, Parsec metrics, controller models, game/core,
  and failure notes in the play-test checklist.
- Use the M1 result to decide whether the next implementation PR should be the
  Terraform baseline or a revised streaming spike.
