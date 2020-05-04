# Gossip-Membership-Protocol

## What is Gossip protocol?

Gossip protocol is used to periodically send multicast messages to all the nodes of the group. The sender periodically picks **b** targets at random and send them copies of the multicast using a gossip message. After receiving the gossip messages the node is said to be infected and it then periodicaly send the message further in the group. The gossip message can be transmitted using UDP as gossip protocol itself is reliable.

## What is Membership Protocol?

Membership protocols are used for detecting failures in a distributed system and give information about which processes are running in the system, left the system or have failed.

The membership protocol performs two tasks:  
**1. Failure Detection** : It helps to detect if any process fails in the system.  
**2. Dissemination** : It helps to transfer this information to the membership list of other processes so that they can update it.

### Properties of failure detectors

**1. Completeness** : Each failure in the system should be detected by a non-faulty process. The protocol should be able to ensure 100% completeness.  
**2. Accuracy** : There should be no mistaken detection. Accuracy should be close to 100%.  
**3. Speed** : The time taken to detect the first failure should be as low as possible.  
**4. Scale** : There should be equal load on the nodes and avoid single point of contacts.  

Completeness and accuracy together can’t be achieved in lossy networks. The failure detector should be able to take care of those cases as well in which when one process fails and simultaneously other fails. This should also be detected speedily.

To detect failures in the system, membership protocol can use different types of heart-beating techniques like centralized heartbeating, ring hearbeating, all to all heartbeating, gossip style heartbeating.

### Gossip style membership

![gossip](https://github.com/mehak0108/Gossip-Membership-Protocol/blob/master/Pictures/gossip.png)

The nodes are connected on the network and periodically gossip membership list to other nodes. Example in this figure process P2 receives the list from P1 and compares it's list with that of P1. If the heartbeat of any process in the membership list of P2 is different from that of P1 then P2 updates the heartbeat and marks it with it's local time-stamp.

## Problem Statement

Implement a membership protocol.Since it is infeasible to run a thousand cluster nodes (peers) over a real network, we are providing you with an implementation of an emulated network layer (EmulNet). Your membership protocol implementation will sit above EmulNet in a peer- to-peer (P2P) layer, but below an App layer.
Think of this like a three-layer protocol stack with Application, P2P, and EmulNet as the three layers (from
top to bottom). More details are below.  
Your protocol must satisfy: i) Completeness all the time: every non-faulty process must detect every node
join, failure, and leave, and ii) Accuracy of failure detection when there are no message losses and message
delays are small. When there are message losses, completeness must be satisfied and accuracy must be
high. It must achieve all of these even under simultaneous multiple failures.  

Two message types are currently defined for the P2P layer (MP1Node.cpp implementation) - JOINREQ and JOINREP. Currently, JOINREQ messages are received by the introducer. The introducer is the first peer to join the system (for Linux, this is typically 1.0.0.0:0, due to the big-endianness).  

Here are the functionalities your implementation must have:  
**Introduction:** Each new peer contacts a well-known peer (the introducer) to join the group. This is implemented through JOINREQ and JOINREP messages. Currently, JOINREQ messages reach the introducer, but JOINREP messages are not implemented. JOINREP messages should specify the cluster member list. The introducer does not need to maintain a list of all peers currently in the system; a partial list of fixed size can be maintained.  
**Membership:** You need to implement a membership protocol that satisfies completeness all the time (for joins and failures), and accuracy when there are no message delays or losses (high accuracy when there are losses or delays). We recommend implementing either gossip-style heartbeating or SWIM-style membership, although all to all heartbeating would be fine too (though you’d learn less).  

## Detailed Solution

Initially the processes while connecting on the network introduce themselves to the group. If there is no group  available then that process becomes the group booter and boots up the group. If processes are already present in the group then it sends a JOINREQ to them.  

There is a message queue for each of the processes which contains different types of messages like reply for join request (JOINREP), creating join request (JOINREQ) and ping messages. Consider a process P1 from the group with it's message queue. Now If message type received by P1 is JOINREQ then it sends a JOINREP to P2 over the EmulNet. After this P1 sends hearbeats to other processes in the group. When P2 receives the message from P1 two cases arise for the membership list of P2. First is that the process P3 is not in the list of P2 so it will add it with it's hearbeat. Second is that the process P3 is already present in the group and so it updates the heartbeat of this process. This updated info of the list is then gossiped to other processes. If the message type received was JOINREP or PING it simply sends heartbeats to other processes.  

If the heartbeat of the process in the membership list is not updated for a specific time period then the process is said to timeout and its entry is removed from the membership list and this information is gossiped to rest of the processes.

## How to run grader?

There is a grader script Grader.sh. It tests your implementation of membership protocol in 3 scenarios and
grades each of them on 3 separate metrics. The scenarios are as follows:  
1. Single node failure  
2. Multiple node failure  
3. Single node failure under a lossy network.  

Navigate in the directory of the project and then perform:

```
	$ make
	$ chmod +x Grader.sh
	$ ./Grader.sh
```
