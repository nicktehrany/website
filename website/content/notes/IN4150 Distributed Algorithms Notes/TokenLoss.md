+++
date = "2021-01-19"
description = "IN4150 Distributed Algorithms Notes"
linktitle = ""
publisdate = "2021-01-19"
title = "Token Loss"
type = "notes"
mathjax = "true"
+++

Since token based systems require the presence of a single token, if a token is lost due to unreliable links, this has to be detected and a single new token has to be generated n a unidirectional ring.

It works by using two tokens $t_0$ and $t_1$ that detect the loss of each other. The _token_ message that transfers the token contains a token number that is the index of the token and a counter which is equal to plus or minus the number of times the tokens have met, plus if the token id is 0 and minus if it is 1. Every node records the counter of the last token it sent in $l$. The token $t_1$ is lost when at some point $t_0$ arrives at a node and the condition $l=c$ is met. Then $t_1$ is regenerated.

This works by the logic that if in a unidirectional ring a process encounters the same token twice without the tokens having met each other on the way, it is lost (doing a cycle in the ring without encountering the token is not possible unless it is lost).
