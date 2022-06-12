- Signature checks on a minicash transaction are defined below.
- Most importantly, note that checksig is called from `vinx_tick`, the valid-in-context check for transactions, which is *not* part of the critical path. You can read the context from the database and check signatures without ever interrupting the validator.
-
- ```
  // `bill` is the output being spent
  // `tick` is the transaction spending it
  checksig(bill :Bill, tick :Tick, i :number) :boolean {
      // ...
  }
  
  // these arguments come from `vinx_tick`
  // `conx` is the context, with ticks containing bills being spent
  // `tick` is the transaction being validated
  // returns *net fees* (can be negative) if valid-in-context
  vinx_tick(conx: Tick[], tick :Tick) :Okay<Fees> {
  
  }
  ```