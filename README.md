# Leaf-Spine OSPF/BGP Lab

Network lab built with netlab, Containerlab, and FRRouting (FRR) to practice routing protocols used in data center fabrics.

## Status
- [x] Phase 1: Two-node OSPF baseline (complete)
- [x] Phase 2: Leaf-spine topology with OSPF (complete)
- [x] Phase 3: Leaf-spine with eBGP, one AS per router (complete)
- [x] Phase 4: Link and node failure testing (complete)

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

## Phase 3: Leaf-spine with eBGP
Same physical topology as Phase 2, but OSPF is removed and each router runs BGP in its own autonomous system: spine1 is AS 65000, spine2 is AS 65001, and the leaves are AS 65100, 65101, and 65102. netlab builds an eBGP session on every spine-leaf link because the two ends are in different ASes.

**Files:** `03-leaf-spine-ebgp/topology.yml` and `03-leaf-spine-ebgp/leaf-spine-bgp-verification.txt`

**Results**
- leaf1 has established eBGP sessions with both spines, and spine1 has established sessions with all three leaves.
- From leaf1, the routes to the other leaves' loopbacks and networks are learned via BGP, each with two equal-cost paths, one through each spine.
- BGP routes show an administrative distance of 20, compared with 110 for the OSPF routes in Phase 2.

## Phase 4: Failure testing
Failure tests on the Phase 3 eBGP fabric, run from leaf1 toward leaf2's network (`172.16.1.0/24`).

**Files:** `04-failure-testing/leaf-spine-failure-test.txt` (uses the topology from `03-leaf-spine-ebgp`)

**Test 1: link failure.** Shut down leaf1's link to spine1 (`ip link set eth2 down`).
- The BGP session to spine1 dropped to Active, and the path via spine1 became inactive.
- The route to `172.16.1.0/24` stayed installed through spine2, and pings from leaf1's loopback to leaf2's loopback still showed 0% packet loss.
- After bringing the link back up, both BGP sessions were established again and both equal-cost paths returned.

**Test 2: node failure.** Stopped the spine1 router (`docker stop`).
- The BGP session to spine1 dropped, leaving a single path through spine2.
- Pings from leaf1's loopback to leaf2's loopback still showed 0% packet loss.

**Notes**
- Pings use each router's loopback as the source (`ping -I`), because the fabric only advertises loopbacks and leaf networks, not the spine-leaf link subnets.
- Reconvergence time was not measured. A natural next step is a continuous ping during the failure to count lost packets.

## How to run
1. Open a GitHub Codespace from the ipspace/netlab-examples repo
2. Copy the `topology.yml` for the phase you want into a new folder
3. Run `netlab up`
4. Run `netlab connect <node>`, then `vtysh` to inspect a router
5. Run `netlab down` when finished

## Tools
netlab, Containerlab, FRRouting, Docker, GitHub Codespaces
