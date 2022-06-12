- The [[coresync]] algorithm defines the minicash system state. Full nodes that choose more efficient sync algorithms must ensure they will lead to the same view of the system state as the naive sync algorithm.
-
- ```
  let best be the heaviest known definitely-valid chain
  let leads be set of possibly-valid chains
  every T seconds,
     send each peer [ask/tocks <init>] (ask for headers since init)
     for each [res/tocks tockhash[]]   (this is their best definitely-valid)
        save the tock chain to `leads` (keep top K>3 candidates)
  every T2 seconds, 
     for each lead,
        send request for tacks
        then send request for ticks
        then attempt to apply
        if applied, maybe update best
  ```