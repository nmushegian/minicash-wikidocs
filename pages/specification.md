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
  ```
-
- modules
	- [[djin]] engine
	- [[dmon]] daemon
	- [[well]] well-formed checks
	- [[word]] basic definitions / ontology
	-
	-
	-