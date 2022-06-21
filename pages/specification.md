- Minicash is defined by specification, and should have multiple competitive implementations of [[full node]] clients.
-
- It is important to emphasize which parts of the protocol definition or any particular codebase are the actual protocol, an implementation detail, or a wire format.
	- The core protocol is defined in terms of ticks, tocks, "valid block" definition, and "best chain" definition. This is 2 data structures and 2 functions that define their relation and a way to determine a "best chain".
	- All aspects of a particular client that deal with organizing and processing these core structures are implementation details.
	- There is one particularly important implementation detail which is the [[wire format]]. We call this a "format" even though it implicitly includes live network protocols like IP or even higher levels of the network stack. The message types in the wire format are defined as rigorously as the rest of the system to give client implementations a way to synchronize that is known to lead to the correct state.
		- Clients could choose more efficient ways to communicate. For example, some information in the memos is simply wasteful, for example, the memo tags could be just 1 byte or even implied from the structure. Other memos contain info that is redundant, it could simply be omitted and reconstructed.
		- When clients add support for new wire formats (including, by our definition, new transport types), *they should still support the original core message types*. This is a way to ensure their clients still synchronize to the correct system state.
-
-
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
  
  <- [ask/tacks tockhash indx]
  -> [res/tacks tockhash indx merkhash[] tickhash[][]]
  
  <- [ask/ticks tickhash[]]
  -> [res/ticks ticks[]]
  
  -> [ann/ticks ticks[]]
  ```
-
- These core message types use a simple canonical chunking and merkle proof strategy to guide protocol implementers towards correctly dealing with concurrent candidates so that all [[full node]] implementations can keep up with the network as it scales up. A subset of the block merkle structure is defined as a [[tack]]
- ```
  tack = [head indx neck feet]
    head = tockhash
    indx = number       // requested tack (index by tick num, multiple of 1024)
    neck = merkhash[]   // merkle nodes at depth 7, if >1024 ticks
    feet = tickhash[][] // chunks of 1024 ticks (except last chunk)
  
  
  ```
-
- Note that this canonical chunking method depends directly on the hard-coded constants that define the minicash spec -- the maximum possible ticks per tock is 2^17, which can be chunked into 2^7 groups of 2^10 ticks each.
-
- These core terms delivered using these core mail types are used to implement the [[coresync]] algorithm.
-
-