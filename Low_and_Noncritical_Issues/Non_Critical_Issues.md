## % Non Critical Issues

1. ## Not using the latest version of OpenZeppelin from dependencies

   Use the latest version of openZeppelin

2. ## 0 address check
   Do zero address check
   function setOracle(address \_oracle)
   external onlyOwner
   {
   require(\_oracle != address(0), "Address cannot be zero");
   oracle = \_oracle;
   emit NewOracle(oracle);
   }
3. ## Omissions in Events

   Events are generally emitted when sensitive changes are made to the contracts.
   The events should include the new value and old value where possible.

   emit NewEventExecution(newValue,oldValue);

4. ## Add parameter to Event-Emit

   Some event-emit description hasn’t parameter. Add to parameter for front-end website or client app , they can has that something has happened on the blockchain.
   Events with no old value;

   emit Opened();
   emit Closed();

5.
