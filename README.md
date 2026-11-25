# Leaf-Spine OSPF/BGP Lab

Network lab built with netlab, Containerlab, and FRRouting (FRR) to practice routing protocols used in data center fabrics.

## Status
- [x] Phase 1: Two-node OSPF baseline (complete)
- [x] Phase 2: Leaf-spine topology with OSPF (complete)
- [ ] Phase 3: eBGP peering on top of the OSPF underlay
- [ ] Phase 4: Link and node failure testing

## Phase 1: Two-node OSPF baseline
Two FRR routers (r1, r2) connected by a point-to-point link, each with a stub network, running OSPF.

**Files:** `01-two-node-ospf/topology.yml` and `01-two-node-ospf/ospf-verification.txt`

**Result:** r1 formed a Full OSPF adjacency with r2 and learned r2's loopback and stub network via OSPF.

## Phase 2: Leaf-spine with OSPF
Five FRR routers: 2 spines and 3 leaves. Every leaf connects to both spines, and each leaf has a stub network. All routers run OSPF.

**Files:** `02-leaf-spine/topology.yml` and `02-leaf-spine/leaf-spine-verification.txt`

**Results**
- Each leaf has Full OSPF adjacencies with both spines.
- Each spine has Full adjacencies with all three leaves.
- From leaf1, the routes to the other leaves' networks have two equal-cost paths, one through each spine (ECMP).

## How to run
1. Open a GitHub Codespace from the ipspace/netlab-examples repo
2. Copy the `topology.yml` for the phase you want into a new folder
3. Run `netlab up`
4. Run `netlab connect <node>`, then `vtysh` to inspect a router
5. Run `netlab down` when finished

## Tools
netlab, Containerlab, FRRouting, Docker, GitHub Codespaces
