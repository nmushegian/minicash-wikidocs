- Minicash is defined by specification, and has multiple implementations of [[full node]] clients.
-
- The specification is oriented around the core terms:
- ```
  type Tick = [ // tick is a list of 2 items
    [ // inputs
      [         // input has 3 items:
        Blob24  //   txin
        Blob1   //   indx
        Blob32  //   sign
      ]
    ]<max7>  // max 7 inputs
    [ // outputs
      [         // output has 2 items:
        Blob20  //   hash
        Blob7   //   cash
      ]
    ]<max7>   // max 7 outputs
  ]
  
  type Tock = [
    Blob20 // prev
    Blob20 // root
    Blob7  // time
    Blob7  // fuzz
  ]
  
  
  ```