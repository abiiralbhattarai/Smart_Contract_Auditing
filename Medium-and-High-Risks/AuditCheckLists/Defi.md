| Number | Issues                                                                                                                                                                  |
| :----: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|   1.   | [Check the Signature](#check-the-signature)                                                                                                                             |
|   2.   | [Check Deadline of function related to adding and removing fund like in uniswap v2](#check-deadline-of-function-related-to-adding-and-removing-fund-like-in-uniswap-v2) |
|   3.   |                                                                                                                                                                         |

1. ### Check the Signature

   In sfrxETH contracts, the result of previewMint() changes with the state of the contract, which causes the value of amount to be volatile in the mintWithSignature function when approveMax is false.
   And when using the mintWithSignature function, which requires the user to sign for an accurate amount value, when the amount used differs from the result of previewMint(), mintWithSignature will not work.
   Consider the following scenarios.

2. ### Check Deadline of function related to adding and removing fund like in uniswap v2

- https://github.com/Uniswap/v2-periphery/blob/0335e8f7e1bd1e8d8329fd300aea2ef2f36dd19f/contracts/UniswapV2Router02.sol#L229
- https://github.com/code-423n4/2022-12-caviar-findings/issues/28

3. ### Check Rounding error related to decimals of tokem in buying, selling functions

   - https://github.com/code-423n4/2022-12-caviar-findings/issues/243

4. ### Conditions to check in the AMM and Liquidity involve

   - One area of concern is whether users can "steal" tokens, i.e. they get more than expected. For example:
     - Receiving more tokens than expected during a swap
     - Performing a successful flash loan without repaying back the loaned tokens or the fee
     - Receiving more liquidity tokens than expected when adding liquidity
     - Receiving more tokens than expected when liquidity is removed
     - Receiving more fees than expected when fees are claimed
   - Math rounding should never favour the user. For example, when liquidity is removed, token amounts should be rounded down to ensure they don't get more than expected and when an user swaps, it should round up the amountIn, while rounding down the amountOut to make sure they don't get more than expected.
     - Ensuring invariants hold. For example:
     - When adding liquidity, reserves should only increase
     - When doing a flashLoan, fees should only increase
     - When removing liquidity, reserves should only decrease
     - When swapping tokens, L should remain constant, while x and y should follow L = p \* x + y and fees should only increase
     - When adding liquidity to the active bin, if the tokens are not added with the same ratio as the reserve of the active bin, the fees should only increase
     - When claiming fees, the fees should only decrease and the reserves should stay constant

5. ### Check properly if two or more ERC20 tokens is involved in calculations.

   - If a smart contract is doing calculations with two or more ERC20 tokens and their amounts/balances, always check that either their decimals are the same or the numbers are scaled to account for different decimals. Otherwise it's pretty likely that the math can be off by a lot

6. ### Staking Contracts

   1. How to Stake?
   2. How to Unstake?
   3. How to collect reward?
   4. How to collect Fee?

7. ### AMM major checks:

   1. Remove liquidity logic
   2. Burn Logic of tokens
   3. Fee Logic
      - may be more and some time may be calcualted early
      - check whether there is cap in the fee or not
   4. Transfer functions of funds
      - whether specified amount is being sent or not.
      - whetther the rebase, deflationary token logics are considered or not
   5. Output amound logic
      - whether specified amount is being sent or not.
      - whether the rebase, deflationary token logics are considered or not
      - Balance before and after transfer of token to the contract should be same
      - check whether contract handles fee on transfer tokens
   6. Check the mathematical logic for the token in and out
   7. Reward distributin and lp distribution
      - is it given correctly? is it more or less?
   8. Check contract owner function
      - whether it will affect other function and prevent user for performing some other functions like withdraw,depostis
   9. Check the used library and interfaces
      - there might be use of vulnerable version of openzepelin
      - solmate library safetransfer token do not check whether the existence of token contract
   10. Check whether there is improper typecasting in function involving tokens and transactions
   11. Check the collateral

   - is non allowed crypto tokens are being used?

8. ### ALL checks

   1. check if there is typecasting that results in loosing fund
   2. Check whether withdrawl access is given only to msg.sender or for others too.
      - when given to others too, there mey occur problems like not able to get the reward.
      - https://github.com/code-423n4/2022-09-y2k-finance-findings/issues/281
   3. Check the withdraw and deposit mechanism properly
   4. Check the sinature replay attack possiblity ( If signature is involved)

9. ### If vault is involved

   1. Check if reward can be taken out after the vault is expired or not
   2. Check if changing some vault operations or controller of vault, we do not lost the control
      - https://github.com/code-423n4/2022-09-y2k-finance-findings/issues/66
   3. Check for the reward logic at begining of deposit(initial 10 second)

10. ### If oracle involved

    1. check if the price is stale or not
    2. check the handling of decimals of price feed
       https://github.com/code-423n4/2022-09-y2k-finance-findings/issues/195

11. ### Check important events properly

    - Like withdraw
    - like ending of epoch
      https://github.com/code-423n4/2022-09-y2k-finance-findings/issues/278

- price feed oracle

12. ### Check if used EIP is implemented properly

- https://github.com/code-423n4/2022-09-y2k-finance-findings/issues/47

13. ### check the modifiers that can trigger special function

- https://github.com/code-423n4/2022-09-y2k-finance-findings/issues/69

14. ### Check consequence of calling one function befor other

    https://github.com/code-423n4/2023-01-rabbithole-findings/issues/122

15. ### If you see a Solidity method that has an argument of type array, always check for 3 things:

    1. What if the array length is 0?
    2. What if there are duplicated elements in the array?
    3. What if there are zero value elements in the array?

16. ### Check about the DOS attack

- https://github.com/code-423n4/2022-12-tessera-findings/issues/25

17. ### check if all the parameters are validated in function

18. ### always update the success and required logic before external call to prevent rentrancy

- https://github.com/code-423n4/2022-12-tessera-findings/issues/8
