# EIGRP Neighbor and Topology Table

## Introduction

EIGRP (Enhanced Interior Gateway Routing Protocol) maintains two essential tables for its operation: the **Neighbor Table** and the **Topology Table**. Understanding these tables is crucial for network engineers to effectively manage EIGRP networks, optimize routing, and troubleshoot issues.

## Why are EIGRP Neighbor and Topology Tables Needed?

- **Neighbor Table:** The EIGRP Neighbor Table keeps track of all the directly connected routers that are running EIGRP and have successfully formed an adjacency. It ensures that EIGRP has a current and accurate view of its immediate neighbors, allowing for efficient routing and network stability.

- **Topology Table:** The EIGRP Topology Table holds all the routes that the router has learned from its neighbors. It contains both the best routes (known as successors) and backup routes (known as feasible successors) for each destination. This table enables EIGRP to quickly converge and select alternative paths in case of network changes or failures.

## What Do These Tables Include?

### EIGRP Neighbor Table

- **Neighbor IP Address:** The IP address of the directly connected EIGRP neighbor.
- **Interface:** The interface through which the neighbor is reachable.
- **Hold Time:** The time left before a neighbor is considered down if no Hello packets are received.
- **Uptime:** The duration for which the adjacency has been established.
- **Sequence Number:** To track packet delivery and ensure reliable communication.
- **Queue Count:** The number of EIGRP packets queued to be sent to the neighbor.
- **Retransmission Timeout (RTO):** The estimated time interval to retransmit EIGRP packets that are not acknowledged.

### EIGRP Topology Table

- **Destination Network:** The network destination that EIGRP has learned about.
- **Successor:** The best path to reach the destination network.
- **Feasible Successor:** Backup paths that satisfy the Feasibility Condition and can be used if the successor route fails.
- **Metric:** The EIGRP composite metric for the route (calculated using bandwidth, delay, reliability, load, and MTU).
- **Reported Distance (RD):** The distance to a destination as reported by a neighbor.
- **Feasible Distance (FD):** The best metric to reach a destination from the current router's perspective.

## Importance of the Neighbor and Topology Tables

- **Fast Convergence:** The Neighbor and Topology Tables allow EIGRP to quickly converge and adapt to changes in the network, minimizing downtime.
- **Efficient Routing:** These tables provide all necessary information to compute the best routes and backup routes, ensuring optimal path selection.
- **Loop Prevention:** EIGRP uses the Topology Table to maintain a loop-free topology, ensuring reliable and stable routing.

## ASCII Diagram of EIGRP Neighbor and Topology Table

```plaintext
+-----------------------+
|    EIGRP Router       |
+-----------------------+
|    Neighbor Table     |  <- Directly Connected Neighbors
|-----------------------|    (Established Adjacencies)
| Neighbor IP Address   | 
| Interface             |
| Hold Time             |
| Uptime                |
| Sequence Number       |
+-----------------------+
           |
           | EIGRP Packets (Hello, Update, etc.)
           v
+-----------------------+
|    Topology Table     |  <- Learned Routes
|-----------------------|    (Best and Backup Paths)
| Destination Network   |
| Successor             |
| Feasible Successor    |
| Metric (FD, RD)       |
+-----------------------+
```
### Checklist for Troubleshooting EIGRP
Commands to Check Neighbor and Topology Table

**Check EIGRP Neighbor Table:**

```bash

show ip eigrp neighbors
```
What to Expect: Lists all EIGRP neighbors, their IP addresses, interfaces, hold times, and uptime.

```plaintext

    Neighbor        V    AS   Msg Rcvd   Msg Sent   Queue   Seq Num
    192.168.1.2     4    100  250        240        0       15
```
**Check EIGRP Topology Table:**

```bash

show ip eigrp topology
```
What to Expect: Displays all routes known to EIGRP, showing successors and feasible successors.

```plaintext

    P 10.1.1.0/24, 1 successors, FD is 30720
        via 192.168.1.2 (30720/28160), Serial0/0
```
**Detailed EIGRP Topology Table:**

```bash

    show ip eigrp topology all-links
```
What to Expect: Shows all paths, including those not meeting the Feasibility Condition.

### Debug Commands for EIGRP Troubleshooting

**Debug EIGRP Neighbors:**

```bash

debug eigrp neighbors
```
What to Expect: Displays real-time information about EIGRP neighbor formation and state changes.

**Debug EIGRP Packets:**

```bash

    debug eigrp packets
```
What to Expect: Shows all EIGRP packets sent and received by the router.

### Common Issues and Steps to Resolve Them

**Mismatched AS Numbers:**
-  Issue: Routers with different EIGRP AS numbers will not form a neighbor adjacency.
-  Resolution: Ensure both routers are configured with the same AS number using:

```bash

    router eigrp 100
```
**Mismatched K-Values:**

-  Issue: Routers with different K-values will not form a neighbor adjacency.
-  Resolution: Match K-values on both routers:

```bash

    router eigrp 100
    metric weights 0 1 0 1 0 0
```
**Interface Issues:**

-  Issue: The interface between routers might be down or misconfigured.
-   Resolution: Check interface status and configuration:

```bash

    show ip interface brief
```
**Authentication Mismatch:**

-  Issue: EIGRP authentication must match on both sides.
-   Resolution: Configure the correct authentication key on both routers:

```bash

    key chain EIGRP_KEYS
    key 1
    key-string mypassword
    interface GigabitEthernet0/1
    ip authentication mode eigrp 100 md5
    ip authentication key-chain eigrp 100 EIGRP_KEYS
```
**Network Statements:**

-  Issue: EIGRP is not enabled on the correct interfaces due to missing or incorrect network statements.
-  Resolution: Verify and correct network statements under EIGRP configuration:

```bash

    router eigrp 100
    network 192.168.1.0 0.0.0.255
```
**Access Control Lists (ACLs):**

    Issue: ACLs or firewalls may block EIGRP packets.
    Resolution: Review ACLs and firewall rules to ensure EIGRP traffic is allowed:
