# EVM Informations

---

### EVM

- Ethereum can be viewed as a transaction-based state machine.
- The world state is a mapping between address and account state.
- An account is an object in the world state. It  is a mapping between address and account state.
- A transaction is a single cryptographically-signed instruction.
- EVM is big endian order (network byte order).
  (Big-endian is an order in which the "big end" (most significant value in the sequence) is stored first, at the lowest storage address.)

### Basics

- Smart contracts are just computer programs, and we can say that Ethereum contracts are smart contracts that run on the Ethereum Virtual Machine.

- The EVM is the sandboxed runtime and a completely isolated environment for smart contracts in Ethereum. This means that every smart contract running inside the EVM has no access to the network, file system, or other processes running on the computer hosting the VM.

- We know, there are two kinds of accounts: contracts and external accounts.

- Every account is identified by an address, and all accounts share the same address space. The EVM handles addresses of 160-bit length. Every account consists of a balance, a nonce, bytecode, and stored data (storage).

- The code and storage of external accounts are empty, while contract accounts store their bytecode and the merkle root hash of the entire state tree.

- External addresses have a corresponding private key, contract accounts don’t.The actions of contract accounts are controlled by the code they host in addition to regular cryptographic signing of every Ethereum transaction.

- ### Contract Creation:

  - The creation of a contract is simply a transaction in which the receiver address is empty and its data field contains the compiled bytecode of the contract to be created (this makes sense — contracts can create contracts too).

  - Things that happens when contract is created:

    1. The first thing that happens when a new contract is deployed to the Ethereum blockchain is that its account is created.
    2. The data sent in with the transaction is executed as bytecode.
       - This will initialize the state variables in storage, and determine the body of the contract being created.
       - This process is executed only once during the lifecycle of a contract.
       - The initialization code is not what is stored in the contract; it actually produces as its return value the bytecode to be stored.

---

1.  ### EVM architecture image

    ![EVM architecture](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*kC4pmvHMtXdmymOX.png "evm architecture")

2.  ### Message Calls

    - Contracts can call other contracts through message calls.
    - Every call has a sender, a recipient, a payload, a value, and an amount of gas
    - Solidity provides a native call method for the address type that works as follows:
      - " address .call.gas(gas).value(value)(data) "
      - gas is the amount of gas to be forwarded, address is the address to be called, value is the amount of Ether to be transferred in wei, and data is the payload to be sent.

3.  ### The EVM disposes of five main data locations:

    - Storage
    - Memory
    - Calldata
    - Stack
    - Code

4.  ### Overview of the data locations available in the EVM

    ![Data locations in EVM](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*ArLLYlT2O3haZtvvyx4Rcw.png)

5.  ### Memory

    - The EVM memory is used to hold temporary values and is erased between external function calls.

    - It is mainly used to store data during execution, mostly for passing arguments to internal functions.

    - In the EVM, the memory is volatile and is specific to the context of a specific contract (environment). Meaning the whiteboard/scratchpad is cleared when the execution context change from one contract to another. A freshly cleared instance of memory is obtained on each new message call.

    - You can read and write from/to the EVM memory. At the low level, the EVM opcodes that can be used to read and write from/to the memory are:

          1. MLOAD loads a word from memory into the stack.
          2. MSTORE saves a word to memory.
          3. MSTORE8 saves a byte to memory.

    - Solidity always stores a free memory pointer at position 0x40, i.e. a reference to the first unused word in memory.

    - If you pay attention to the bytecode output by the Solidity compiler, you will notice that all of them start with 0x6060604052…, which means:

    ***

        PUSH1   :  EVM opcode is 0x60
        0x60    :  The free memory pointer
        PUSH1   :  EVM opcode is 0x60
        0x40 : Memory position for the free memory pointer
        MSTORE : EVM opcode is 0x52

    ***

    - Additionally to the cost of the write itself, there is a cost to this expansion, which increases linearly for the first 724 bytes and quadratically after that.

    - Certain EVM opcodes such as CALL, DELEGATECALL or STATICCALL consume their arguments from the EVM memory.

    - You must be very careful when operating with memory at assembly level. Otherwise, you could overwrite a reserved space.

6.  ### Calldata

    - The calldata is a read-only byte-addressable space where the data parameter of a transaction or call is held.

    - calldata is equivalent to a container taken out of a ship or truck. These containers contain materials delivered to the factory for processing. Calldata is read-only.

    - The calldata is where the data of a transaction or the parameters of an external function call reside. It is a read-only data location. You cannot write to it.

    - Unlike the stack,To use this data you have to specify an exact byte offset and number of bytes you want to read.

    - The opcodes provided by the EVM to operate with the calldata include:

      1. CALLDATASIZE tells the size of the transaction data.

      2. CALLDATALOAD loads 32 bytes of the transaction data onto the stack.

      3. CALLDATACOPY copies a number of bytes of the transaction data to memory

    - Solidity also provides an inline assembly version of these opcodes. These are calldatasize, calldataload and calldatacopy respectively.

    - The calldatacopy expects three arguments (t, f, s): it will copy s bytes of calldata at position f into memory at position t. In addition, Solidity lets you access to the calldata through msg.data.

7.  ### Stack

    - The EVM is a stack machine, meaning that it doesn’t operate on registers but on a virtual stack.
    - The stack has a maximum size of 1024. Stack items have a size of 256 bits; in fact, the EVM is a 256-bit word machine (this facilitates Keccak256 hash scheme and elliptic-curve computations).
    - The stack is used to hold small local variables. It is almost free to use (use a very low amount of gas), but is limited in size and can hold a limited number of items.
    - The stack is where most of the local variables created inside a function reside. It is an essential part of the EVM.

    ***

    1. POP removes item from the stack.
    2. PUSH*n* places the following n bytes item in the stack, with n from 1 to 32.
    3. DUP*n* duplicates the n_th stack item, with \_n from 1 to 32.
    4. SWAP*n* exchanges the 1st and n_th stack item, with \_n from 1 to 32.

    ***

8.  ### Storage

    - Storage is a persistent read-write word-addressable space
    - This is where each contract stores its persistent information
    - Unlike memory, storage is a persistent area and can only be addressed by words.
    - It is a key-value mapping of 2²⁵⁶ slots of 32 bytes each.
    - A contract can neither read nor write to any storage apart from its own
    - The amount of gas required to save data into storage is one of the highest among operations of the EVM.
    - Modifying a storage slot from a zero value to a non-zero one costs 20,000. While storing the same non-zero value or setting a non-zero value to zero costs 5,000. However, in the last scenario when a non-zero value is set to zero, a refund of 15,000 will be given.
    - The EVM provides two opcodes to operate the storage:
      - SLOAD loads a word from storage into the stack.
      - SSTORE saves a word to storage.

    ***

    - Layout of State Variables in Storage
      - Statically-sized variables (everything except mapping and dynamically-sized array types) are laid out contiguously in storage starting from position 0.
      - Multiple items that need less than 32 bytes are packed into a single storage slot if possible
      - Structs and array data always start a new slot and occupy whole slots (but items inside a struct or array are packed tightly according to these rules).
      - Due to their unpredictable size, mapping and dynamically-sized array types use a Keccak-256 hash computation to find the starting position of the value or the array data. These starting positions are always full stack slots.

    ***

9.  ### Code

    - The code refers to the contract bytecode. You can only read from the contract bytecode, not write to it.

    - The bytecode contains a lot of information and logic about the contract, including the dispatcher, and the contract metadata.

    - Data location

      ![Data Location](https://miro.medium.com/v2/resize:fit:688/format:webp/1*oeSfgoTY45GbyFfhur3PBw.png "Data Location")

    - Default Locations for variables

10. ### Default Locations for variables

    - The Solidity language defines some default rules around in which place some variable should reside by default, depending on where they are defined.

      - variables defined as constant = contract code (= bytecode)

      - state variables (declared outside of functions) = in storage by default

      - local variables (declared inside the body of a function) = in the stack

    - For arrays (fixed or dynamic sized arrays, like uint256[]), bytes , string, struct and mappings, you have to explicitly provide the data area where the value is stored. This can be storage, memory or calldata.

11. ### When to use the keywords storage, memory, and calldata?

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

12. ### Data Location — Behaviours

        - “Data locations are not only relevant for the persistency of data, but also for the semantics of assignments.”

        - The getter with storage is slightly cheaper than the one with memory because it acts as a storage pointer.

        - A getter using memory is more expensive and costs more gas because a new variable is created.

13. ###

### Only the top 16 values are available for access.

---
