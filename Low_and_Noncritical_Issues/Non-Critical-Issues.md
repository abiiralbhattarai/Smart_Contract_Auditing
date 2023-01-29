### Non Critical Issues List

| Number | Issues                                                                                                                            |
| :----: | :-------------------------------------------------------------------------------------------------------------------------------- |
|   1.   | [Not using the latest version of OpenZeppelin from dependencies](#not-using-the-latest-version-of-openzeppelin-from-dependencies) |
|   3.   | [Omissions in Events](#omissions-in-Events)                                                                                       |
|   4.   | [Add parameter to Event-Emit](#add-parameter-to-event-emit)                                                                       |
|   5.   | [Include return parameters in NatSpec comments](#include-return-parameters-in-natspec-comments)                                   |
|   6.   | [NatSpec is missing](#natSpec-is-missing)                                                                                         |
|   7.   | [Missing Time locks](#missing-time-locks)                                                                                         |
|   8.   | [Critical Address Changes Should Use Two-step Procedure](#critical-address-changes-should-use-two-step-procedure)                 |
|   9.   | [ Require messages are too short and unclear](#require-messages-are-too-short-and-unclear)                                        |
|  10.   | [Add indexes to events](#add-indexes-to-events)                                                                                   |
|  11.   | [It is best to test all functions](#it-is-best-to-test-all-functions)                                                             |
|  12.   | [ Avoid variable names that can shade](#avoid-variable-names-that-can-shade)                                                      |
|  13.   | [Use a more recent version of Solidity](#use-a-more-recent-version-of-solidity)                                                   |
|  14.   | [Lines are too long](#lines-are-too-long)                                                                                         |
|  15.   | [ Empty blocks should be removed or Emit something](#empty-blocks-should-be-removed-or-emit-something)                            |
|  16.   | [ Lack of Event Emission For Critical Functions](#lack-of-event-emission-for-critical-functions)                                  |
|  17.   | [Add missing @param information](#add-missing-param-information)                                                                  |
|  18.   | [Named return variables not used though its defined](#named-return-variables-not-used-though-its-defined)                         |
|  19.   | [Constant should be defined rather than using magic numbers](#constant-should-be-defined-rather-than-using-magic-numbers)         |
|  20.   | [use order of functions](#use-order-of-functions)                                                                                 |
|  21.   | [Mixing and Outdated compiler](#mixing-and-outdated-compiler)                                                                     |
|  22.   | [Choose Specific Compiler Version Pragma](#choose-specific-compiler-version-pragma)                                               |
|  23.   |                                                                                                                                   |
|  24.   |                                                                                                                                   |
|  25.   |                                                                                                                                   |
|  26.   |                                                                                                                                   |
|  27.   |                                                                                                                                   |
|  28.   |                                                                                                                                   |
|  29.   |                                                                                                                                   |
|  30.   |                                                                                                                                   |

---

# Non Critical Issues

1.  ## Not using the latest version of OpenZeppelin from dependencies

    Use the latest version of openZeppelin

2.  ## Omissions in Events

    Events are generally emitted when sensitive changes are made to the contracts.

    The events should include the new value and old value where possible.

    emit NewEventExecution(newValue,oldValue);

3.  ## Add parameter to Event-Emit

    Some event-emit description hasn’t parameter. Add to parameter for front-end website or client app , they can has that something has happened on the blockchain.

    Events with no old value;

    emit Opened();

    emit Closed();

4.  ## Include return parameters in NatSpec comments

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

5.  ## NatSpec is missing

    Always create natspec for the functions. It will make ease in the development

6.  ## Missing Time locks

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

7.  ## Critical Address Changes Should Use Two-step Procedure

        The critical procedures should be two step process. See similar findings in previous Code4rena contests for reference:

        https://code4rena.com/reports/2022-06-illuminate/#2-critical-changes-should-use-two-step-procedure

        Recommended Mitigation Steps Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.

        ***

        The following contracts have a function that allows them an admin to change it to a different address. If the admin accidentally uses an invalid address for which they do not have the private key, then the system gets locked.

        It is important to have two steps admin change where the first is announcing a pending new admin and the new address should then claim its ownership.

        ***

8.  ## Require messages are too short and unclear

    The correct and clear error description explains to the user why the function reverts, but the error descriptions below in the project are not self-explanatory. These error descriptions are very important in the debug features of DApps.Error definitions should be added to the require block, not exceeding 32 bytes.

    Donn't DO:
    require(success);

9.  ## Add indexes to events

    Solidity indexes help dApps and users to filter and process the information provided by smart contracts. That is why adding indexes in values that may be interesting for the user improves the usability of the contract.

```diff
 event AuctionCreated(
-       uint256 auctionId, address seller, AuctionParameters params, Timings timings, bytes encryptedPrivKey
+       uint256 indexed auctionId, address indexed seller, AuctionParameters params, Timings timings, bytes encryptedPrivKey
 );

```

10. ## It is best to test all functions

    - Testing all functions is best practice in terms of security criteria.
    - Some function test coverage is not found in test files

11. ## Avoid variable names that can shade

    With global variable names in the form of call{value: value } , argument name similarities can shade and negatively affect code readability.

12. ## Use a more recent version of Solidity

    For security, it is best practice to use the latest Solidity version.

13. ## Don't Use vulnerable dependency of OpenZeppelin

    Recommendation: Use patched versions

14. ## Lines are too long

    Usually lines in source code are limited to 80 characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length Reference: https://docs.soliditylang.org/en/v0.8.10/style-guide.html#maximum-line-length

15. ## Empty blocks should be removed or Emit something

    ```
    166:     constructor() initializer {}
    629:     receive() external payable {}

    ```

16. ## Lack of Event Emission For Critical Functions

    ```
    function updatePriorityStakingBlock(uint256 _endBlock) external onlyOwner {
        updateAccruedETHPerShares();
        priorityStakingEndBlock = _endBlock;
    }

    ```

    Description: Several functions update critical parameters that are missing event emission. These should be performed to ensure tracking of changes of such critical parameters.

    Recommendation: Consider adding events to functions that change critical parameters.

17. ## Add missing @param information
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

18. # Named return variables not used though its defined

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

19. # Constant should be defined rather than using magic numbers

    It is best practice to use constant variables rather than hex/numeric literal values to make the code easier to understand and maintain, but if they are used those numbers should be well docummented.

    - Summary
      Magic numbers are hardcoded numbers used in the code which are ambiguous to their intended purpose. These should be replaced with constants to make code more readable and maintainable.
    - Details
      Values are hardcoded and would be more readable and maintainable if declared as a constant

    ```
    else if (block.timestamp > a.timings.endTimestamp + 24 hours)

    (bool res, bytes memory ret) = address(0x07).staticcall{gas: 6000}(data);

    ```

    - Mitigation
      Replace magic hardcoded numbers with declared constants.

20. # use order of functions

    The solidity documentation recommends the following order for functions:

    - constructor
    - receive function (if exists)
    - fallback function (if exists)
    - external
    - public
    - internal
    - private
    - Within a grouping, place the view and pure functions last

21. # Mixing and Outdated compiler

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

22. # Choose Specific Compiler Version Pragma

    Avoid floating pragmas for non-library contracts. While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations. A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain. It is recommended to pin to a concrete compiler version,

    e.g. 'pragma solidity ^0.8.0;' -> 'pragma solidity 0.8.4;"
