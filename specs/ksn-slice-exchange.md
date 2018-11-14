# KSN Slice Exchange Protocol
> This document describes the high level `Slice exchange protocol` built as a set of global PubSub topics that KSN (kitsunet) clients subscribe/publish slices on.

## Overview

The kitsunet network is built on top of a distributed PubSub primitive that supports topic based publish/subscribe. The underlying PubSub mechanism itself is beyond the scope of this document. 

The protocol allows the kitsunet clients to collaborate in distributing `slices` over the PubSub mesh.

### Slices

The mustekala project has introduced the concept of slices, which are **merkel subtries** and are based around the concept of smallest unique prefix, similar to the one described in the Kademlia DHT paper. This construct allows us to partition the merkel patricia trie that Ethereum clients keep by processing blocks.

Slice have two properties that allow us to address groups of slices independent of the latest block, as well as address a particular slice tied to some block (or rather `stateRoot` or `storageRoot`). 

#### Slice group

A slice group is an identifier that points to a group of slices regardless of the current block. For example - `0000-10`, points to the slice with offset `0000` and depth of `10`. This allows us to track any slice in this group and tie updates to this slice to some specific block, in other words we can update the local slice when we learn of a new block.

#### Specific slice

A specific slice, is an identifier that uniquely points to a particular slice tied to a block (or rather `stateRoot` or `storageRoot`). For example, `0000-10-abcdef012345....` will point to a slice attached to the block with a state root of `abcdef012345....`. This allows us to request this particular slice and none other.

### The KSN protocol

The KSN slice exchange protocol uses these properties to track slices over time or request specific slices on demand. Bellow is an outline of the different channels/topics available to track and request slices on demand from the network.

### API

#### Track slices

`kitsunet:slice:<path>-<depth>` subscribe to a slice group

This enables the client to track a particular slice. It will trigger a new slice every time a block is generated.

`kitsunet:slice:<path>-<depth>-<root>` subscribe to a specific slice

This enables the client to track a slice for a specific state root, it will only get updates for that slice, this might trigger on each block update, but it will not change across block updates.

#### Request slices

In addition to the above two channel types, the KSN network supports broadcast topics to request slices on demand from the network. It is possible to request either a specific slice or a slice group, depending if the `root` was provided as part of the topic payload. A kitsunet bridge - a KSN client that is talking to a full node over RPC (or the kitsunet protocol directly), can then decide to fulfill the requested slice/slice group.

`kitsunet:slice:track` request a state slice (as opposed to a storage slice) from the network, it can contain either a slice group or a specific slice.  

`kitsunet:slice:track-storage` request a storage slice (as opposed to a state slice) from the network, it can contain either a slice group or a specific slice. 

#### Block headers

KSN also broadcasts block headers that kitsunet bridges request from full nodes over RPC.

`kitsunet:block-header` subscribe to block header updates, triggers each time a new block is available.
