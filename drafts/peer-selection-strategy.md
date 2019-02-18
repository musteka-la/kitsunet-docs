# KSN Peer selection strategy

> This document describes how peers are selected in the KSN network. 

## Rationale

In a p2p network, such as the KSN network, where the data being distributed is well understood and has a specific schema, such as - blocks, block headers and slices (merkle subtries), it is possible to structure peer selection strategies around the quality of that data, in addition to other metrics such as bandwidth and latency. For example, if a peer is serving malformed or out of date block headers, we should disconnect and possibly mark that peer as `bad` as soon as we find a better peer. Ultimately, peers need to have some notion of `good` and `bad`, and should be rated on some numeric scale where, the higher the score the better the peer is and vice-versa.

Ultimately, peer selection should converge with some sort of incentivization token.
