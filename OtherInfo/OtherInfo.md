# All About Other Terms

1.  # Nonce

    ***

    1. A replay attack is when a signed message is reused to claim authorization for a second action. To avoid replay attacks we use the same technique as in Ethereum transactions themselves, a so-called nonce

    ***

2.  # EOA

    Externally Owned Account, so your normal Ethereum address, not a wallet contract.

    ***

    1. In general, there are two types of accounts: externally owned accounts, controlled by private keys, and contract accounts, controlled by their contract code.

    2. When it comes to EOA, the nonce is equal to the number of transaction sent by an EOA account.

    3. And for the contract accounts, the nonce is equal to the number of contracts saved by the contract accounts.

    ***

3.  # ecrecover

    ecrecover() is a very useful Solidity function that allows the smart contract to validate that incoming data is properly signed by an expected party.

    ***

    1. In some cases ecrecover can return a random address instead of 0 for an invalid signature. This is prevented above by the owner address inside the typed data.(doing address(0) checking)
    2. Signature are malleable, meaning you might be able to create a second also valid signature for the same data. In our case we are not using the signature data
    3. An attacker can construct a hash and signature that look valid if the hash is not computed within the contract itself.

    ***

4.  # About Encoding

    - abi.encode:
      encode its parameters using the ABI specs. Parameters are padded to 32 bytes.

    - return keccak256(abi.encode(text,text));

    ***

    for input "AAA","BBB"

    000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000800000000000000000000000000000000000000000000000000000000000000005224141412200000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000052242424222000000000000000000000000000000000000000000000000000000

    ***

    - abi.encodepacked
      encode the parameters using the space required by the type. It means the encoding is done in such a way the only the required value is kept. The output is smaller one.

      - return keccak256(abi.encodePacked(text,text));

        ***

        for input "AAA","BBB"

        5224141412252242424222

        ***

    ***

    There might occur the problem of hash collistion while using the encoding.

    function collision(string memory text0, string memory text1, uint256 number) external pure returns (bytes 32){
    return keccak256(abi.encodePacked(text0, text1,number));
    }

5.  ### To prevent the collision

    we can do following things to prevent the collision

    - use abi.encode instead of abi.encodePacked
    - if there are other input to the hash function then we can rearrange inputs such that no dynamic data types are next to each other.

          function collision(string memory text0,uint256 number, string memory text1) external pure returns (bytes 32){
            return keccak256(abi.encodePacked(text0, number,text1));

      }

    ***

6.  ### Deflationary Token

    Deflationary tokens use various mechanisms to reduce their supply, with coins usually destroyed through transaction fees and coin burning.

7.  ### Upgradable Contracts

    - Upgradable contracts don't have constructor

    - we have initialize fucntion tha can be used as a constructor to initialize the variable
    - Avoidw Initial Values in Field Declarations

```diff
- contract MyContract {
-    uint256 public hasInitialValue = 42; // equivalent to setting in the constructor}

+contract MyContract is Initializable {
+   uint256 public hasInitialValue;
+  function initialize() public initializer {
+       hasInitialValue = 42; // set initial value in initializer
+   }}
```

    - It is still ok to define constant state variables, because the compiler does not reserve a storage slot for these variables, and every occurrence is replaced by the respective constant expression.

```
uint256 public constant hasInitialValue = 42;
```

    - Do not leave an implementation contract uninitialized. An uninitialized implementation contract can be taken over by an attacker, which may impact the proxy.

8. ### Msg.value

   - msg.value — The amount of wei sent with a message to a contract (wei is a denomination of ETH)

9. ### Rebase Token
   - A rebase token, also known as an elastic token, is a kind of cryptocurrency where the supply is algorithmically adjusted to control price.
