# Important Suggestion

---

- ### Generate perfect code headers every time

  Description: I recommend using header for Solidity code layout and readability

  https://github.com/transmissions11/headers

---

### Low Risk And Non Critical Issues List

| Number | Issues                                                                                                                              |
| :----: | :---------------------------------------------------------------------------------------------------------------------------------- |
|   1.   | [Not using the latest version of OpenZeppelin from dependencies](#not-using-the-latest-version-of-openzeppelin-from-dependencies)   |
|   2.   | [zero address check ](#zero-address-check)                                                                                          |
|   3.   | [Omissions in Events](#omissions-in-Events)                                                                                         |
|   4.   | [Add parameter to Event-Emit](#add-parameter-to-event-emit)                                                                         |
|   5.   | [Include return parameters in NatSpec comments](#include-return-parameters-in-natspec-comments)                                     |
|   6.   | [NatSpec is missing](#natSpec-is-missing)                                                                                           |
|   7.   | [Signature Malleability of EVM's ecrecover()](#signature-malleability-of-evms-ecrecover)                                            |
|   8.   | [Stop using this one](#stop-using-this-one)                                                                                         |
|   9.   | [Missing Time locks](#missing-time-locks)                                                                                           |
|  10.   | [DOMAIN_SEPARATOR Can Change](#domain_separator-can-change)                                                                         |
|  11.   | [Critical Address Changes Should Use Two-step Procedure](#critical-address-changes-should-use-two-step-procedure)                   |
|  12.   | [Owner can renounce Ownership](#owner-can-renounce-ownership)                                                                       |
|  13.   | [ Require messages are too short and unclear](#require-messages-are-too-short-and-unclear)                                          |
|  14.   | [ Integer overflow by unsafe casting](#integer-overflow-by-unsafe-casting)                                                          |
|  15.   | [Add indexes to events](#add-indexes-to-events)                                                                                     |
|  16.   | [It is best to test all functions]()                                                                                                |
|  17.   | [ Avoid variable names that can shade](#avoid-variable-names-that-can-shade)                                                        |
|  18.   | [Use a more recent version of Solidity](#use-a-more-recent-version-of-solidity)                                                     |
|  19.   | [ Loss of precision due to rounding](#loss-of-precision-due-to-rounding)                                                            |
|  21.   | [Use safeTransferOwnership instead of transferOwnership function](#use-safetransferownership-instead-of-transferownership-function) |
|  22.   | [Lines are too long](#lines-are-too-long)                                                                                           |
|  23.   | [ Empty blocks should be removed or Emit something](#empty-blocks-should-be-removed-or-emit-something)                              |
|  24.   | [ Missing Equivalence Checks in Setters](#missing-equivalence-checks-in-setters)                                                    |
|  25.   | [ Lack of Event Emission For Critical Functions](#lack-of-event-emission-for-critical-functions)                                    |
|  26.   | [Add missing @param information](#add-missing-param-information)                                                                    |
|  27.   | [Named return variables not used though its defined](#named-return-variables-not-used-though-its-defined)                           |
|  28.   | [Constant should be defined rather than using magic numbers](#constant-should-be-defined-rather-than-using-magic-numbers)           |
|  29.   | [use order of functions](#use-order-of-functions)                                                                                   |
|  30.   | [Avoid using tx.origin](#avoid-using-txorigin)                                                                                      |
|  31.   | [Mixing and Outdated compiler](#mixing-and-outdated-compiler)                                                                       |
|  32.   | [Lack of checks supportsInterface](#lack-of-checks-supportsinterface)                                                               |
|  33.   | [Choose Specific Compiler Version Pragma](#choose-specific-compiler-version-pragma)                                                 |
|  34.   | [ERC20 transfer / transferFrom with not checked return value](#erc20-transfer--transferfrom-with-not-checked-return-value)          |
|  35.   | [Prevent div by 0](#prevent-div-by-0)                                                                                               |
|  36.   |                                                                                                                                     |
|  37.   |                                                                                                                                     |
|  38.   |                                                                                                                                     |
|  39.   |                                                                                                                                     |
|  40.   |                                                                                                                                     |
|  41.   |                                                                                                                                     |

# Low Risk And Non Critical Issues

1.  ## Not using the latest version of OpenZeppelin from dependencies

    Use the latest version of openZeppelin

2.  ## zero address check

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

3.  ## Omissions in Events

    Events are generally emitted when sensitive changes are made to the contracts.

    The events should include the new value and old value where possible.

    emit NewEventExecution(newValue,oldValue);

4.  ## Add parameter to Event-Emit

    Some event-emit description hasn’t parameter. Add to parameter for front-end website or client app , they can has that something has happened on the blockchain.

    Events with no old value;

    emit Opened();

    emit Closed();

5.  ## Include return parameters in NatSpec comments

    If Return parameters are declared, you must prefix them with "/// @return".
    Some code analysis programs do analysis by reading NatSpec details, if they can't see the "@return" tag, they do incomplete analysis.

    Recommendation Code Style:

    ```
    /// @notice information about what a function does
    /// @param pageId The id of the page to get the URI for.
    /// @return Returns a page's URI if it has been minted
    function tokenURI(uint256 pageId) public view virtual override returns (string memory) {
    if (pageId == 0 || pageId > currentId) revert("NOT_MINTED");

        return string.concat(BASE_URI, pageId.toString());

    }
    ```

6.  ## NatSpec is missing

    Always create natspec for the functions. It will make ease in the development

7.  ## Signature Malleability of EVM's ecrecover()

    ecrecover() is a very useful Solidity function that allows the smart contract to validate that incoming data is properly signed by an expected party.

    Recommendation:\* Use the ecrecover function from OpenZeppelin's ECDSA library for signature verification.

    ***

    sussceptible to replay attack

    ***

8.  ## Stop using this one

    - Stop using v != 27 && v != 28 or v == 27 || v == 28

      See this for reference :
      https://twitter.com/alexberegszaszi/status/1534461421454606336?s=20&t=H0Dv3ZT2bicx00hLWJk7Fg

9.  ## Missing Time locks

    Description: When critical parameters of systems need to be changed, it is required to broadcast the change via event emission and recommended to enforce the changes after a time-delay. This is to allow system users to be aware of such critical changes and give them an opportunity to exit or adjust their engagement with the system accordingly. None of the onlyOwner functions that change critical protocol addresses/parameters have a timelock for a time-delayed change to alert:

    - Because of human error it's possible to set a new invalid owner. When you want to change the owner's address it's better to propose a new owner, and then accept this ownership with the new wallet.

    ***

    - users and give them a chance to engage/exit protocol if they are not agreeable to the changes

    - team in case of compromised owner(s) and give them a chance to perform incident response.

    Recommendation: Users may be surprised when critical parameters are changed. Furthermore, it can erode users' trust since they can’t be sure the protocol rules won’t be changed later on. Compromised owner keys may be used to change protocol addresses/parameters to benefit attackers. Without a time-delay, authorised owners have no time for any planned incident response.

    ```
     function setExecutionDelegate(IExecutionDelegate _executionDelegate)
       external
       onlyOwner
    {
       require(address(_executionDelegate) != address(0), "Address cannot be zero");
       executionDelegate = _executionDelegate;
       emit NewExecutionDelegate(executionDelegate);
    }

    function setPolicyManager(IPolicyManager _policyManager)
       external
       onlyOwner
    {
       require(address(_policyManager) != address(0), "Address cannot be zero");
       policyManager = _policyManager;
       emit NewPolicyManager(policyManager);
    }

    ```

    ***

10. ## DOMAIN_SEPARATOR Can Change

    Description: The variable DOMAIN_SEPARATOR is assigned in the constructor and will not change after being initialized. However, if a hard fork happens after the contract deployment, the domain would become invalid on one of the forked chains due to the block.chainid has changed.

    Recommendation: Consider the solution from [Sushi Trident](https://github.com/sushiswap/trident/blob/concentrated/contracts/pool/concentrated/TridentNFT.sol#L47-L62).

11. ## Critical Address Changes Should Use Two-step Procedure

        The critical procedures should be two step process. See similar findings in previous Code4rena contests for reference:

        https://code4rena.com/reports/2022-06-illuminate/#2-critical-changes-should-use-two-step-procedure

        Recommended Mitigation Steps Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.

        ***

        The following contracts have a function that allows them an admin to change it to a different address. If the admin accidentally uses an invalid address for which they do not have the private key, then the system gets locked.

        It is important to have two steps admin change where the first is announcing a pending new admin and the new address should then claim its ownership.

        ***

12. ## Owner can renounce Ownership

    Description: Typically, the contract’s owner is the account that deploys the contract. As a result, the owner is able to perform certain privileged activities.

    The non-fungible Ownable used in this project contract implements renounceOwnership . This can represent a certain risk if the ownership is renounced for any other reason than by design. Renouncing ownership will leave the contract without an owner, thereby removing any functionality that is only available to the owner.

    onlyOwner functions;

    Recommendation: We recommend to either reimplement the function to disable it or to clearly specify if it is part of the contract design.

13. ## Require messages are too short and unclear

    The correct and clear error description explains to the user why the function reverts, but the error descriptions below in the project are not self-explanatory. These error descriptions are very important in the debug features of DApps.Error definitions should be added to the require block, not exceeding 32 bytes.

    Donn't DO:
    require(success);

14. ## Integer overflow by unsafe casting

    Keep in mind that the version of solidity used, despite being greater than 0.8, does not prevent integer overflows during casting, it only does so in mathematical operations.

    It is necessary to safely convert between the different numeric types.

    Recommendation:

    -Use a safeCast from Open Zeppelin or increase the type length.

    problem:

    uint32(block.timestamp)

15. ## Add indexes to events

    Solidity indexes help dApps and users to filter and process the information provided by smart contracts. That is why adding indexes in values that may be interesting for the user improves the usability of the contract.

```diff
 event AuctionCreated(
-       uint256 auctionId, address seller, AuctionParameters params, Timings timings, bytes encryptedPrivKey
+       uint256 indexed auctionId, address indexed seller, AuctionParameters params, Timings timings, bytes encryptedPrivKey
 );

```

16. ## It is best to test all functions

    - Testing all functions is best practice in terms of security criteria.
    - Some function test coverage is not found in test files

17. ## Avoid variable names that can shade

    With global variable names in the form of call{value: value } , argument name similarities can shade and negatively affect code readability.

18. ## Use a more recent version of Solidity

    For security, it is best practice to use the latest Solidity version.

19. ## Loss of precision due to rounding

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

20. ## Always Use vulnerable dependency of OpenZeppelin

    Recommendation: Use patched versions

21. ## Use safeTransferOwnership instead of transferOwnership function

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

22. ## Lines are too long

    Usually lines in source code are limited to 80 characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length Reference: https://docs.soliditylang.org/en/v0.8.10/style-guide.html#maximum-line-length

23. ## Empty blocks should be removed or Emit something

    ```
    166:     constructor() initializer {}
    629:     receive() external payable {}

    ```

24. ## Missing Equivalence Checks in Setters

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

25. ## Lack of Event Emission For Critical Functions

    ```
    function updatePriorityStakingBlock(uint256 _endBlock) external onlyOwner {
        updateAccruedETHPerShares();
        priorityStakingEndBlock = _endBlock;
    }

    ```

    Description: Several functions update critical parameters that are missing event emission. These should be performed to ensure tracking of changes of such critical parameters.

    Recommendation: Consider adding events to functions that change critical parameters.

26. ## Add missing @param information
    Some parameters have not been commented, which reflects that the logic has been updated but not the documentation, it is advisable that developers update both at the same time to avoid the code being out of sync with the project documentation.

```diff
   /// @param baseToken The ERC20 to be sold by the seller
    /// @param quoteToken The ERC20 to be bid by the bidders
    /// @param reserveQuotePerBase Minimum price that bids will be filled at
    /// @param totalBaseAmount Max amount of `baseToken` to be auctioned
    /// @param minimumBidQuote Minimum quote amount a bid can buy
+   /// @param merkleRoot seller's merkleRoot for whitelist bidders
    /// @param pubKey On-chain storage of seller's ephemeral public key
    struct AuctionParameters {
        address baseToken;
        address quoteToken;
        uint256 reserveQuotePerBase;
        uint128 totalBaseAmount;
        uint128 minimumBidQuote;
        bytes32 merkleRoot;
        ECCMath.Point pubKey;
    }

```

27. # Named return variables not used though its defined

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

28. # Constant should be defined rather than using magic numbers

    It is best practice to use constant variables rather than hex/numeric literal values to make the code easier to understand and maintain, but if they are used those numbers should be well docummented.

    ```
    else if (block.timestamp > a.timings.endTimestamp + 24 hours)

    (bool res, bytes memory ret) = address(0x07).staticcall{gas: 6000}(data);

    ```

29. # use order of functions

    The solidity documentation recommends the following order for functions:

    - constructor
    - receive function (if exists)
    - fallback function (if exists)
    - external
    - public
    - internal
    - private
    - Within a grouping, place the view and pure functions last

30. # Avoid using tx.origin

    tx.origin is a global variable in Solidity that returns the address of the account that sent the transaction.

    Using the variable could make a contract vulnerable if an authorized account calls a malicious contract. You can impersonate a user using a third party contract.

    This can make it easier to create a vault on behalf of another user with an external administrator (by receiving it as an argument).

31. # Mixing and Outdated compiler

    Note that mixing pragma is not recommended. Because different compiler versions have different meanings and behaviors, it also significantly raises maintenance costs. As a result, depending on the compiler version selected for any given file, deployed contracts may have security issues

    - 0.8.14:

      ABI Encoder: When ABI-encoding values from calldata that contain nested arrays, correctly validate the nested array length against calldatasize() in all cases.
      Override Checker: Allow changing data location for parameters only when overriding external functions.

    - 0.8.15

      Code Generation: Avoid writing dirty bytes to storage when copying bytes arrays.
      Yul Optimizer: Keep all memory side-effects of inline assembly blocks.

    - 0.8.16

      Code Generation: Fix data corruption that affected ABI-encoding of calldata values represented by tuples: structs at any nesting level; argument lists of external functions, events and errors; return value lists of external functions. The 32 leading bytes of the first dynamically-encoded value in the tuple would get zeroed when the last component contained a statically-encoded array.

    - 0.8.17

      Yul Optimizer: Prevent the incorrect removal of storage writes before calls to Yul functions that conditionally terminate the external EVM call.

32. # Lack of checks supportsInterface

    The EIP-165 standard helps detect that a smart contract implements the expected logic, prevents human error when configuring smart contract bindings, so it is recommended to check that the received argument is a contract and supports the expected interface.

    - Reference:

      https://eips.ethereum.org/EIPS/eip-165

33. # Choose Specific Compiler Version Pragma

    Avoid floating pragmas for non-library contracts. While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations. A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain. It is recommended to pin to a concrete compiler version,

    e.g. 'pragma solidity ^0.8.0;' -> 'pragma solidity 0.8.4;"

34. # ERC20 transfer / transferFrom with not checked return value

    - Impact
      Not every ERC20 token follows OpenZeppelin's recommendation. It's possible (inside ERC20 standard) that a transferFrom doesn't revert upon failure but returns false.

35. # Prevent div by 0

    - Impact
      On several locations in the code precautions are not being taken to not divide by 0, this would revert the code.
    - Recommended Mitigation Steps
      Recommend making sure division by 0 won’t occur by checking the variables beforehand and handling this edge case.
      ***
      - uint minimumCollateral = debt _ 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) _ 10000 / collateralFactorBps;//
      - uint liquidationFee = repaidDebt _ 1 ether / price _ liquidationFeeBps / 10000;

36. #
