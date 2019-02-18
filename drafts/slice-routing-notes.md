High level interactions in KSN

- Track slices
  - subscribe to slice with prefix xxxx
- Find nodes that are serving slices we're interested in
  - routing mechanism to locate nodes (kademlia? perhaps a gossip based peer routing mechanism?)
- Request new slices from the network (slices that we weren't aware of previously)
  - Edge nodes (bridges) are the ones requesting slice from full nodes (in an ideal world, 
    full nodes would be running the KSN or similar protocol and we could just use those directly), 
    we need to let them know that we need to track a slice that wasn't previously being tracked.
    We might not be directly connected to edge nodes, hence we need a broadcast mechanism 
    to let them know that a slice needs to be tracked. This has several considerations...
      - Do we connect to the edge node that is now tracking our slice directly?
        - ideally, we first connect to some intermediary node, and only connect to edge nodes
        if we could not locate any intermediary node, in which case, we become an intermediary node for
        some other node in the future.

========================================================================

Requesting slices from network

1) On each new node that we connect to, we need to communicate which slices
we want to share

2) when a new slice (one that was not previosly tracked) is detected (usually storage), 
  we need to communicate to the rest of the network, that we are now interested in tracking it
  - if the network knows about it, we need to locate and connect to those nodes
  - if the network doesn't know about it, we need to edge nodes to start tracking it and the destination node
    to reseive the slice

    Whats the best way of acomplishing that?


The way I currently see kitsunet working is as follows:

1) For the state trie, we've got a deterministic address space, and most of it is going to be relativelly well balanced,
so we can safelly asume, that 4 nibble prefixes are going to be contain at least a few accounts.
  - since we know what the address space is, we can serve state slices up front, where each prefix is going to be

2) For the storage trie, the storie is a bit different, as we don't know a priory which prefix contains data, and which doesn't
so, we can't split it up, unless we know the strucure of the trie, and even then, this is going to be tricky, since the structure
changes randomly, from block to block.



