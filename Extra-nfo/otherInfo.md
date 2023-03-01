# Extra Infromation

1.  When a smart contract is deployed on a network, its bytecode gets associated with its address and stored on the blockchain. To be more precise, the bytecode of a smart contract is stored in the world state, under the “code” field of the smart contract address.

2.  # Bits and Bytes

- At the smallest scale in the computer, information is stored as bits and bytes.

---

- Bit
  1. a "bit" is atomic: the smallest unit of storage
  2. A bit stores just a 0 or 1
  3. Group 8 bits together to make 1 byte
  4. Mathematically: n bits yields 2n patterns
  5. 8 bits -> 256 -> one byte

---

---

- Bytes
  1. One byte = collection of 8 bits
  2. e.g. 0 1 0 1 1 0 1 0
  3. One byte can store one character, e.g. 'A' or 'x' or '$'
  4. All storage is measured in bytes, despite being very different hardware
  - Kilobyte, KB, about 1 thousand bytes
  - Megabyte, MB, about 1 million bytes
  - Gigabyte, GB, about 1 billion bytes
  - Terabyte, TB, about 1 trillion bytes

---

- One byte is the range 00000000 - 11111111 in binary, or 0x00 - 0xFF in hex. As we can see, one byte is represented in hex as a 2 character string. Therefore, a 32 byte hex string is 64 characters long.

- Integers are typically stored with either 4 or 8 bytes
  4 bytes can store numbers between -2147483648 and 2147483647
  8 bytes can store numbers between -9223372036854775808 and 9223372036854775807

  - Adding in binary is just like normal addition with carrying
    But when you run out of bits you can't carry anymore
    Leftmost bit indicates sign, so carrying to the leftmost bit changes a number from positive to negative.
    So adding 1 to 2147483647 goes to -2147483648!
    Called Integer Overflow

3. Function Signatures:
   The function signature contains the function name and parameter type.

```
function func() public {
}
function func2(address[] addr, uint256 amount) public {
}
```

- The signatures of the above two functions are:

  func()
  func2(address[],uint256)
