- A minicash transaction is called a tick. Ticks are serialized using RLP.
- A tick is a list containing 2 items:
	- A list of inputs, called `move`s. A tick can have at most 7 moves.
	- A list of outputs, called `ment`s. A tick can have at most 7 ments.
- ````
  tick = [moves, ments]
  ```
- A `move` is a list with 3 items: `[txin, idx, sig]`
	- A `txin` is a tickhash. A tickhash is a 24-byte hash of a serialized tick (the first 24 bytes of its `keccak256` hash).
	- A `idx` is a single byte containing a value from 0-7. Note that a move can