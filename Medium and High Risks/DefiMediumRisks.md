# Medium Risks in Defi and Finance

---

| Number | Issues                                                                                                                                                                                                                                |
| :----: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|   1.   | [Functions with callbacks should have reentrancy guards in place for protection against possible malicious actors](#functions-with-callbacks-should-have-reentrancy-guards-in-place-for-protection-against-possible-malicious-actors) |
|   2.   | [Contract Owner Possesses Too Many Privileges](#contract-owner-possesses-too-many-privileges)                                                                                                                                         |
|   3.   | [Check if the ordered value is being transffered to buyer](#check-if-the-ordered-value-is-being-transffered-to-buyer)                                                                                                                 |
|   4.   | [ Check the Withdraw Logic](#check-the-withdraw-logic)                                                                                                                                                                                |
|   5.   | [ Unbounded Array should be handled with care](#unbounded-array-should-be-handled-with-care)                                                                                                                                          |
|   6.   | [Important operations may run out of the gas](#important-operations-may-run-out-of-the-gas)                                                                                                                                           |
|   7.   | [IERC20.transfer does not support all ERC20 token](#ierc20transfer-does-not-support-all-erc20-token)                                                                                                                                  |
|   8.   | [ Check the sign with signature](#check-the-sign-with-signature)                                                                                                                                                                      |
|   9.   | [Deadline should be there while adding and removing the liquidity or funds](#deadline-should-be-there-while-adding-and-removing-the-liquidity-or-funds)                                                                               |
|  10.   | [Rounding error in the operation of buying and selling](#rounding-error-in-the-operation-of-buying-and-selling)                                                                                                                       |
|  11.   | [Make sure that lp token supplied is not zero when initial token is added.](#make-sure-that-lp-token-supplied-is-not-zero-when-initial-token-is-added)                                                                                |
|  12.   |                                                                                                                                                                                                                                       |
|  13.   |                                                                                                                                                                                                                                       |
|  14.   |                                                                                                                                                                                                                                       |
|  15.   |                                                                                                                                                                                                                                       |
|  16.   |                                                                                                                                                                                                                                       |
|  17.   |                                                                                                                                                                                                                                       |
|  18.   |                                                                                                                                                                                                                                       |
|  19.   |                                                                                                                                                                                                                                       |

---

---

1. ### Functions with callbacks should have reentrancy guards in place for protection against possible malicious actors

   -[Vulnerability Details](https://github.com/code-423n4/2022-01-timeswap-findings/issues/43)

2. ### Contract Owner Possesses Too Many Privileges

   - https://github.com/code-423n4/2022-10-blur-findings/issues/235

3. ### Check if the ordered value is being transffered to buyer

   - https://github.com/code-423n4/2022-10-blur-findings/issues/666

4. ### Check the Withdraw Logic

   - https://code4rena.com/reports/2022-09-frax#h-02-frontrunning-by-malicious-validator

5. ### Unbounded Array should be handled with care

   - https://github.com/code-423n4/2022-09-frax-findings/issues/12

6. ### Important operations may run out of the gas

   - https://github.com/code-423n4/2022-09-frax-findings/issues/17
   - https://github.com/code-423n4/2022-09-frax-findings/issues/12

7. ### IERC20.transfer does not support all ERC20 token

   - https://github.com/code-423n4/2022-09-frax-findings/issues/233

8. ### Check the sign with signature

   - https://github.com/code-423n4/2022-09-frax-findings/issues/35

9. ### Deadline should be there while adding and removing the liquidity or funds

   - Check the deadline
   - https://github.com/code-423n4/2022-12-caviar-findings/issues/28

10. ### Rounding error in the operation of buying and selling

    - https://github.com/code-423n4/2022-12-caviar-findings/issues/243

11. ### Make sure that lp token supplied is not zero when initial token is added.

    - First depositor can break minting of shares
    - https://github.com/code-423n4/2022-12-caviar-findings/issues/442

12. ### When possible always transfer LP token at last to manage reentrancy
    Reentrancy in buy function for ERC777 tokens allows buying funds with considerable discount
    - ERC77 has hooks and behave abit differently than ERC20
    - https://github.com/code-423n4/2022-12-caviar-findings/issues/343

13. ### Check whether there is address zero check or not for recepients
    https://github.com/code-423n4/2022-08-foundation-findings/issues/31

14. ### Check the logic for deflationary token in the contract
 - https://github.com/code-423n4/2022-05-rubicon-findings/issues/126

15. ###  Storage Gap should be there for Upgradeable Contracts
    - https://github.com/code-423n4/2022-05-rubicon-findings/issues/67

16. ### Token received by a contract might be lost if there is no way of ever retrieving
https://github.com/code-423n4/2022-05-rubicon-findings/issues/78