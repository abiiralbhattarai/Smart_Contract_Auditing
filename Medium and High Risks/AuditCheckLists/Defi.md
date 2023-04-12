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
4.
