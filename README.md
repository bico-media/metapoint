Status: _idea_ → __lose draft__ → _draft_ → _proposal_ → _final review_ → _stable_

# Metapoint

> Separating the Metanet graph from content already on the blockchain.

This document describes the protocol "Metapoint". Please share [inputs and comments](https://github.com/bico-media/metapoint/issues/new).

## Introduction 

The Metanet is a concept described by nChain in [this document](https://drive.google.com/viewerng/viewer?url=https://nchain.com/app/uploads/2019/06/The-Metanet-Technical-Summary-v1.0.pdf) where a directed acyclic graph is constructed from BitCoin transactions. In short, a transaction on the blockchain can identify as a node in a graph if it is signed by its parent while it still has the ability to include any arbitraty data or protocols (as known from other OP_RETURN transactions).

Metapoint is a protocol that lets a Metanet transactions indicate where the actual content is located on the blockchain. This means that data don't need to be readded to the blockchain in case the graph is restructured for a new topology or if we want to include data added by others. 


## Description

The Metapoint protocol consist of 3 parts: namespace, type and a target value. 

```
OP_FALSE
OP_RETURN
  meta
  [Public Key for node]
  [Tx ID of parent node]  
  |
  1NnEuYAPJoeB1oa5vfEaoKYEYUPgsuLLEp
  [type]
  [value]
```

- Please use `|` (pipe) to separate the Metanet information and the rest of the protocols

- The bitcom namespace for Metapoint is `1NnEuYAPJoeB1oa5vfEaoKYEYUPgsuLLEp`

- `type` must have one of the following values:
  - the string `tx` to indicate that `value` contains the transaction ID of a transaction. 
  - the string `c` indicating that `value` contains the sha256 hash of the targeted content. 
  - the string `b` indicating that `value` contains the transaction ID of a b:// formatted transaction.
  - the string `bcat` indicating that `value` contains the transaction ID of a B://cat formatted transaction.
  - The string `txt` indicating that `value` contains the actual content. 

- `value` must be a utf8 encoded string no longer than 512 bytes. 



### Example

```
OP_FALSE
OP_RETURN
  meta
  [Public Key for node]
  [Tx ID of parent node]  
  |
  1NnEuYAPJoeB1oa5vfEaoKYEYUPgsuLLEp
  bcat
  3bc9205b43bf8ca42856d798c2e959c18ffaaa5ff3212b11e9a07dd6cf303aa9
```

## Notes

### Metanet transactions

As any transaction can be referenced as the target, it is up to the implementation to hold information about how the targeted transaction should be processed/interpreted. It is, however, strongly suggested that implementations using Metapoint disregard any Metanet information in a targeted transaction to ensure relations between nodes are kept on the Metanet 

###  c, b and bcat 
One could argue that `tx` is all that is needed to reference another transaction. However, `c`, `b` and `bcat` are the most used protocols for storing more data-heavy transactions on the blockchain at the moment. As the process of obtaining the content differs, it is helpful information to have already when the node has been loaded. 

### txt
The key `txt` Is meant to be used when an implementation normally relies on Metapoint, but short strings like settings or names make the overhead of another transaction too big compared to the data carried. 




----

Please share [inputs and comments](https://github.com/bico-media/metapoint/issues/new).



