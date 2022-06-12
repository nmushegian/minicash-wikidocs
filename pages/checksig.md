- Signature checks on a minicash transaction are defined below.
- Most importantly, note that checksig is called from `vinx_tick`, the valid-in-context check for  transactions, which is *not* part of the critical path. You can read the context from the database and check signatures without ever interrupting the validator.
- ```
  checksig(conx :Tick[], tick :Tick, i :number) :boolean {
      // ...
  }
  ```