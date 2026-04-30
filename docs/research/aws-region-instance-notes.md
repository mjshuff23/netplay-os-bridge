# AWS Region And Instance Notes

Use this file as a template while preparing and running M1. Keep sensitive
values out of the repo.

## Official References

- AWS EC2 G4 instances: https://aws.amazon.com/ec2/instance-types/g4/
- AWS EC2 G5 instances: https://aws.amazon.com/ec2/instance-types/g5/
- AWS EC2 on-demand pricing: https://aws.amazon.com/ec2/pricing/on-demand/
- Parsec hardware/software compatibility: https://support.parsec.app/hc/en-us/articles/32381568346644-Hardware-and-Software-Compatibility
- Parsec cloud workstation setup: https://support.parsec.app/hc/en-us/articles/32381875912468-Cloud-Workstation-Setup-Guide
- Parsec Virtual Display Driver: https://support.parsec.app/hc/en-us/articles/32381178803604-Overview-Prerequisites-and-Installation

Recheck these links before launching a paid instance. Pricing, quotas, drivers,
and availability can change.

## Player Location Inputs

Do not commit exact home addresses or public IPs.

| Player | Approximate region | Network notes |
| --- | --- | --- |
| Owner | TBD | TBD |
| Guest | TBD | TBD |

## Candidate Regions

Pick the region that minimizes the worst-player latency, not the region that is
best for only one player.

| AWS region | Owner latency | Guest latency | GPU quota available | GPU capacity available | Notes |
| --- | --- | --- | --- | --- | --- |
| TBD | TBD | TBD | TBD | TBD | TBD |
| TBD | TBD | TBD | TBD | TBD | TBD |
| TBD | TBD | TBD | TBD | TBD | TBD |

## Instance Candidates

Start with `g4dn.xlarge`. Use `g5.xlarge` only when the POC shows a reason or
`g4dn.xlarge` is blocked.

| Instance | Role | Result | Notes |
| --- | --- | --- | --- |
| `g4dn.xlarge` | Default first test | TBD | Lower-cost first validation target. |
| `g5.xlarge` | Fallback / comparison | TBD | Try if more GPU headroom is needed. |

## Launch Notes

- Selected region:
- Selected instance:
- Selected Windows image:
- Root disk size:
- GPU driver path:
- Admin access path:
- Display setup:
- Audio setup:
- Parsec bandwidth setting:

## Capacity And Quota Notes

Record quota or capacity blockers here.

```text

```

## Playability Summary

Summarize the best run after completing `docs/checklists/m1-playtest.md`.

```text

```

## Decision

Choose one:

- [ ] Use `g4dn.xlarge` for the M2 Terraform baseline.
- [ ] Use `g5.xlarge` for the M2 Terraform baseline.
- [ ] Repeat M1 with another region or setup.
- [ ] Revise streaming strategy before infrastructure work.

Rationale:

```text

```
