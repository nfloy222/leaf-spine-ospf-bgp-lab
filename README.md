# Leaf-Spine OSPF/BGP Lab

Network lab built with netlab, Containerlab, and FRRouting (FRR) to practice routing protocols used in data center fabrics.

## Status
- [x] Phase 1: Two-node OSPF baseline (complete)
- [ ] Phase 2: Leaf-spine topology (2 spines, 3 leaves)
- [ ] Phase 3: OSPF underlay with eBGP peering
- [ ] Phase 4: Link and node failure testing

## Phase 1: Two-node OSPF baseline
Two FRR routers (r1, r2) connected by a point-to-point link, each with a stub network, running OSPF.

**Files**
- `topology.yml`: netlab topology definition
- `ospf-verification.txt`: `show ip ospf neighbor` and `show ip route` output from r1

**Result:** r1 formed a Full OSPF adjacency with r2 and learned r2's loopback and stub network via OSPF.

## How to run
1. Open a GitHub Codespace from the ipspace/netlab-examples repo
2. Copy `topology.yml` into a new folder
3. Run `netlab up`
4. Run `netlab connect r1`, then `vtysh` to inspect the routers
5. Run `netlab down` when finished

## Tools
netlab, Containerlab, FRRouting, Docker, GitHub Codespaces
