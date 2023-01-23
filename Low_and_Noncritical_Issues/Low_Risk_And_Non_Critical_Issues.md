## Non Critical Issues

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

    1. users and give them a chance to engage/exit protocol if they are not agreeable to the changes

    2. team in case of compromised owner(s) and give them a chance to perform incident response.

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

The critical procedures should be two step process. See similar findings in previous Code4rena contests for reference: https://code4rena.com/reports/2022-06-illuminate/#2-critical-changes-should-use-two-step-procedure

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
