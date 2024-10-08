# EIGRP Neighborship Adjacency Formation

## Overview

EIGRP (Enhanced Interior Gateway Routing Protocol) neighbor adjacency formation is crucial for the protocol's operation. EIGRP routers must establish and maintain neighbor relationships to exchange routing information and ensure network convergence.

### EIGRP Neighborship Formation Steps

1. **Neighbor Discovery**: Routers send Hello packets to discover neighbors.
2. **Establishing Adjacency**: Neighbors must agree on specific parameters to form an adjacency.
3. **Exchanging Routing Information**: Routers exchange Update packets containing routing information.
4. **Maintaining Neighborship**: Routers send periodic Hello packets to maintain the adjacency.

### EIGRP Neighborship Formation

```
+---------------------+                    +---------------------+
|     Router A        |                    |     Router B        |
| AS: 100             |                    | AS: 100             |
| K-Values: 1,0,1,0,1 |                    | K-Values: 1,0,1,0,1 |
| Interface: G0/0     |                    | Interface: G0/0     |
+--------+------------+                    +--------+------------+
         |                                      |
         | Send Hello Packet                    | Receive Hello Packet
         |------------------------------------->|
         |                                      |
         | Receive Hello Packet                 | Send Hello Packet
         |<-------------------------------------|
         |                                      |
         | Add to Neighbor Table                | Add to Neighbor Table
         | Establish Bidirectional Adjacency     | Establish Bidirectional Adjacency
         |                                      |
         | Send Update Packet                   | Send Update Packet
         |------------------------------------->|
         |                                      |
         | Send ACK Packet                      | Send ACK Packet
         |<-------------------------------------|
         |                                      |
         | Adjacency Formed                     | Adjacency Formed
+--------+--------+                    +--------+--------+
|     Router A    |                    |     Router B    |
+-----------------+                    +-----------------+
```
### Conditions for Successful Adjacency Formation

1. **Matching Autonomous System (AS) Number:** Routers must be in the same EIGRP AS.
2. **Matching K-Values:** The K-values used for metric calculation must match.
3. **Authentication:** If configured, authentication must match between routers.
4. **IP Reachability:** Routers must be reachable at Layer 3.
5. **Interface State:** Interfaces must be up and not configured as passive.

### Troubleshooting EIGRP Neighborship

If EIGRP neighbors are not forming or are unstable, follow this checklist:

1. **Check AS Numbers:** Ensure that the EIGRP AS numbers match on both routers.
2. **Verify K-Values:** Confirm that the K-values are identical on both routers.
3. **Check IP Reachability:** Verify that there is Layer 3 connectivity between the routers.
4. **Review Interface States:** Ensure the interfaces are up and not configured as passive.
5. **Examine Authentication Settings:** If authentication is configured, check that the keys match.
6. **Check for ACLs or Firewalls:** Ensure no ACLs or firewalls are blocking EIGRP traffic i.e. **IP Portocol 88**.
7. **Inspect the Hold and Hello Timers:** Ensure timers are configured correctly and are compatible.
8. **Review EIGRP Debug Output:** Use debug commands to see if there are errors in forming neighborships.

### Commands to Run and What to Expect

**Check EIGRP Neighbors**

``` plaintext 
show ip eigrp neighbors
```
Expected Output: List of EIGRP neighbors with the state "up."
```plaintext 
Neighbor ID     Interface       Hold   Uptime   SRTT   RTO   Q  Seq
192.168.1.2     Gi0/0           12     **00:10:45** 1      100   0  3
```
**Verify EIGRP Topology**
```plaintext 
show ip eigrp topology
```
Expected Output: Displays EIGRP routes in the topology table, showing successors and feasible successors.
```plaintext 
P 192.168.1.0/24, 1 successors, FD is 28160
   via 192.168.1.2 (28160/2576), GigabitEthernet0/0
```
**Check EIGRP Interface Status**
```plaintext 
show ip eigrp interfaces
```
Expected Output: Lists interfaces where EIGRP is enabled.
```plaintext 
GigabitEthernet0/0 is up, line protocol is up
IP-EIGRP 100 for AS(100)
```
**Verify EIGRP Parameters**
```plaintext 
show ip protocols
```
Expected Output: Confirms EIGRP configuration, including AS number and network statements.
```plaintext 
Routing Protocol is "eigrp 100"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Default networks flagged in outgoing updates
  Default metric weights are K1=1, K2=0, K3=1, K4=0, K5=0
```
**Inspect EIGRP Authentication**
```plaintext 
show ip eigrp neighbors detail
```
Expected Output: Details on EIGRP neighbors, including authentication status.
```plaintext 
Neighbor 192.168.1.2, Interface: GigabitEthernet0/0
Authentication mode is enabled
```
**Debug EIGRP Packets**
```plaintext 
debug eigrp packets
```
Expected Output: Detailed output on EIGRP packets being sent and received; useful for diagnosing neighborship issues.
```plaintext 
EIGRP: Sending HELLO on GigabitEthernet0/0
EIGRP: Received HELLO on GigabitEthernet0/0 from 192.168.1.2
```
