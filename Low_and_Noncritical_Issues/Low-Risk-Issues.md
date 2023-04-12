### Low Risk Issues List

| Number | Issues                                                                                                                                  |
| :----: | :-------------------------------------------------------------------------------------------------------------------------------------- |
|   1.   | [zero address check ](#zero-address-check)                                                                                              |
|   2.   | [Signature Malleability of EVM's ecrecover()](#signature-malleability-of-evms-ecrecover)                                                |
|   3.   | [Stop using this one](#stop-using-this-one)                                                                                             |
|   4.   | [DOMAIN_SEPARATOR Can Change](#domain_separator-can-change)                                                                             |
|   5.   | [Owner can renounce Ownership](#owner-can-renounce-ownership)                                                                           |
|   6.   | [ Integer overflow by unsafe casting](#integer-overflow-by-unsafe-casting)                                                              |
|   7.   | [ Loss of precision due to rounding](#loss-of-precision-due-to-rounding)                                                                |
|   8.   | [Use safeTransferOwnership instead of transferOwnership function](#use-safetransferownership-instead-of-transferownership-function)     |
|   9.   | [ Missing Equivalence Checks in Setters](#missing-equivalence-checks-in-setters)                                                        |
|  10.   | [Avoid using tx.origin](#avoid-using-txorigin)                                                                                          |
|  11.   | [Lack of checks supportsInterface](#lack-of-checks-supportsinterface)                                                                   |
|  12.   | [ERC20 transfer / transferFrom with not checked return value](#erc20-transfer--transferfrom-with-not-checked-return-value)              |
|  13.   | [Prevent div by 0](#prevent-div-by-0)                                                                                                   |
|  14.   | [Always Use non-vulnerable dependency of OpenZeppelin](#always-use-non-vulnerable-dependency-of-openzeppelin)                           |
|  15.   | [Missing sanity checks on to addresses](#missing-sanity-checks-on-to-addresses)                                                         |
|  16.   | [Add non-zero address checks for address arguments in constructors](#add-non-zero-address-checks-for-address-arguments-in-constructors) |
|  17.   | [Use `_safeMint` instead of `_mint`](#use-_safemint-instead-of-_mint)                                                                   |
|  18.   | [Missing Contract-existence Checks Before Low-level Calls](#missing-contract-existence-checks-before-low-level-calls)                   |
|  19.   | [fulfillRandomWords must not revert](#fulfillrandomwords-must-not-revert)                                                               |
|  20.   | [Don't use payable.transfer()/payable.send()](#dont-use-payabletransferpayablesend)                                                     |
|  21.   | [Unused/empty receive()/fallback() function ](#initialize-functions-can-be-frontrun)                                                    |
|  22.   | [Minting tokens to the zero address should be avoided](#minting-tokens-to-the-zero-address-should-be-avoided)                           |
|  23.   | [ Use call() instead of transfer() when transferring ETH](#use-call-instead-of-transfer-when-transferring-eth)                          |
|  24.   | [Initialize functions can be frontrun ](#initialize-functions-can-be-frontrun)                                                          |
|  25.   |                                                                                                                                         |
|  26.   |                                                                                                                                         |
|  27.   |                                                                                                                                         |
|  28.   |                                                                                                                                         |
|  29.   |                                                                                                                                         |
|  30.   |                                                                                                                                         |
|  31.   |                                                                                                                                         |
|  32.   |                                                                                                                                         |
|  33.   |                                                                                                                                         |
|  34.   |                                                                                                                                         |
|  35.   |                                                                                                                                         |
|  36.   |                                                                                                                                         |
|  37.   |                                                                                                                                         |
|  38.   |                                                                                                                                         |
|  39.   |                                                                                                                                         |
|  40.   |                                                                                                                                         |
|  41.   |                                                                                                                                         |

---

# Low Risk Issues

1.  ### zero address check

    Do zero address check

    ```
    function setOracle(address \_oracle)
    external onlyOwner
    {
    require(\_oracle != address(0), "Address cannot be zero");
    oracle = \_oracle;
    emit NewOracle(oracle);
    }
    ```

2.  ### Signature Malleability of EVM's ecrecover()

    ecrecover() is a very useful Solidity function that allows the smart contract to validate that incoming data is properly signed by an expected party.

    Recommendation:\* Use the ecrecover function from OpenZeppelin's ECDSA library for signature verification.

    ***

    sussceptible to replay attack

    ***

3.  ### Stop using this one

    - Stop using v != 27 && v != 28 or v == 27 || v == 28
    - This code is part of signature verification in contracts, where a special contract ("ecrecover precompile") takes the signature components (v,r,s) and returns an address.

      See this for reference :
      https://twitter.com/alexberegszaszi/status/1534461421454606336?s=20&t=H0Dv3ZT2bicx00hLWJk7Fg

4.  ### DOMAIN_SEPARATOR Can Change

    Description: The variable DOMAIN_SEPARATOR is assigned in the constructor and will not change after being initialized. However, if a hard fork happens after the contract deployment, the domain would become invalid on one of the forked chains due to the block.chainid has changed.

    ```
    In Ethereum and Solidity, a domain separator is a cryptographic construct used to prevent replay attacks in smart contract interactions.

    A replay attack occurs when an attacker intercepts a legitimate message sent from a user to a smart contract and replays it later, causing the contract to execute the same action multiple times. This can lead to unintended consequences, such as double-spending or the depletion of contract funds.

    To prevent this type of attack, Ethereum uses a domain separator, which is a unique value that is included in every transaction sent to a smart contract. The domain separator is calculated based on several inputs, including the address of the contract, the function being called, and a nonce value that is incremented for each transaction.

    When a transaction is sent to a smart contract, the domain separator is included in the data payload along with the function parameters. The contract then uses this value to generate a unique hash for the transaction, which is used to verify its authenticity and prevent replay attacks.

    In Solidity, the domain separator is typically defined as a constant variable in the contract code.
    ```

    Recommendation: Consider the solution from [Sushi Trident](https://github.com/sushiswap/trident/blob/concentrated/contracts/pool/concentrated/TridentNFT.sol#L47-L62).

5.  ### Owner can renounce Ownership

    Description: Typically, the contract’s owner is the account that deploys the contract. As a result, the owner is able to perform certain privileged activities.

    The non-fungible Ownable used in this project contract implements renounceOwnership . This can represent a certain risk if the ownership is renounced for any other reason than by design. Renouncing ownership will leave the contract without an owner, thereby removing any functionality that is only available to the owner.

    onlyOwner functions;

    Recommendation: We recommend to either reimplement the function to disable it or to clearly specify if it is part of the contract design.

6.  ### Integer overflow by unsafe casting

    Keep in mind that the version of solidity used, despite being greater than 0.8, does not prevent integer overflows during casting, it only does so in mathematical operations.

    It is necessary to safely convert between the different numeric types.

    Recommendation:

    -Use a safeCast from Open Zeppelin or increase the type length.

    problem:

    uint32(block.timestamp)

7.  ### Loss of precision due to rounding

    Due to / PRECISION, users can avoid paying fee if claimed [ ][ ] result is below PRECISION

    ```
    contracts/liquid-staking/GiantMevAndFeesPool.sol:
    199
    200:     /// @dev Internal re-usable method for setting claimed to max for msg.sender
    201:     function _setClaimedToMax(address _user) internal {
    202:         // New ETH stakers are not entitled to ETH earned by
    203:         claimed[_user][address(lpTokenETH)] = (accumulatedETHPerLPShare * lpTokenETH.balanceOf(_user)) / PRECISION;
    204:     }

    ```

8.  ### Use safeTransferOwnership instead of transferOwnership function

    Description: transferOwnership function is used to change Ownership

    ```
    ** import { Ownable } from "@openzeppelin/contracts/access/Ownable.sol";
    93      /// @inheritdoc IOwnableSmartWallet
    94:     function transferOwnership(address newOwner)
    95:         public
    96:         override(IOwnableSmartWallet, Ownable)
    97:     {
    98:         // Only the owner themselves or an address that is approved for transfers
    99:         // is authorized to do this
    100:         require(
    101:             isTransferApproved(owner(), msg.sender),
    102:             "OwnableSmartWallet: Transfer is not allowed"
    103:         ); // F: [OSW-4]
    104:
    105:         // Approval is revoked, in order to avoid unintended transfer allowance
    106:         // if this wallet ever returns to the previous owner
    107:         if (msg.sender != owner()) {
    108:             _setApproval(owner(), msg.sender, false); // F: [OSW-5]
    109:         }
    110:         _transferOwnership(newOwner); // F: [OSW-5]
    111:     }

    ```

    Use a 2 structure transferOwnership which is safer. safeTransferOwnership, use it is more secure due to 2-stage ownership transfer.

    Recommendation:

    - Use [Ownable2Step.sol] (https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol)

9.  ### Missing Equivalence Checks in Setters

    Description: Setter functions are missing checks to validate if the new value being set is the same as the current value already set in the contract. Such checks will showcase mismatches between on-chain and off-chain states.

    Recommendation: This may hinder detecting discrepancies between on-chain and off-chain states leading to flawed assumptions of on-chain state and protocol behavior.

    ```
     function updateTicker(string calldata _newTicker) external onlyDAO {
        require(bytes(_newTicker).length >= 3, "String must be 3-5 characters long");
        require(bytes(_newTicker).length <= 5, "String must be 3-5 characters long");
        require(numberOfKnots == 0, "Cannot change ticker once house is created");

        stakehouseTicker = _newTicker;

        emit NetworkTickerUpdated(_newTicker);
    }

    ```

10. ### Avoid using tx.origin

    tx.origin is a global variable in Solidity that returns the address of the account that sent the transaction.

    Using the variable could make a contract vulnerable if an authorized account calls a malicious contract. You can impersonate a user using a third party contract.

    This can make it easier to create a vault on behalf of another user with an external administrator (by receiving it as an argument).

11. ### Lack of checks supportsInterface

    The EIP-165 standard helps detect that a smart contract implements the expected logic, prevents human error when configuring smart contract bindings, so it is recommended to check that the received argument is a contract and supports the expected interface.

    - Reference:

      https://eips.ethereum.org/EIPS/eip-165

12. ### ERC20 transfer / transferFrom with not checked return value

    - Impact
      Not every ERC20 token follows OpenZeppelin's recommendation. It's possible (inside ERC20 standard) that a transferFrom doesn't revert upon failure but returns false.

13. ### Prevent div by 0

    - Impact
      On several locations in the code precautions are not being taken to not divide by 0, this would revert the code.
    - Recommended Mitigation Steps
      Recommend making sure division by 0 won’t occur by checking the variables beforehand and handling this edge case.
      ***
      - uint minimumCollateral = debt _ 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) _ 10000 / collateralFactorBps;//
      - uint liquidationFee = repaidDebt _ 1 ether / price _ liquidationFeeBps / 10000;

14. ### Always Use non-vulnerable dependency of OpenZeppelin

    - Recommendation: Use patched versions

15. ### Missing sanity checks on to addresses

when the public/external functions require an address to as a parameter to which to send either tokens or ETH, the protocol should check that if the to address is contract then that contract should be able to manage ERC20, otherwise funds would be lost.

```
   function swapAVAXForExactTokens(
        uint256 _amountOut,
        uint256[] memory _pairBinSteps,
        IERC20[] memory _tokenPath,
        address _to,
        uint256 _deadline
    )

```

16. ### Add non-zero address checks for address arguments in constructors

check address value for zero

```
 constructor(
       ILBFactory _factory,
       IJoeFactory _oldFactory,
       IWAVAX _wavax
   ) {
       factory = _factory;
       oldFactory = _oldFactory;
       wavax = _wavax;
   }
```

17. ### Use `_safeMint` instead of `_mint`

    According to openzepplin's ERC721, the use of `_mint` is discouraged, use `_safeMint` whenever possible.

    - https://docs.openzeppelin.com/contracts/3.x/api/token/erc721#ERC721-_mint-address-uint256-

18. ### Missing Contract-existence Checks Before Low-level Calls

    - Low-level calls return success if there is no code present at the specified address.
    - In addition to the zero-address checks, add a check to verify that <address>.code.length > 0

19. ### fulfillRandomWords must not revert

    According to Chainlink's documentation, fulfillRandomWords should not revert

        fulfillRandomWords must not revert If your fulfillRandomWords() implementation reverts, the VRF service will not attempt to call it a second time. Make sure your contract logic does not revert. Consider simply storing the randomness and taking more complex follow-on actions in separate contract calls made by you, your users, or an Automation Node.

    - code: https://github.com/code-423n4/2022-12-forgeries/blob/main/src/VRFNFTRandomDraw.sol#L236

20. ### Don't use payable.transfer()/payable.send()

    The use of payable.transfer() is heavily [frowned upon](https://consensys.net/diligence/blog/2019/09/stop-using-soliditys-transfer-now/) because it can lead to the locking of funds. The transfer() call requires that the recipient is either an EOA account, or is a contract that has a payable callback. For the contract case, the transfer() call only provides 2300 gas for the contract to complete its operations. This means the following cases can cause the transfer to fail:

    The contract does not have a payable callback
    The contract's payable callback spends more than 2300 gas (which is only enough to emit something)
    The contract is called through a proxy which itself uses up the 2300 gas Use OpenZeppelin's Address.sendValue() instead

    ```
    File: contracts/utils/LineLib.sol
        48: payable(receiver).transfer(amount);
    ```

21. ### Unused/empty receive()/fallback() function

    If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. require(msg.sender == address(weth))). Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds

    ```
    receive() external payable {}
    ```

22. ### Minting tokens to the zero address should be avoided
        Consider applying a check in the function to ensure tokens aren’t minted to the zero address.
    ```
        function mint(
    72: address to,
    73: uint256 collateral,
    74: bytes calldata data
    75: )
    76: external
    77: override
    78: nonReentrant
    79: returns (uint256 shares)
    80: {
        //other code but no check of address to
    }
    ```
23. ### Use call() instead of transfer() when transferring ETH

    When transferring ETH, use call() instead of transfer().

    The transfer() function only allows the recipient to use 2300 gas. If the recipient uses more than that, transfers will fail. In the future gas costs might change increasing the likelihood of that happening.

    Keep in mind that call() introduces the risk of reentrancy. But, as long as the router follows the checks effects interactions pattern it should be fine. It's not supposed to hold any tokens anyway.

    ```
    - Don't do
    msg.sender.transfer(delta);
    ```

24. ### Initialize functions can be frontrun

    https://github.com/code-423n4/2021-08-notional-findings/issues/59
