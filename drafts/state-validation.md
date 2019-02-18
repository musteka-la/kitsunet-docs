# Verifying state in kitsunet

## Normal flow

Conditions:

> we're connected to good nodes and are receiving correct block headers (happy path)

Steps:

- start receiving block headers from pubsub, we're not getting any conflicting blocks
- try getting a slice for the stateRoot in the header
  - this alone gives us confidence that the block is correct

## Partial eclipse attack

Conditions:

> we're connected to _some_ malicious/stale nodes that are broadcasting bad block headers

Steps:

- start receiving blocks 
  - we got a conflicting block (block with same number)
    - verify and both blocks, if one of them fails the seal verification, discard the block
    - if both blocks pass, verify PoW and pick the block header with the highest difficulty
- get the slice for the good block
  - if slice is available, then we're looking at the correct chain
  - if slice is not available
    - wait for next block?
    - re-download the CHT?
    - ???

## Full eclipse attack
(this is why we need the CHT)

Conditions:

> we're only connected to malicious nodes broadcasting bad block headers



