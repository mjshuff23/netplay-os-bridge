# 0002: Use Parsec First For The MVP Play Path

## Status

Accepted

## Context

The project optimizes for stable, low-latency retro gaming with a technical
friend who should not need to do router setup or software-engineering work.
The guest can tolerate a small amount of setup if it buys a better play
experience.

The current architecture needs a streaming and input path that can make a
remote controller appear local to the host. Browser-only options are appealing,
but their multi-controller behavior and latency feel still need project-specific
validation.

## Decision

Use Parsec native client on a Windows GPU host as the MVP play path.

The first proof of concept should validate:

- owner and guest Parsec access to the same host
- two distinct controllers visible to Windows and RetroArch
- stable video and audio for a 30-minute session
- whether `g4dn.xlarge` is enough or `g5.xlarge` is justified

Amazon DCV and Kasm remain later browser-only spikes. They should not block the
Parsec-first proof of concept.

## Alternatives Considered

- Amazon DCV first: deferred because browser access and AWS integration are
  attractive, but two-player controller behavior needs validation before it can
  replace Parsec as the default.
- Kasm first: deferred because browser-only access is attractive, but Linux
  container/workspace constraints may narrow app compatibility.
- Sunshine or Moonlight first: deferred because the current MVP values the most
  proven low-friction controller-sharing path over open-source control.
- Remote desktop administration tools first: rejected for play sessions because
  admin access and low-latency game streaming are different jobs.

## Consequences

- The first host target is Windows with GPU driver, Parsec, RetroArch, display,
  audio, and controller setup.
- The guest experience can assume installing or using Parsec for the first MVP.
- Browser-only access remains a product goal candidate, not an MVP requirement.
- The control plane can focus on host lifecycle and instructions before it owns
  the low-latency stream.

## Follow-ups

- Use the M1 runbook and checklist to collect real Parsec play-test evidence.
- Document any required virtual display, audio, or controller setup discovered
  during the proof of concept.
- Revisit DCV and Kasm in M6 only after the Parsec baseline is known.
