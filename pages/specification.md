- Minicash is defined by specification, and has multiple implementations of [[full node]] clients.
-
- The specification is oriented around the core terms:
- ```
  tick = [moves bills]
    move = [txin indx sign]
    bill = [hash cash]
  tock = [prev root time fuzz]
  ```
-
- The protocol definition also includes the core message types which all full node implementations must implement (they can also implement more efficient ones)
- ```
  <- [ask/tocks from]
  -> [res/tocks tocks[]]
  
  <- [ask/tacks tockhash[]]
  -> [res/tacks tocks[] necks[][] feet[][]]
  
  <- [ask/ticks tickhash[]]
  -> [res/ticks ticks[]]
  ```
-
- These core message types use a simple canonical chunking and merkle proof
-
- modules
	- [[djin]] engine
	- [[dmon]] daemon
	- [[well]] well-formed checks
	- [[word]] basic definitions / ontology
	-
	-
	-