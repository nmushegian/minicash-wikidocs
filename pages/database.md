- The design of the database layer of minicash is perhaps the most important part of creating a performant minicash client.
- Minicash acknowledges the reality of forking head-on and relaxes the POW rules to make them simpler to specify but leading to continuous forking instead of treating it like an exception.
-
- Minicash has two different abstractions for dealing with the tree of blocks and system state.
	- Here are the set of records that define the minicash system state.
	- ```
	  // Rock:                                                                                                                                                                                     
	  //    ['tick', tickhash]         -> tick                                                                                                                                                     
	  //    ['tack', tockhash, i]      -> tack                                                                                                                                                     
	  //    ['tock', tockhash]         -> tock                                                                                                                                                     
	  //    ['work', tockhash]         -> work  // cumulative work                                                                                                                                 
	  //    ['fold', tockhash, i]      -> fold  // [snap, fees]  partial utxo / fees                                                                                                               
	  //    ['know', tockhash]         -> know  // validity state                                                                                                                                  
	  //    ['best']                   -> tock                                                                                                                                                     
	  
	  // Tree:                                                                                                                                                                                     
	  //    [(snap) 'ment', mark]      -> ment  // utxo put [code, cash]                                                                                                                           
	  //    [(snap) 'pent', mark]      -> pent  // utxo use [tish, tosh] (by tick, in tock)                                                                                                        
	  //    [(snap) 'pyre', mark]      -> time  // utxo expires  
	  ```
- Note that the special mint tick rules mean that `ment/pent` also keeps a *per-branch* set of tocks in history. This is so that you can quickly answer "is X and ancestor of Y" *for a particular branch*.
-
- The first is called Rock.
	- Rock is a direct key-value store.
	- Rock is insert-only, no value can be changed once inserted (except for 'best', which only changes when there is a definitely-valid tock with higher total work). All state is implemented with constant-time access of some small number of values (e.g., "exists" and "spent" status for utxo).
	- A database transaction over a rock is called a `rite`. We say "database transaction" to emphasize that this is a distinct concept from a minicash transaction, which is called a `tick`. Processing a tick happens at a different layer of abstraction.
- The second is called Tree.
	- Tree is a key-value store with an immutable map API. You could call this a "pure map" or an "immutable map", although in a precise definition you should try to avoid these terms and emphasize that it is an *API* that navigates an abstraction in the underlying rock.
	- A database transaction over a tree is called a `twig`. It is distinct from `rite`, the transaction on the underlying rock, and very different from `tick`, which is a minicash transaction (which exists at a higher level of abstraction).
	- A `twig` is *implemented* using `rite`, because Tree is an abstraction on top of Rock.
	- For efficiency, you can access the underlying `rite` from inside a `twig`, so that you don't need to open separate transactions if you want to insert things into the direct map and also into the pure map. Your language bindings should help you ensure that these remain logically isolated.
	- A tree is implemented using a data structure called an *adaptive radix trie*.
		- It is a prefix tree
			- It has variable "depth" nodes, like a patricia trie
			- It has variable "width" nodes, this is the new feature of the "adaptive" radix tree introduced by the authors of the [adaptive radix tree paper](https://db.in.tum.de/~leis/papers/ART.pdf)
		- The"depth" part is a time optimization, and is essential for performance.
		- The "width" part is a space optimization, it has secondary impact on performance via the memory/storage hierarchy, you can achieve the same by adding more and faster memory/storage
-
-