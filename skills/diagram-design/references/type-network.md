# Network Topology

**Best for:** physical or logical network structure — routers, switches, firewalls, access points, and the subnets/VLANs/links that connect them. Answers "how does traffic get from A to B, and through what boundary." Architecture answers "what talks to what" at the application/service level; deployment answers "what runs on which host, in which environment." Network topology answers "what wire, what subnet, what device, what protocol" — if the diagram has no device (router/switch/firewall/AP), no subnet/VLAN boundary, and no link-level detail (interface, protocol, speed), it isn't this type; use Architecture or Deployment instead.

## Layout conventions

Two nesting levels, outermost to innermost — reuses the containment grammar from `type-nested.md` and the zone grammar from `type-architecture.md`, specialized for L2/L3 topology:

1. **Subnet / VLAN zone** — a network segment or broadcast domain (`10.0.10.0/24`, `VLAN 20 — IoT`, `DMZ`). A rect, `rx=8`, fill `ink @ 0.02`, stroke `ink @ 0.20` dashed `4,4` — identical treatment to the deployment zone. A Geist Mono 8px uppercase tracked eyebrow label (the subnet CIDR or VLAN name) sits in the top-left corner on a paper-colored mask over the border. Zones are drawn **first**, before links and devices (z-order: bg → zones → links → labels → devices).
2. **Device node** — a router, switch, firewall, access point, server, or endpoint. The §6 node-box pattern (SKILL.md) at `rx=6`, with a rectangular type tag (`rx=2`, **not** a pill) in the top-left corner: `RTR` / `SW` / `FW` / `AP` / `SRV` / `EP`. Use the matching icon from `primitive-icons.md` (`router`, `switch`, `access-point`, `firewall`) at 16–20px beside the device name where an icon reads faster than the tag alone — don't use both a tag and an icon redundantly on a tightly-budgeted diagram; pick one per device.

**Interface / port label.** Where a link attaches to a device, a small Geist Mono 7px label sits just outside the device's edge (not inside the box) giving the interface or port (`eth0`, `Gi0/1`, `:443`) when it matters to the story — e.g. a WAN uplink port, a trunk port carrying multiple VLANs. Omit when every link on a device is equivalent and the interface adds no information (an unmanaged switch's downstream ports, for instance).

**Links.** Orthogonal elbows between devices (SKILL.md §6, elbow formula in `type-architecture.md`), labeled with protocol/speed/VLAN in Geist Mono 8px (`1G`, `10G`, `VLAN 20`, `OSPF`, `IPSec`). A link that crosses a subnet/VLAN boundary is `link`-blue; a link that stays inside one zone is `muted`. A redundant, failover, or backup path is dashed `5,4`. Never leave a link unlabeled — protocol and speed are the content of a network diagram, not decoration.

**Focal rule.** The 1–2 accent elements are the single point of failure, the newly introduced device, or the boundary under discussion — commonly the firewall or the WAN uplink. In the canonical example: the edge firewall (accent-tint fill, accent stroke) and its WAN uplink (accent, solid, labeled `WAN`).

**Security / boundary devices.** A firewall (or other inline security control) uses the Security/Boundary treatment from SKILL.md §5 (`accent @ 0.05` fill, `accent @ 0.50` stroke dashed `4,4`) only when it is the diagram's focal point; otherwise it uses the standard device node-box like any other device — don't double-mark it as both a focal accent and a dashed security boundary unless it genuinely carries both meanings (the device *and* the routed edge crossing it).

**Aggregating a LAN segment.** When the individual hosts behind a switch aren't the point — only the segment's existence and its uplink are — draw one device node for the switch/AP and label it with a host-count sublabel (`24 hosts`, `+12 clients`) instead of drawing every endpoint. This keeps the diagram inside budget and matches the "delete first" philosophy (SKILL.md §1).

## Complexity budget

| Limit | Rule |
|---|---|
| Max subnet/VLAN zones | 4 |
| Max devices | 9 |
| Max links | 12 |
| Max accent elements | 2 |
| Max aggregated-host badges | no limit (they replace nodes, not add them) |

Over budget → split into one network diagram per site, per VLAN group, or overview + detail (core topology vs. one segment's detail).

## Anti-patterns

- Redrawing the application architecture with network-sounding labels bolted on — if there's no device, no subnet boundary, and no link-level detail, use `type-architecture.md` instead.
- Conflating this with Deployment — deployment answers "what software runs where"; network topology answers "what wire connects what device." A diagram trying to do both usually means splitting into two.
- Unlabeled links — protocol and speed (or VLAN) are the entire content of a link; an unlabeled line is a wasted connector.
- One node drawn per host on a LAN segment instead of a single switch/AP node with a host-count badge.
- Cloud-icon-soup standing in for a named device — name the router/switch/firewall, don't decorate it.
- Zones that are just visual grouping with no real subnet/VLAN boundary — a zone must correspond to an actual broadcast domain or routed segment, not a layout convenience.
- More than 2 accent elements — a network diagram with every device highlighted has no focal point left.
- Interface labels on every port when only one or two ports matter to the story — label the WAN uplink and the trunk port, not every access port.

## Examples

- `assets/example-network.html` — minimal light
- `assets/example-network-dark.html` — minimal dark
- `assets/example-network-full.html` — full editorial
