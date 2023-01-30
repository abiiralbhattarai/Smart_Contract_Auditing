# Solidity 101 from secureum

1. The layout of a Solidity source file can contain an arbitrary number of pragma directives, import directives and struct/enum/contract definitions. The best-practices for layout within a contract is the following order: state variables, events, modifiers, constructor and functions.

2. SPDX License Identifier: SPDX stands for Software Package Data Exchange

3. “pragma solidity x.y.z;” where x.y.z indicates the version of the compiler. A different y in x.y.z indicates breaking changes e.g. 0.6.0 introduces breaking changes over 0.5.z. A different z in x.y.z indicates bug fixes.

4. NatSpec Comments: NatSpec stands for “Ethereum Natural Language Specification Format.” These are written with a triple slash (///) or a double asterisk block(/\*_ ... _/) directly above function declarations or statements to generate documentation in JSON format for developers and end-users. It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). These comments contain different types of tags:

- @title: A title that should describe the contract/interface

- @author: The name of the author (for contract, interface)

- @notice: Explain to an end user what this does (for contract, interface, function, public state variable, event)

- @dev: Explain to a developer any extra details (for contract, interface, function, state variable, event)

- @param: Documents a parameter (just like in doxygen) and must be followed by parameter name (for function, event)

- @return: Documents the return variables of a contract’s function (function, public state variable)

- @inheritdoc: Copies all missing tags from the base function and must be followed by the contract name (for function, public state variable)

- @custom…: Custom tag, semantics is application-defined (for everywhere)

5. State Variables: Constant & Immutable

   - State variables can be declared as constant or immutable. In both cases, the variables cannot be modified after the contract has been constructed. For constant variables, the value has to be fixed at compile-time, while for immutable, it can still be assigned at construction time i.e. in the constructor or point of declaration.

   - For constant variables, the value has to be a constant at compile time and it has to be assigned where the variable is declared. Any expression that accesses storage, blockchain data (e.g. block.timestamp, address(this).balance or block.number) or execution data (msg.value or gasleft()) or makes calls to external contracts is disallowed.

   - Immutable variables can be assigned an arbitrary value in the constructor of the contract or at the point of their declaration. They cannot be read during construction time and can only be assigned once.

   - The compiler does not reserve a storage slot for these variables, and every occurrence is replaced by the respective value.

6. Compared to regular state variables, the gas costs of constant and immutable variables are much lower

7. An external function f cannot be called internally (i.e. f() does not work, but this.f() works)

8. Function Overloading: A contract can have multiple functions of the same name but with different parameter types. This process is called “overloading.”

9. Events: They are an abstraction on top of the EVM’s logging functionality. Emitting events cause the arguments to be stored in the transaction’s log - a special data structure in the blockchain. These logs are associated with the address of the contract, are incorporated into the blockchain, and stay there as long as a block is accessible.

10. Variables and other items declared outside of a code block, for example functions, contracts, user-defined types, etc., are visible even before they were declared. This means you can use state variables before they are declared and call functions recursively.

11. 