+++
date = "2021-01-23"
description = "IN4150 Distributed Algorithms Notes"
linktitle = ""
publisdate = "2021-01-23"
title = "Consensus"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

Distributed systems will have to deal with faults caused by software or hardware components, not operating according to specification. Faults can be classified as _permanent_, once a processor exhibits a fault it will be considered as faulty for ever, which also leads to crash failures and malicious or Byzantine failures. _Transient_ faults are when a processor may exhibit a fault but will return to correct operation again, i.e. transmission error or short power loss.

Reaching agreement based on every process starting with a value from the set of possible initial values has the following conditions,

1. _Agreement:_ No two processes decide on different values.
2. _Validity:_ If all processes start with the same value then no process decides on a different value (The final agreement is related to the original problem).
3. _Termination:_ All non-faulty processes decide within finite time.

### Consensus in Synchronous Systems with Crash Failures

#### An Algorithm for Agreement in a Synchronous System with at most f Crash Failures

Every process maintains a set $W$ which initially only contains the value it starts with. In each round out of a total of $f+1$ they all broadcasts their sets $W$ and set their set $W$ to the union of all sets received and their own. If after the $f+1$ rounds the set $W$ only contains one element the process decides on that element, otherwise the process decides on the default value.

Since there are $f+1$ rounds and at most $f$ processor failures, there is at least one round during which no processor fails. At the end of this round all still active processes will have identical sets $W$ and they will not change anymore.

**Message complexity** with $n$ processes is $O(f*n^2)$.

### Consensus in Synchronous Systems with Byzantine Failures

Algorithms solving the Byzantine-generals problem in synchronous systems with $n$ processes, the maximal number of faulty processes $f$ has to satisfy $f< n/3$. This is interpreted as the 3 sets that exist, the malicious processes, the majority, and the minority, where there should be at least more than in the other sets. All deterministic byzantine algorithms require a minimum of $f+1$ rounds.

#### Impossibility for Three Generals

_Impossibility results_ are consensus problems where no solutions exits, which is shown by two different scenarios that are indisitinguishable to at least one process and two processes have to reach different conclusions. Among two lieutenants and one commander there is one disloyal one and in the following two scenarios to $L_1$ the they are both the same.

![Three Generals](/images/IN4150/ThreeGenerals.png)

#### Lamport-Pease-Shostak Algorithm for Consensus in Synchronous Systems without Authentication

The algorithm can tolerate a maximum of $f$ faults. As the bottom case when $f$ equals $0$ the commander sends his value to the lieutenants who simply decide on this value. When $f$ is positive, the commander sends his value to the lieutenants each of whom then executes the algorithm recursively with parameter $f-1$ as themselves as the commander and the others as the lieutenants (except the commander). Each lieutenant decides on the majority value among the value received directly from the commander and the values on which he decides as a lieutenant of the other lieutenants acting as commander. When a process does not receive a message because the sender is faulty it assumes the default value. Messages contain a value and a sequence of lieutenants through which it passed. The recursive part is taking decisions starting at level $(OM(0))$ which is the leaf level, the last value he receives, and since it is just one value he chooses it. The next level $(OM(1))$ he picks the majority of the value at the current level and all the children. This continues until he reaches the root.

![Byzantine Agreement](/images/IN4150/ByzantineAgreement.png)

**Message complexity** is $(n-1)+(n-1)(n-2)+...+(n-1)(n-2)(n-f-1)=O(n^{f+1})$, since in each round the sending to processes becomes one less as the message is not sent to the acting commander.

#### Lamport-Pease-Shostak Algorithm for Consensus in Synchronous Systems with Authentication

With authentication signatures cannot be forged and modifications of a signed message can be detected. This way a false acting commander cannot change contents of previous messages from the commander when sending the values down the tree. If signatures are modified, the receiver will use the default value.

#### Srikanth-Toueg Algorithm for Consensus with Authenticated broadcast in Synchronous Systems with a Completely Connected Network

_Note, this algorithm is not exam material._

The commander stars with some binary value $v$ that it wants to send to all other processes, which initialize their own variable $v$ to $0$. The algorithm consists of two layers, where the bottom one implements the authenticated broadcast and the upper one implements the actual algorithm. The upper layer has $f+1$ rounds. In the first round if the general is correct (non-fault) and it has $v=0$ it will never broadcast a message, and if $v=1$ it will broadcast a $1$ to all others. The other processes broadcast a $1$ only once and once they have set their own value to $1$, which happens when they received in all rounds up to and including round $r$ a $1$ from at least $r$ processes including the general.

The lower layer has the requirement that a faulty process may send a signed message to some but not all correct processes.
