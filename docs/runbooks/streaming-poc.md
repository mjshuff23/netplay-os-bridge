# M1 Streaming Proof Of Concept Runbook

## Goal

Validate the core play loop before building infrastructure automation:

```text
Player 1 controller -> Parsec -> Windows GPU host
Player 2 controller -> Parsec -> Windows GPU host
Windows GPU host -> RetroArch -> Parsec video/audio stream -> both players
```

The proof of concept is successful when two players can complete a 30-minute
co-op session with stable audio/video and two distinct controller inputs.

## Prerequisites

- AWS account with permission to launch GPU EC2 instances.
- Parsec account for the host owner and guest access path.
- Two working controllers available on the player machines.
- A legally owned test game and any required BIOS/firmware.
- A way to administer the Windows host during setup, preferably SSM Session
  Manager or temporary RDP restricted to the owner IP.
- The checklist at `docs/checklists/m1-playtest.md`.
- The comparison notes at `docs/research/aws-region-instance-notes.md`.

Do not commit secrets, account IDs, IP addresses, Parsec credentials, ROMs,
BIOS files, screenshots of credentials, or billing account details.

## Step 1: Choose The First Test Region

1. Record both players' approximate locations in the research notes.
2. Pick two or three nearby AWS regions to compare.
3. Estimate latency from both players to each candidate region.
4. Choose the first region that gives the best worst-player latency.
5. Record the reason for the region choice before launching anything.

If the players are in the eastern United States, start by comparing `us-east-1`
and `us-east-2`. If either player is far from those regions, choose candidates
that better balance both players.

## Step 2: Launch A Temporary Windows GPU Host

1. Start with `g4dn.xlarge`.
2. Use `g5.xlarge` only if `g4dn.xlarge` is unavailable, quota-blocked, or the
   play test shows insufficient headroom.
3. Use a Windows Server image that supports the required NVIDIA and Parsec host
   setup.
4. Allocate enough root disk space for Windows updates, tools, RetroArch, and
   one test game. Use the research notes to record the chosen size.
5. Keep inbound access narrow:
   - Prefer SSM Session Manager when possible.
   - If RDP is needed, restrict it to the owner's current IP and remove it after
     setup.
6. Tag the instance clearly as a temporary M1 streaming POC host.

Stop or terminate the instance immediately after the test window. Do not leave
GPU capacity running unattended.

## Step 3: Install Host Prerequisites

On the Windows host:

1. Install current Windows updates that are required before GPU or Parsec setup.
2. Install the NVIDIA driver path appropriate for the selected AWS GPU instance.
3. Confirm Windows can see the GPU in Device Manager.
4. Install Parsec.
5. Sign in to Parsec with the host owner account.
6. Enable hosting in Parsec.
7. Install Parsec Virtual Display Driver if the session is headless or display
   behavior is unstable.
8. Add a virtual audio device only if RetroArch or Parsec cannot capture audio.
9. Install RetroArch.
10. Create a simple working directory, for example:
    - `C:\netplay-os-bridge\roms`
    - `C:\netplay-os-bridge\notes`

Record every manual step that was required. Anything surprising becomes input
for M2 or M5.

## Step 4: Prepare RetroArch

1. Launch RetroArch on the host.
2. Update assets, controller profiles, databases, and core info files if
   needed.
3. Install one core for the selected legal test game.
4. Place the test game in the temporary ROM directory.
5. Confirm the game launches locally on the host.
6. Leave RetroArch in a known state before inviting the guest.

Do not optimize shader, frontend, or library polish during this proof of
concept. The goal is controller and stream viability.

## Step 5: Connect Owner And Guest Through Parsec

1. Owner connects to the host with Parsec.
2. Guest connects through the agreed Parsec access path.
3. Confirm the host sees both connected sessions.
4. Confirm the guest has controller permission.
5. Open Windows controller diagnostics if needed.
6. Verify controller 1 and controller 2 are distinct at the Windows level.
7. Launch RetroArch and verify controllers map to Port 1 and Port 2.

If the controllers collapse into one device or one player controls both ports,
stop and record the failure before changing too many variables.

## Step 6: Run The 30-Minute Play Test

1. Start the selected couch co-op game.
2. Play for at least 30 minutes.
3. Keep the Parsec metrics overlay available if possible.
4. Record:
   - region
   - instance type
   - OS image
   - GPU driver path
   - Parsec display settings
   - Parsec bandwidth setting
   - controller models
   - RetroArch core
   - game tested
   - encode/decode/network latency if visible
   - packet loss or dropped frame notes
   - audio stability
   - subjective playability from both players

Do not tune endlessly during the first run. Make one baseline run, record it,
then decide whether a second run is worth the cost.

## Step 7: Stop The Host And Record Cost Notes

1. Exit RetroArch.
2. Disconnect the guest.
3. Disconnect the owner.
4. Stop the EC2 instance.
5. Verify the instance is stopped in the AWS console.
6. Record approximate elapsed runtime.
7. Record whether the instance should be kept stopped for follow-up or
   terminated.

## Step 8: Make The M1 Decision

Use the checklist to choose one result:

- **Proceed to M2 Infrastructure Baseline:** the play loop worked well enough
  and only needs automation/hardening.
- **Repeat M1 With Adjustments:** the path is promising but needs a different
  region, instance type, driver, display, audio, or controller setup.
- **Revise Streaming Strategy:** Parsec/Windows/controller behavior failed in a
  way that makes the default architecture questionable.

Record the decision in the checklist and link it from GitHub issue `#2` and
Linear `TSH-52`.
