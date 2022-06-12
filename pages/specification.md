- Minicash is defined by specification, and has multiple implementations of [[full node]] clients.
-
- The specification is oriented around the core terms, [[tick]] and [[tock]]
- ```
  tick = [moves bills]
    move = [txin indx sign]
    bill = [hash cash]
  tock = [prev root time fuzz]
  ```
-
- The protocol definition also includes the core [[mail types]] which all full node implementations must implement (they can also implement more efficient ones)
- ```
  <- [ask/tocks from]
  -> [res/tocks tocks[]]
  
  <- [ask/tacks tockhash[]]
  -> [res/tacks tockhash[] merkhash[][] tickhash[][]]
  
  <- [ask/ticks tickhash[]]
  -> [res/ticks ticks[]]
  
  -> [ann/ticks ticks[]]
  ```
-
- These core message types use a simple canonical chunking and merkle proof strategy to guide protocol implementers towards correctly dealing with concurrent candidates so that all [[full node]] implementations can keep up with the network as it scales up. A subset of the block merkle structure is defined as a [[tack]]
- ```
  tack = [head neck feet]
    head = tockhash
    neck = merkhash[]   // merkle nodes at depth 7, if >1024 ticks
    feet = tickhash[][] // chunks of 1024 ticks (except last chunk)
  ```
-
- Note that this canonical chunking method depends directly on the hard-coded constants that define the minicash spec -- the maximum possible ticks per tock is 2^17, which can be chunked into 2^7 groups of 2^10 ticks each.
-
- These core terms delivered using these core mail types are used to implement the [[coresync]] algorithm.
-
-
-
-
- modules
	- [[djin]] engine
	- [[dmon]] daemon
	- [[well]] well-formed checks
	- [[word]] basic definitions / ontology
	-
	-
	-