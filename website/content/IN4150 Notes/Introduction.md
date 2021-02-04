+++
date = "2021-01-10"
description = "IN4150 Distributed Algorithms Notes"
linktitle = ""
publisdate = "2021-01-10"
title = "Introduction"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

#### What is a Distributed System?

Distributed computer systems are collections of computer systems that present themselves as single entities to their
users and are characterized by

- **Autonomy:** The components of the DS have a certain power or authority to make their own decisions.
- **Cooperation:** The components iof a DS are working together towards common goals.
- **Communication:** The components of the DS exchange information.

#### Properties of Distributed Systems

1. There is no regular structure such that a DS may be connected by heterogenous network technologies and consist of
many different processors. (They still use the same communication protocols)
2. There is no directly accessible common state such as shared variables in a shared memory.
3. There is no common clock. _Synchronous_ systems maintain a notion of a common clock through message exchanges where there is a bound on transmission times, whereas in
_asynchronous_ systems there are no assumptions about message transfer times.
4. There is nondeterminism such that different components of the system kae progress independently.
5. There are independent failure nodes.

#### Requirements of Distributed Systems

1. **Transparency:** DSs have to present themselves to their users and the applications running on them as single entities
without revealing that they are in fact built from many components.
2. **Scalability:** DSs should be extensible without bottlenecks and the increase of capacity should be proportional to the extension.
3. **Consistency:** DSs should be consistent in terms of their performance, user interface, and the global view.
4. **Modularity and Openness:** In DSs, there is a strong emphasis on the design and standardization of interfaces, which is in particular
helpful when systems are heterogenous.

There are two main algorithms running in distributed systems, algorithms that compute something (e.g. linear algebra computations) and
control algorithms that execute some function to ensure the proper operation of some aspect of a DS.
