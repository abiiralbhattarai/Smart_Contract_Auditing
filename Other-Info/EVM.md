# EVM Informations

1. ### EVM architecture image

   ![EVM architecture](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*kC4pmvHMtXdmymOX.png "evm architecture")

2. ### The EVM disposes of five main data locations:

   - Storage
   - Memory
   - Calldata
   - Stack
   - Code

3. ### Overview of the data locations available in the EVM

   ![Data locations in EVM](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*ArLLYlT2O3haZtvvyx4Rcw.png)

4. ### Memory

   - The EVM memory is used to hold temporary values and is erased between external function calls.

   - In the EVM, the memory is volatile and is specific to the context of a specific contract (environment). Meaning the whiteboard/scratchpad is cleared when the execution context change from one contract to another. A freshly cleared instance of memory is obtained on each new message call.

   - You can read and write from/to the EVM memory. At the low level, the EVM opcodes that can be used to read and write from/to the memory are MLOAD, MSTORE, and MSTORE8.

   - Certain EVM opcodes such as CALL, DELEGATECALL or STATICCALL consume their arguments from the EVM memory.

5. ### Calldata

   - calldata is equivalent to a container taken out of a ship or truck. These containers contain materials delivered to the factory for processing. Calldata is read-only.

   - The calldata is where the data of a transaction or the parameters of an external function call reside. It is a read-only data location. You cannot write to it.

   - The calldata behaves mostly like memory and is a byte-addressable space. You have to specify an exact byte offset for the number of bytes you want to read.

6. ### Stack

   - The stack is used to hold small local variables. It is almost free to use (use a very low amount of gas), but is limited in size and can hold a limited number of items.

   - The stack is where most of the local variables created inside a function reside. It is an essential part of the EVM.

7. ### Code

   - The code refers to the contract bytecode. You can only read from the contract bytecode, not write to it.

   - The bytecode contains a lot of information and logic about the contract, including the dispatcher, and the contract metadata.

   - Data location

     ![Data Location](https://miro.medium.com/v2/resize:fit:688/format:webp/1*oeSfgoTY45GbyFfhur3PBw.png "Data Location")


### EVM works with a word size of 256 bits mainly to facilitate native hashing and elliptic curve operations

### Only the top 16 values are available for access.
