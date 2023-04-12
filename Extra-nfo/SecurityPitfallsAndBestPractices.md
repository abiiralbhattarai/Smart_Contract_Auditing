## some points from Security Pitfalls And Best Practise

1. Modifier side-effects: Modifiers should only implement checks and not make state changes and external calls which violates the checks-effects-interactions pattern. These side-effects may go unnoticed by developers/auditors because the modifier code is typically far from the function implementation.

2. Incorrect modifier: If a modifier does not execute \_ or revert, the function using that modifier will return the default value causing unexpected behavior.

3. Avoid transfer()/send() as reentrancy mitigations: Although transfer() and send() have been recommended as a security best-practice to prevent reentrancy attacks because they only forward 2300 gas, the gas repricing of opcodes may break deployed contracts. Use call() instead, without hardcoded gas limits along with checks-effects-interactions pattern or reentrancy guards for reentrancy protection.
4. Divide before multiply: Performing multiplication before division is generally better to avoid loss of precision because Solidity integer division might truncate.

```diff
contract A {
 function f(uint n) public {
- coins = (oldSupply / n) \* interest;
+ coins = (oldSupply * interest / n)
}
}

```
5. Signature malleability: The ecrecover function is susceptible to signature malleability which could lead to replay attacks. Consider using OpenZeppelin’s ECDSA library.
