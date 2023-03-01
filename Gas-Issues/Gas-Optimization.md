# Gas optimization tricks

| Number | Issues                                                                                                                 |
| :----: | :--------------------------------------------------------------------------------------------------------------------- |
|   1.   | [++I COSTS LESS GAS THAN I++, ESPECIALLY WHEN IT’S USED IN FOR-LOOPS](#)                                               |
|   2.   | [USING PRIVATE RATHER THAN PUBLIC FOR CONSTANTS, SAVES GAS](#using-private-rather-than-public-for-constants-saves-gas) |

---

---

1.  ## ++I COSTS LESS GAS THAN I++, ESPECIALLY WHEN IT’S USED IN FOR-LOOPS

    - i++ gets compiled to something like
      j = i;
      i = i + 1;
      return j

    - while ++i gets compiled to something like
      i = i + 1;
      return i;
      Long story short, i++ returns the non-incremented value, and ++i returns the incremented value

2.  ## USING PRIVATE RATHER THAN PUBLIC FOR CONSTANTS, SAVES GAS

    If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that returns a tuple of the values of all currently-public constants. Saves 3406-3606 gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it’s used, and not adding another entry to the method ID table

3.  ## DIVISION BY TWO SHOULD USE BIT SHIFTING

    <x> / 2 is the same as <x> >> 1. While the compiler uses the SHR opcode to accomplish both, the version that uses division incurs an overhead of 20 gas due to JUMPs to and from a compiler utility function that introduces checks which can be avoided by using unchecked {} around the division by two

4.  ## STACK VARIABLE USED AS A CHEAPER CACHE FOR A STATE VARIABLE IS ONLY USED ONCE

    If the variable is only accessed once, it’s cheaper to use the state variable directly that one time, and save the 3 gas the extra stack assignment would spend

5.  ## USING CALLDATA INSTEAD OF MEMORY FOR READ-ONLY ARGUMENTS IN EXTERNAL FUNCTIONS SAVES GAS

    When a function with a memory array is called externally, the abi.decode() step has to use a for-loop to copy each index of the calldata to the memory index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 \* <mem_array>.length). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution. Note that even if an interface defines a function as having memory arguments, it’s still valid for implementation contracs to use calldata arguments instead.
    If the array is passed to an internal function which passes the array to another internal function where the array is modified and therefore memory is used in the external call, it’s still more gass-efficient to use calldata when the external function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one

    ```
    function \_unpackLegacyCommands(bytes memory executeData)
    external
    pure
    returns (
    uint256 chainId,
    bytes32[] memory commandIds,
    string[] memory commands,
    bytes[] memory params)

    ```

6.  ## USING STORAGE INSTEAD OF MEMORY FOR STRUCTS/ARRAYS SAVES GAS

    When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read. Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a memory variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another memory array/struct

    ```
    LockedBalance memory locked\_ = locked[msg.sender];

    ```

7.  ## <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES

        penaltyAccumulated += penaltyAmount;

8.  ## > 0 is more expensive than != 0 for unsigned integers

    != 0 costs 6 less GAS compared to > 0 for unsigned integers in require statements with the optimizer enabled.

9.  ## Use fixed-size bytes32 instead of string / bytes

10. ### Only update storage variables with the final results of your computation, after all intermediate calculations instead of at each step. This is because operations on storage is more expensive than operations on memory / calldata.

    ```
    pragma solidity ^0.8.0;
    contract Test {
    int internal count;
    function A() public {

    for (uint i = 0; i < 10; i++){
        count++; // Writes to storage at every intermediate step
    }

        }

    function B() public {
    uint j;
    for (uint i = 0; i < 10; i++){
    j++;
    }
    count = j; // Writes only the final result to storage
    }
    }

    ```

11. ## Use constant and immutable keywords(It is more cheaper)

    ```
    bytes32 constant MY_HASH = keccak256("abc");
    address immutable owner;

    ```

12. ## Cache storage values in memory

    Anytime you are reading from storage more than once, it is cheaper to cache variables in memory. An SLOAD cost 100 GAS while MLOAD and MSTORE only cost 3 GAS.

    SLOADs are expensive (100 gas after the 1st one) compared to MLOADs/MSTOREs (3 gas each). Storage values read multiple times should instead be cached in memory the first time (costing 1 SLOAD) and then read from this cache to avoid multiple SLOADs.

    ```
    // Before
    for(uint 1; i < approvals.length; i++); // approvals is a storage variable

    // After
    uint memory numApprovals = approvals.length;
    for(uint 1; i < numApprovals; i++);

    ```

13. ## STATE VARIABLES SHOULD BE CACHED IN STACK VARIABLES RATHER THAN RE-READING THEM FROM STORAGE

14. ## Cache external values in memory

    When making repeated external calls when the values between calls do not change, cache the values:

    ```
    // Before
    require(core.controller().hasRole(core.controller().MANAGER_ROLE(), msg.sender));

    // After
    IController memory controller = core.controller();
    require(controller.hasRole(controller.MANAGER_ROLE(), msg.sender));
    ```

15. ## Splitting require() statements that use && saves gas

    ```
    // Before
    require(result >= MIN_64x64 && result <= MAX_64x64);
    // After
    require(result >= MIN_64x64);
    require(result <= MAX_64x64);

    ```

16. ## Shorten require messages to less than 32 characters

    ```
    // Before
    require(value > 2, "This require statement message exceeds thirty two characters.");
    // After
    require(value > 2, "A201")

    ```

17. ## Use Custom Errors instead of Revert Strings to save Gas

    Starting from Solidity v0.8.4, there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., revert(“Insufficient funds.”);), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

18. ## Non-strict inequalities are cheaper than strict ones

        In the EVM, there is no opcode for non-strict inequalities (>=, <=) and two operations are performed (> + =.) Consider replacing >= with the strict counterpart >:

        ```
        // Before
        require(value >= 2);
        // After
        require(value > 3);

        ```

19. ## Use external instead of public where possible

    Functions with the public visibility modifier are costlier than external. Default to using the external modifier until you need to expose it to other functions within your contract.

20. ## Internal functions are cheaper than public
        - If a function is only callable by other functions internally, specify it as internal. Internal functions are cheaper to call than public functions.
21. ## Skip initializing default values

    When Solidity variables are not set, they refer to a set of default values.

    - address = address(0)

      Gas is spent when you unecessarily initialize variables to its default value:

    - uint256 foo = 0; // bad
    - uint256 bar; // better

22. ## USING BOOLS FOR STORAGE INCURS OVERHEAD
    Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from false to true, after having been true in the past

mapping(address => bool) private \_blocklist;
mapping(Role => bool) public isRole;

22. ## REQUIRE() OR REVERT() STATEMENTS THAT CHECK INPUT ARGUMENTS SHOULD BE AT THE TOP OF THE FUNCTION

23. ## DON’T COMPARE BOOLEAN EXPRESSIONS TO BOOLEAN LITERALS

    ```
    if (<x> == true) => if (<x>), if (<x> == false) => if (!<x>)
    require(_claim.isActive == true, "NO_ACTIVE_CLAIM");
    if (proposalHasBeenActivated[proposalId_] == true) {

    ```

24. ## FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE

    If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.
    The extra opcodes avoided are
    CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

    - function setAdmin(address admin, bool isEnabled) public onlyAdmin {}

25. ## DON’T USE `_MSGSENDER()` IF NOT SUPPORTING EIP-2771

    Use msg.sender if the code does not implement EIP-2771 trusted forwarder support.
    `_admins[_msgSender()] = true;`

26. ## REPLACE MODIFIER WITH FUNCTION

        ```
        modifier onlyByOwnGov() {
        function onlyByOwnGov() private {
        require(msg.sender == timelock*address || msg.sender == owner, "Not owner or timelock");
        _ ;

        }

        ```

27. ## STATE VARIABLES CAN BE PACKED INTO FEWER STORAGE SLOTS

    ```
    uint256(32), address(20), bool(1)
    uint48(6), uint8(1), array(32)
    =>Eg:
    i) (uint256 tokenId, uint256 highestBid, address highestBidder, uint256 startTime, uint256 endTime, bool settled) = auction.auction();
    =>Changed to:
    ii) (uint256 startTime, uint256 endTime, bool settled, address highestBidder, uint256 tokenId, uint256 highestBid) = auction.auction();
    => Eg:
    i)(, uint256 highestBid, address highestBidder, , , ) = auction.auction();
    =>(, , , address highestBidder, , uint256 highestBid) = auction.auction();
    ```

28. ## <x> = <x> + 1 even more efficient than pre increment ++i, j++(When used direct without in the for loop)

29. ## USE NAMED RETURNS FOR LOCAL VARIABLES WHERE IT IS POSSIBLE

        ```
        function getModuleAddress(Keycode keycode*) internal view returns (address) {
        address moduleForKeycode = address(kernel.getModuleForKeycode(keycode*));
        if (moduleForKeycode == address(0)) revert Policy*ModuleDoesNotExist(keycode*);
        return moduleForKeycode;
        }

        //FIX
        function getModuleAddress(Keycode keycode*) internal view returns (address moduleForKeycode) {
            moduleForKeycode = address(kernel.getModuleForKeycode(keycode*));
            if (moduleForKeycode == address(0)) revert Policy*ModuleDoesNotExist(keycode\*);
            }

        ```

30. ## DELETING A STRUCT IS CHEAPER THAN CREATING A NEW STRUCT WITH NULL VALUES

```
activeProposal = ActivatedProposal(0, 0);
delete activeProposal;
```

31. ## DUPLICATED REQUIRE()/REVERT() CHECKS SHOULD BE REFACTORED TO A MODIFIER OR FUNCTION

32. ## USE LOCAL VARIABLE INSTEAD OF THE STORAGE VARIABLE WHENEVER POSSIBLE

    ```
    Auction memory _auction = auction;
    if (auction.settled) revert AUCTION_SETTLED();(Don’t do this)
    if (_auction.settled) revert AUCTION_SETTLED();(Do this)

    ```

33. ## Use Internal View Functions in Modifiers To Save Bytecode

    When you add a function modifier, the code of that function is picked up and put in the function modifier in place of the `"_"` symbol. This can also be understood as ‘The function modifiers are inlined”. In normal programming languages, inlining small code is more efficient without any real drawback but Solidity is no ordinary language. In Solidity, the maximum size of a contract is restricted to 24 KB by EIP 170. If the same code is inlined multiple times, it adds up in size and that size limit can be hit easily.

    Internal functions, on the other hand, are not inlined but called as separate functions. This means they are very slightly more expensive in run time but save a lot of redundant bytecode in deployment. Internal functions can also help avoid the dreaded “Stack too deep Error” as variables created in an internal function don’t share the same restricted stack with the original function, but the variables created in modifiers share the same stack limit.

    Show case:

    ```diff
    - modifier onlyAuthorized() {
    -       bool isOwner = msg.sender == IOwnable(address-(securityToken)).owner();
    -    require(isOwner ||
    -        securityToken.isModule(msg.sender, DATA_KEY) ||
    -       securityToken.checkPermission(msg.sender, address(this), MANAGEDATA),
    -        "Unauthorized"
    -   );
    -    _;
    }
    + modifier onlyAuthorized() {
    +   _isAuthorized();
    +    _;
    +}
    +function _isAuthorized() internal view {/*codes*/}
    ```

34. ## Avoid emitting a storage variable when a memory value is available

    When they are the same, consider emitting the memory value instead of the storage value:
    postRevealBaseURIHash = \_postRevealBaseURIHash;
    baseURI = \_baseURI;
    emit URIUpdated(baseURI, postRevealBaseURIHash);
    emit URIUpdated(\_baseURI, \_postRevealBaseURIHash);

35. ## Unchecking arithmetics operations that can't underflow/overflow

    We never want behavior that leads to over/underflow\*. The reason the "unchecked" keyword exists is to allow Solidity developers to write more efficient programs. The default "checked" behavior costs more gas when calculating, because under-the-hood those checks are implemented as a series of opcodes that, prior to performing the actual arithmetic, check for under/overflow and revert if it is detected. So if you're a Solidity developer who needs to do some math in 0.8.0 or greater, and you can prove that there is no possible way for your arithmetic to under/overflow (maybe because you have your own "if" statement which checks that the numbers being added are never greater than, say, 100), then you can surround the arithmetic in an "unchecked" block.

```diff
    if (currentBalance >= limitPerAccount) {
    return 0;
    }

-       uint256 availableToMint = limitPerAccount - currentBalance;
    uint256 availableToMint;
    unchecked { availableToMint = limitPerAccount - currentBalance;
    }

```

36. ## Duplicated conditions should be refactored to a modifier or function to save deployment costs

    This one:

    ```
    NFTCollectionFactory.sol:203: require(\_implementation.isContract(), "NFTCollectionFactory: Implementation is not a contract");
    NFTCollectionFactory.sol:227: require(\_implementation.isContract(), "NFTCollectionFactory: Implementation is not a contract");

    ```

37. ## Increments/decrements can be unchecked in for-loops
    Consider wrapping with an unchecked block here (around 25 gas saved per instance):
    The change would be:

```diff
- for (uint256 i; i < numIterations; i++) {

+  for (uint256 i; i < numIterations;) {
  // ...
- unchecked { ++i; }
  }

```

38. # Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead
    When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

```diff
-        uint128 ink = (art == balances.art)

+        uint256 ink = (art == balances.art)

```

39. # Multiple accesses of a mapping/array should use a local variable cache

    ```
    function executeOperation(
        address[] calldata assets,
        uint256[] calldata amounts,
        uint256 amount = amounts[0];

        vaultCollateral.safeTransfer(address(mimoProxy), amounts[0]);

    ```

40. # Multiple MAPPINGS CAN BE COMBINED IN A SINGLE Struct

```diff
- mapping (address => uint256) public withdrawals;
  /// @dev maps a token address to a point in time, a hold, after which an approval can be made
- mapping (address => uint256) public approvals;

+ struct Hold {
+ uint256 withdrawals;
+ uint256 approvals;
+ }
+ mapping (address => Hold) public holds;

```

41. ### Bytes constant are cheaper than string constants

    If the string can fit into 32 bytes, then bytes32 is cheaper than string. string is a dynamically sized-type, which has current limitations in Solidity compared to a statically sized variable.

    This means extra gas spent upon deployment and every time the constant is read.

```diff

- string constant public NAME = 'Swivel Finance';
- string constant public VERSION = '3.0.0';

* bytes32 constant public NAME = 'Swivel Finance';
* bytes32 constant public VERSION = '3.0.0';
```

42. ### If a variable is set in the constructor and never modified afterwards, marking it as immutable
    If a variable is set in the constructor and never modified afterwards, marking it as immutable can save a storage slot - 20,000 gas. This also saves 97 gas on every read access of the variable.

```
- address public implementation;

+ address immutable implementation
  constructor() {
  implementation = address(new Vault());
  }

```

43. #s## It costs more gas to initialize non-constant/non-immutable variables to zero than to let the default of zero be applied

```diff
- for (uint256 i = 0; i < \_tokens.length; i++) {
+ for (uint256 i; i < \_tokens.length; ++i) {
```

44. ### Internal functions only called once can be inlined to save gas

    Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

45. ### Cache the length of arrays in loops ~6 gas per iteration

    Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack.

46. ### Use selfbalance() instead of address(this).balance

    Use assembly when getting a contract's balance of ETH.

    You can use selfbalance() instead of address(this).balance when getting your contract's balance of ETH to save gas. Additionally, you can use balance(address) instead of address.balance() when getting an external contract's balance of ETH.

47. ### Use assembly to check for address(0)

- saves 6 gas per instance

48. ### TERNARY OPERATION IS CHEAPER THAN IF-ELSE STATEMENT
    - use ternary operation whenever possible. Using ternary operation will save modest amounts of gas.

```diff
function BitMath {
         uint8 _bit,
         bool _rightSide
     ) internal pure returns (uint256) {
-        if (_rightSide) {
-            return closestBitRight(_integer, _bit - 1);
-        } else return closestBitLeft(_integer, _bit + 1);
+        return _rightSide ? closestBitRight(_integer, _bit - 1) : closestBitLeft(_integer, _bit + 1);
     }
```
