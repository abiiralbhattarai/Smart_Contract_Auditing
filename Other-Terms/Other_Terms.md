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

    4. # About Encoding

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

    ### To prevent the collision

    we can do following things to prevent the collision

    - use abi.encode instead of abi.encodePacked
    - if there are other input to the hash function then we can rearrange inputs such that no dynamic data types are next to each other.

          function collision(string memory text0,uint256 number, string memory text1) external pure returns (bytes 32){
            return keccak256(abi.encodePacked(text0, number,text1));

      }

    ***
