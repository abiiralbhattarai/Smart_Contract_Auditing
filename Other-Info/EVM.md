# EVM Informations

---

### Basics

- The EVM is the sandboxed runtime and a completely isolated environment for smart contracts in Ethereum. This means that every smart contract running inside the EVM has no access to the network, file system, or other processes running on the computer hosting the VM.

- We know, there are two kinds of accounts: contracts and external accounts.

- Every account is identified by an address, and all accounts share the same address space. The EVM handles addresses of 160-bit length. Every account consists of a balance, a nonce, bytecode, and stored data (storage).

- The code and storage of external accounts are empty, while contract accounts store their bytecode and the merkle root hash of the entire state tree. 

- External addresses have a corresponding private key, contract accounts don’t.The actions of contract accounts are controlled by the code they host in addition to regular cryptographic signing of every Ethereum transaction.

---

1.  ### EVM architecture image

    ![EVM architecture](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*kC4pmvHMtXdmymOX.png "evm architecture")

2.  ### The EVM disposes of five main data locations:

    - Storage
    - Memory
    - Calldata
    - Stack
    - Code

3.  ### Overview of the data locations available in the EVM

    ![Data locations in EVM](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*ArLLYlT2O3haZtvvyx4Rcw.png)

4.  ### Memory

    - The EVM memory is used to hold temporary values and is erased between external function calls.

    - In the EVM, the memory is volatile and is specific to the context of a specific contract (environment). Meaning the whiteboard/scratchpad is cleared when the execution context change from one contract to another. A freshly cleared instance of memory is obtained on each new message call.

    - You can read and write from/to the EVM memory. At the low level, the EVM opcodes that can be used to read and write from/to the memory are MLOAD, MSTORE, and MSTORE8.

    - Certain EVM opcodes such as CALL, DELEGATECALL or STATICCALL consume their arguments from the EVM memory.

5.  ### Calldata

    - calldata is equivalent to a container taken out of a ship or truck. These containers contain materials delivered to the factory for processing. Calldata is read-only.

    - The calldata is where the data of a transaction or the parameters of an external function call reside. It is a read-only data location. You cannot write to it.

    - The calldata behaves mostly like memory and is a byte-addressable space. You have to specify an exact byte offset for the number of bytes you want to read.

6.  ### Stack

    - The stack is used to hold small local variables. It is almost free to use (use a very low amount of gas), but is limited in size and can hold a limited number of items.

    - The stack is where most of the local variables created inside a function reside. It is an essential part of the EVM.

7.  ### Code

    - The code refers to the contract bytecode. You can only read from the contract bytecode, not write to it.

    - The bytecode contains a lot of information and logic about the contract, including the dispatcher, and the contract metadata.

    - Data location

      ![Data Location](https://miro.medium.com/v2/resize:fit:688/format:webp/1*oeSfgoTY45GbyFfhur3PBw.png "Data Location")

    - Default Locations for variables

8.  ### Default Locations for variables

    - The Solidity language defines some default rules around in which place some variable should reside by default, depending on where they are defined.

      - variables defined as constant = contract code (= bytecode)

      - state variables (declared outside of functions) = in storage by default

      - local variables (declared inside the body of a function) = in the stack

    - For arrays (fixed or dynamic sized arrays, like uint256[]), bytes , string, struct and mappings, you have to explicitly provide the data area where the value is stored. This can be storage, memory or calldata.

    9. ### When to use the keywords storage, memory, and calldata?

    - You can specify the data location of where to reference a variable in 3 places only in a function:

    A) For the parameters (= function definition)
    B) For local variables inside the function (= function body)
    C) Return values are always in memory (=function definition).

    ![storage](https://miro.medium.com/v2/resize:fit:1094/format:webp/1*25008Ad5Igy4wtOccnpxdA.png "Data Location")

    - When storage is used as a reference for a function parameter, it is a pointer to the contract storage.

    - Same for memory and calldata. Such keywords create pointers to some location in the EVM memory or the input data field (= calldata) coming in from the transaction.

    - Memory references : we can always copy any data in memory (whether it comes from the contract’s storage or the calldata).

    - For storage and calldata = we can only assign values coming from the data location specified (either directly or via a reference of the same type.

      1. storage references: can always be assigned some value from the contract storage directly (=state variables), or via another storage reference, but they cannot be assigned a memory or calldata reference

      2. calldata references: can always be assigned some value from the calldata directly (= tx/message call input), or via another calldata reference, but they cannot be assigned a storage or memory reference

    10. ### Data Location — Behaviours

        - “Data locations are not only relevant for the persistency of data, but also for the semantics of assignments.”

        - The getter with storage is slightly cheaper than the one with memory because it acts as a storage pointer.

        - A getter using memory is more expensive and costs more gas because a new variable is created.

### EVM works with a word size of 256 bits mainly to facilitate native hashing and elliptic curve operations

### Only the top 16 values are available for access.
