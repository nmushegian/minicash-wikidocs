- A minicash transaction is called a tick. Ticks are serialized using RLP.
- A tick is a list containing 2 items:
	- A list of inputs, called `move`s. A tick can have at most 7 moves.
	- A list of outputs, called `ment`s. A tick can have at most 7 ments.
- ````
  type tick = [move[], ment[]]
  type move = [txin, idx, sig]
  type ment = [code, cash]
  ```
- A `move` is a list with 3 items: `[txin, idx, sig]`
	- A `txin` is a tickhash. A tickhash is a 24-byte hash of a serialized tick (the last 24 bytes of its `keccak256` hash).
	- A `idx` is a single byte containing a value from 0-7. Note that a tick can only have 7 outputs, which correspond to `idx` value 0-6. When idx is 7, this is the special "mint tick", described below.
	- A `sig` is a `secp256k1` signature, which is 65 bytes. See [[vinx]] (`vinx_tick`) for details about the signature check.
- A `ment` is a list with 2 items: `[code, cash]`
	- `code` is a 20-byte hash of a public key (the last 20 bytes of the `keccak256` of the public key).
	- `cash` is a 7-byte value representing amount of cash.
-
- It is important to emphasize that cash is stored in a "UTXO point", which is a `txin` and `idx`. We define a `mark` as a concatenated `txid ++ idx`, which is 25 bytes. This is the actual object that holds cash. Cash is *not* stored in an "address" (`code`). The same code could be used in multiple places, but you spend UTXOs.
-
-
-
-
-
- A mint tick is the tick that contains the miner reward. It has a special form, and puts all the special case logic in one place.
	- A mint tick must have only 1 move and 1 ment.
	- A mint tick must be the *last* tick in a tock.
	- The move's `txin` refers to the previous tock hash, not to a tick. If a client implementation uses a purely functional data structure for the UTXO set, then this move functions as a fast *per-branch* ancestor check.
	- The move's `idx` is 7. This emphasizes that it is not spending a regular tick, and also emphasizes the final nature of the protocol -- it would require very roundabout logic to allow more than 7 ticks per tock, because it would conflict with this special case.
	- The move's `sig` does not need to be a valid signature, because it is not spending a real tick. However it does still need to be the same size as a regular signature (65 bytes).
	-