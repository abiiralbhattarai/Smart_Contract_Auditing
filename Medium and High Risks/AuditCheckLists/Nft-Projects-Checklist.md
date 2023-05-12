#NFT AND NFT MARKET PLACE CONTRACTS

| Number | Issues                                                                                                                              |
| :----: | :---------------------------------------------------------------------------------------------------------------------------------- |
|   1.   | [Check whether there is address zero check or not for recepients](#check-whether-there-is-address-zero-check-or-not-for-recepients) |
|   2.   | [Buyer has to pay gass fee of other royalty recipents](#buyer-has-to-pay-gass-fee-of-other-royalty-recipents)                       |
|   3.   | [Check wheter maximum limit to mint is applied properly or not](#check-wheter-maximum-limit-to-mint-is-applied-properly-or-not)     |

---

1. ### Vulnerabilities Check list (NFT AND NFT MARKET PLACE)
   1. Re-entrancy
   2. Timestamp Dependence
   3. Gas Limit and Loops
   4. Exception Disorder
   5. Dos with Block Gas Limit
   6. Gasless Send
   7. Exception disorder
   8. Transaction-Ordering Dependence
   9. Use of tx.origin
   10. Compiler version not fixed
   11. Address hardcoded
   12. Divide before multiply
   13. Balance equality
   14. Byte Array
   15. Transfer forwards all gas
   16. Integer overflow/underflow
   17. Dangerous strict equalities
   18. Tautology or contradiction
   19. Return values of low-level calls
   20. Missing Zero Address Validation
   21. ERC20 API violation
   22. Send instead of transfer
   23. Unchecked external call
   24. Style guide violation
   25. Malicious libraries
   26. Unchecked math
   27. Implicit visibility level
   28. Unsafe type inference
   29. Compiler version not fixed
   30. Redundant fallback function
   31. Private modifier
   32. Revert/require functions
   33. Using block.timestamp
   34. Multiple Sends
   35. Using SHA3
   36. Using suicide
   37. Using throw
   38. Using inline assembly
   39. Support CEI pattern and check rentrancy in safemint()
2. ### Check whether there is address zero check or not for recepients

   - https://github.com/code-423n4/2022-08-foundation-findings/issues/31

3. ### Buyer has to pay gass fee of other royalty recipents

   - https://github.com/code-423n4/2022-08-foundation-findings/issues/165

4. ### Check wheter maximum limit to mint is applied properly or not

   - Use a mapping to track how many NFTs an address has bought instead of relying on balanceOf
   - https://github.com/code-423n4/2022-08-foundation-findings/issues/59

5. ### Vulnerabiliti
