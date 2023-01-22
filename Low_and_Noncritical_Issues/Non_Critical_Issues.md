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
