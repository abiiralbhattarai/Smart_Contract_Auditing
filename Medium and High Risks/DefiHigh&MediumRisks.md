# Medium Risks in Defi and Finance

• Medium issues are security vulnerabilities that may not be directly exploitable or
may require certain conditions in order to be exploited. All major issues should
be addressed.

• High issues are directly exploitable security vulnerabilities that need to be fixed.

---

| Number | Issues                                                                                                                                                  |
| :----: | :------------------------------------------------------------------------------------------------------------------------------------------------------ |
|   1.   | [Functions with callbacks should have reentrancy guards in place for protection against possible malicious actors]                                      |
|   2.   | [Contract Owner Possesses Too Many Privileges](#contract-owner-possesses-too-many-privileges)                                                           |
|   3.   | [Check if the ordered value is being transffered to buyer](#check-if-the-ordered-value-is-being-transffered-to-buyer)                                   |
|   4.   | [ Check the Withdraw Logic](#check-the-withdraw-logic)                                                                                                  |
|   5.   | [ Unbounded Array should be handled with care](#unbounded-array-should-be-handled-with-care)                                                            |
|   6.   | [Important operations may run out of the gas](#important-operations-may-run-out-of-the-gas)                                                             |
|   7.   | [IERC20.transfer does not support all ERC20 token](#ierc20transfer-does-not-support-all-erc20-token)                                                    |
|   8.   | [ Check the sign with signature](#check-the-sign-with-signature)                                                                                        |
|   9.   | [Deadline should be there while adding and removing the liquidity or funds](#deadline-should-be-there-while-adding-and-removing-the-liquidity-or-funds) |
|  10.   | [ Rounding error in the operation of buying and selling](#rounding-error-in-the-operation-of-buying-and-selling)                                        |
|  11.   | [Make sure that lp token supplied is not zero when initial token is added.](#make-sure-that-lp-token-supplied-is-not-zero-when-initial-token-is-added)  |
|  12.   |                                                                                                                                                         |
|  13.   |                                                                                                                                                         |
|  14.   |                                                                                                                                                         |
|  15.   |                                                                                                                                                         |
|  16.   |                                                                                                                                                         |
|  17.   |                                                                                                                                                         |
|  18.   |                                                                                                                                                         |
|  19.   |                                                                                                                                                         |

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
- https://github.com/sherlock-audit/2022-09-harpie-judging/tree/main/005-M

15. ### Storage Gap should be there for Upgradeable Contracts

    - https://github.com/code-423n4/2022-05-rubicon-findings/issues/67

16. ### Token received by a contract might be lost if there is no way of ever retrieving

- https://github.com/code-423n4/2022-05-rubicon-findings/issues/78

17. ### Change Formula for amounts In:

```diff
- amountsIn[i - 1] = (_reserveIn * amountOut_ * 1_000) / (_reserveOut - amountOut_ * 997) + 1; To
+ amountsIn[i - 1] = (_reserveIn * amountOut_ * 1_000) / ((_reserveOut - amountOut_) * 997) + 1;

```

- https://github.com/code-423n4/2022-10-traderjoe-findings/issues/400

18. ### Delisted Token might be used for further operations

- https://github.com/code-423n4/2022-10-paladin-findings/issues/145

19. ### Check logic for feeon transfer/ Rebasing/ Inflationary/Deflationary Token

    - https://github.com/code-423n4/2022-11-size-findings/issues/47

20. ### Check the Token Conract Existence

- https://github.com/code-423n4/2022-11-size-findings/issues/48

21. ### Auction created by ERC777 Tokens with tax can be stolen by re-entrancy attack

- https://github.com/code-423n4/2022-11-size-findings/issues/192
- check re-entrancy

22. ### Replay Attacks may be possible

    - https://github.com/sherlock-audit/2022-09-harpie-judging/tree/main/004-M
    - use nonces

23. ### Check typecasting of funds while transferring or sending

- https://github.com/sherlock-audit/2022-09-harpie-judging/blob/main/018-M/1-report.md

24. ### Lack of price freshness check in ChainlinkOracle.solgetPrice()(oracle) allows a stale price to be used

- https://github.com/sherlock-audit/2022-08-sentiment-judging/blob/main/002-M/1-report.md

25. ### check the iteration of function as it may run out of gas

- https://github.com/code-423n4/2022-10-juicebox-findings/issues/64

26. ### Significant loss of precission ( Divide before multiply)

- https://github.com/code-423n4/2022-09-y2k-finance-findings/issues/225

27. ### Check if fee is being taken on risk collateral

- read the fee logic in the document
- https://github.com/code-423n4/2022-09-y2k-finance-findings/issues/44

28. ### Check if sudden change done by user affects the financial aspect of the project

- https://github.com/code-423n4/2022-09-y2k-finance-findings/issues/483

30. ### Improper reward balance checks can make some users unable to withdraw their rewards

    - https://github.com/code-423n4/2022-09-y2k-finance-findings/issues/50

31. ### Reward duration might have been increased when checking the reward

- https://code4rena.com/reports/2022-09-y2k-finance/#m-11-stakingrewards-reward-rate-can-be-dragged-out-and-diluted

32. ### check if changing some vault operations or controller of vault, we do not loss the control

    - https://github.com/code-423n4/2022-09-y2k-finance-findings/issues/66

33. ### Check for the reward logic at begining of deposit(initial 10 second)

    - as if there is no deposit for sometime in start then reward for those period is never used
    - https://github.com/code-423n4/2022-09-y2k-finance-findings/issues/93

34. ### Incorrect handling of price feed decimals

    - https://github.com/code-423n4/2022-09-y2k-finance-findings/issues/195

35. ### Contract is not the EIP complaint
    - like is the contract follow all the EIP rules properly
    - https://github.com/code-423n4/2022-09-y2k-finance-findings/issues/47

36. ### Unwanted function execute due to mistake in modifier

    - https://github.com/code-423n4/2022-09-y2k-finance-findings/issues/69

37. ### Check consequence of calling one function befor other (Order of function)
    - mainly in whithdrawing, deposting
    -  https://github.com/code-423n4/2023-01-rabbithole-findings/issues/122

38. ### Check The Dos Attack
    - https://github.com/code-423n4/2022-12-tessera-findings/issues/25

39. ### Lender is able to seize the collateral by changing the loan parameters and also oracle manuplation
    https://github.com/code-423n4/2022-04-abranft-findings/issues/51
    https://github.com/code-423n4/2022-04-abranft-findings/issues/37
