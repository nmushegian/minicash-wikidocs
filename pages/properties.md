public:: true

- The following hard-coded constants and functions define minicash at a high level. With these, different developers could come up with close to identical systems. The purpose of the specification is to define minicash down to the exact byte layout.
	-
- There are two basic forms that define the minicash system:
	- a [[tick]] is a minicash transaction
	- a [[tock]] is a minicash block header
- The minicash spec also defines a [[tack]], which is a convention for chunking transactions
- These three structures are used in the core message types used to define full node equivalence. All clients must implement these messages, but they can also invent more efficient ones
	-
-
	- There are at most `7` inputs and at most `7` outputs per [[tick]].
	- There are at most `2^17 == 131072` ticks per [[tock]].
	- The time between tocks is exactly `57` seconds.
	- `roll` is defined as `rlp.encode`
	- `hash` is defined as `keccak256`.
	- `scry` is defined as `secp256k1.ecrecover` (compressed pubkey)
	- The total supply is `2^53`. Note that this is one larger than the safe f64 range (max 2^53 - 1). This is intentional -- it is to remind the implementor that at tock-level validation, you *must* use bignums, because a tock can have total outputs greater than the total supply.