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


