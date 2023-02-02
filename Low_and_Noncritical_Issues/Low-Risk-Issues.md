### Low Risk Issues List

| Number | Issues                                                                                                                              |
| :----: | :---------------------------------------------------------------------------------------------------------------------------------- |
|   1.   | [zero address check ](#zero-address-check)                                                                                          |
|   2.   | [Signature Malleability of EVM's ecrecover()](#signature-malleability-of-evms-ecrecover)                                            |
|   3.   | [Stop using this one](#stop-using-this-one)                                                                                         |
|   4.   | [DOMAIN_SEPARATOR Can Change](#domain_separator-can-change)                                                                         |
|   5.   | [Owner can renounce Ownership](#owner-can-renounce-ownership)                                                                       |
|   6.   | [ Integer overflow by unsafe casting](#integer-overflow-by-unsafe-casting)                                                          |
|   7.   | [ Loss of precision due to rounding](#loss-of-precision-due-to-rounding)                                                            |
|   8.   | [Use safeTransferOwnership instead of transferOwnership function](#use-safetransferownership-instead-of-transferownership-function) |
|   9.   | [ Missing Equivalence Checks in Setters](#missing-equivalence-checks-in-setters)                                                    |
|  10.   | [Avoid using tx.origin](#avoid-using-txorigin)                                                                                      |
|  11.   | [Lack of checks supportsInterface](#lack-of-checks-supportsinterface)                                                               |
|  12.   | [ERC20 transfer / transferFrom with not checked return value](#erc20-transfer--transferfrom-with-not-checked-return-value)          |
|  13.   | [Prevent div by 0](#prevent-div-by-0)                                                                                               |
|  14.   | [Always Use non-vulnerable dependency of OpenZeppelin](#always-use-non-vulnerable-dependency-of-openzeppelin)                       |
|  15.   |                                                                                                                                     |
|  16.   |                                                                                                                                     |
|  17.   |                                                                                                                                     |
|  18.   |                                                                                                                                     |
|  19.   |                                                                                                                                     |
|  21.   |                                                                                                                                     |
|  22.   |                                                                                                                                     |
|  23.   |                                                                                                                                     |
|  24.   |                                                                                                                                     |
|  25.   |                                                                                                                                     |
|  26.   |                                                                                                                                     |
|  27.   |                                                                                                                                     |
|  28.   |                                                                                                                                     |
|  29.   |                                                                                                                                     |
|  30.   |                                                                                                                                     |
|  31.   |                                                                                                                                     |
|  32.   |                                                                                                                                     |
|  33.   |                                                                                                                                     |
|  34.   |                                                                                                                                     |
|  35.   |                                                                                                                                     |
|  36.   |                                                                                                                                     |
|  37.   |                                                                                                                                     |
|  38.   |                                                                                                                                     |
|  39.   |                                                                                                                                     |
|  40.   |                                                                                                                                     |
|  41.   |                                                                                                                                     |

---

# Low Risk Issues

1.  ## zero address check

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

2.  ## Signature Malleability of EVM's ecrecover()

    ecrecover() is a very useful Solidity function that allows the smart contract to validate that incoming data is properly signed by an expected party.

    Recommendation:\* Use the ecrecover function from OpenZeppelin's ECDSA library for signature verification.

    ***

    sussceptible to replay attack

    ***

3.  ## Stop using this one

    - Stop using v != 27 && v != 28 or v == 27 || v == 28

      See this for reference :
      https://twitter.com/alexberegszaszi/status/1534461421454606336?s=20&t=H0Dv3ZT2bicx00hLWJk7Fg

4.  ## DOMAIN_SEPARATOR Can Change

    Description: The variable DOMAIN_SEPARATOR is assigned in the constructor and will not change after being initialized. However, if a hard fork happens after the contract deployment, the domain would become invalid on one of the forked chains due to the block.chainid has changed.

    Recommendation: Consider the solution from [Sushi Trident](https://github.com/sushiswap/trident/blob/concentrated/contracts/pool/concentrated/TridentNFT.sol#L47-L62).

5.  ## Owner can renounce Ownership

    Description: Typically, the contract’s owner is the account that deploys the contract. As a result, the owner is able to perform certain privileged activities.

    The non-fungible Ownable used in this project contract implements renounceOwnership . This can represent a certain risk if the ownership is renounced for any other reason than by design. Renouncing ownership will leave the contract without an owner, thereby removing any functionality that is only available to the owner.

    onlyOwner functions;

    Recommendation: We recommend to either reimplement the function to disable it or to clearly specify if it is part of the contract design.

6.  ## Integer overflow by unsafe casting

    Keep in mind that the version of solidity used, despite being greater than 0.8, does not prevent integer overflows during casting, it only does so in mathematical operations.

    It is necessary to safely convert between the different numeric types.

    Recommendation:

    -Use a safeCast from Open Zeppelin or increase the type length.

    problem:

    uint32(block.timestamp)

7.  ## Loss of precision due to rounding

    Due to / PRECISION, users can avoid paying fee if claimed [][] result is below PRECISION

    ```
    contracts/liquid-staking/GiantMevAndFeesPool.sol:
    199
    200:     /// @dev Internal re-usable method for setting claimed to max for msg.sender
    201:     function _setClaimedToMax(address _user) internal {
    202:         // New ETH stakers are not entitled to ETH earned by
    203:         claimed[_user][address(lpTokenETH)] = (accumulatedETHPerLPShare * lpTokenETH.balanceOf(_user)) / PRECISION;
    204:     }

    ```

8.  ## Use safeTransferOwnership instead of transferOwnership function

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

9.  ## Missing Equivalence Checks in Setters

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

10. # Named return variables not used though its defined

    When Named return variable are declared they should be used inside the function instead of the return statement or if not they should be removed to avoid confusion.

    ```
    function tokensAvailableForWithdrawal(uint256 auctionId, uint128 baseAmount)
        public
        view
        returns (uint128 tokensAvailable)
    {
        Auction storage a = idToAuction[auctionId];
        return CommonTokenMath.tokensAvailableAtTime(
            a.timings.vestingStartTimestamp,
            a.timings.vestingEndTimestamp,
            uint32(block.timestamp),
            a.timings.cliffPercent,
            baseAmount
        );
    }

    ```

11. # Avoid using tx.origin

    tx.origin is a global variable in Solidity that returns the address of the account that sent the transaction.

    Using the variable could make a contract vulnerable if an authorized account calls a malicious contract. You can impersonate a user using a third party contract.

    This can make it easier to create a vault on behalf of another user with an external administrator (by receiving it as an argument).

12. # Lack of checks supportsInterface

    The EIP-165 standard helps detect that a smart contract implements the expected logic, prevents human error when configuring smart contract bindings, so it is recommended to check that the received argument is a contract and supports the expected interface.

    - Reference:

      https://eips.ethereum.org/EIPS/eip-165

13. # ERC20 transfer / transferFrom with not checked return value

    - Impact
      Not every ERC20 token follows OpenZeppelin's recommendation. It's possible (inside ERC20 standard) that a transferFrom doesn't revert upon failure but returns false.

14. # Prevent div by 0

    - Impact
      On several locations in the code precautions are not being taken to not divide by 0, this would revert the code.
    - Recommended Mitigation Steps
      Recommend making sure division by 0 won’t occur by checking the variables beforehand and handling this edge case.
      ***
      - uint minimumCollateral = debt _ 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) _ 10000 / collateralFactorBps;//
      - uint liquidationFee = repaidDebt _ 1 ether / price _ liquidationFeeBps / 10000;

15. ## Always Use non-vulnerable dependency of OpenZeppelin

    - Recommendation: Use patched versions