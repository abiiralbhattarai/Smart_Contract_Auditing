# Important Suggestion

---

- ### Generate perfect code headers every time

  Description: I recommend using header for Solidity code layout and readability

  https://github.com/transmissions11/headers

---

### Low Risk Issues List

| Number | Issues Details                                                      |    Context    |
| :----: | :------------------------------------------------------------------ | :-----------: |
| [L-01] | Draft Openzeppelin Dependencies                                     |       1       |
| [L-02] | Stack too deep when compiling                                       |               |
| [L-03] | Remove unused code                                                  |       2       |
| [L-04] | Insufficient coverage                                               |               |
| [L-05] | Critical Address Changes Should Use Two-step Procedure              |               |
| [L-06] | Avoid variable names that can shade                                 |       1       |
| [L-07] | Use a more recent version of Solidity                               | All contracts |
| [L-08] | Owner can renounce Ownership                                        |       2       |
| [L-09] | _Lock pragmas_ to specific compiler version                         |      24       |
| [L-10] | Loss of precision due to rounding                                   |       1       |
| [L-11] | Using vulnerable dependency of OpenZeppelin                         |       1       |
| [L-12] | Use `safeTransferOwnership` instead of `transferOwnership` function |       2       |

# Non Critical Issues

1.  ## Not using the latest version of OpenZeppelin from dependencies

    Use the latest version of openZeppelin

2.  ## 0 address check

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

8.  ## Stop using v != 27 && v != 28 or v == 27 || v == 28

    See this for reference : https://twitter.com/alexberegszaszi/status/1534461421454606336?s=20&t=H0Dv3ZT2bicx00hLWJk7Fg

9.  ## Missing Time locks

    Description: When critical parameters of systems need to be changed, it is required to broadcast the change via event emission and recommended to enforce the changes after a time-delay. This is to allow system users to be aware of such critical changes and give them an opportunity to exit or adjust their engagement with the system accordingly. None of the onlyOwner functions that change critical protocol addresses/parameters have a timelock for a time-delayed change to alert:

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

16. ## Testing all functions is best practice in terms of security criteria.

    Some function test coverage is not found in test files

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
