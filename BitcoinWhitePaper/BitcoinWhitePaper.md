# Bitcoin Whitepaper Points

---

Abstract. A purely peer-to-peer version of electronic cash would allow online
payments to be sent directly from one party to another without going through a
financial institution. Digital signatures provide part of the solution, but the main
benefits are lost if a trusted third party is still required to prevent double-spending.
We propose a solution to the double-spending problem using a peer-to-peer network.
The network timestamps transactions by hashing them into an ongoing chain of
hash-based proof-of-work, forming a record that cannot be changed without redoing
the proof-of-work. The longest chain not only serves as proof of the sequence of
events witnessed, but proof that it came from the largest pool of CPU power. As
long as a majority of CPU power is controlled by nodes that are not cooperating to
attack the network, they'll generate the longest chain and outpace attackers. The
network itself requires minimal structure. Messages are broadcast on a best effort
basis, and nodes can leave and rejoin the network at will, accepting the longest
proof-of-work chain as proof of what happened while they were gone.

---

1. ### Introduction

   - What is needed is an electronic payment system based on cryptographic proof instead of trust,allowing any two willing parties to transact directly with each other without the need for a trusted third party.

   - Transactions that are computationally impractical to reverse would protect sellers from fraud, and routine escrow mechanisms could easily be implemented to protect buyers.

2. ### Transactions

   - We define an electronic coin as a chain of digital signatures. Each owner transfers the coin to the next by digitally signing a hash of the previous transaction and the public key of the next owner and adding these to the end of the coin. A payee can verify the signatures to verify the chain of ownership.

   - The problem of course is the payee can't verify that one of the owners did not double-spend the coin.

   - To accomplish this without a trusted party, transactions must be publicly announced, and we need a system for participants to agree on a single history of the order in which they were received. The payee needs proof that at the time of each transaction, the majority of nodes agreed it was the first received.

3. ### Timestamp Server

   - The solution we propose begins with a timestamp server. A timestamp server works by taking a hash of a block of items to be timestamped and widely publishing the hash, such as in a newspaper or Usenet post
   - The timestamp proves that the data must have existed at the time, obviously, in order to get into the hash

4. ### Proof-of-Work

   - To implement a distributed timestamp server on a peer-to-peer basis, we will need to use a proofof-work system

   - For our timestamp network, we implement the proof-of-work by incrementing a nonce in the block until a value is found that gives the block's hash the required zero bits. Once the CPU effort has been expended to make it satisfy the proof-of-work, the block cannot be changed without redoing the work. As later blocks are chained after it, the work to change the block would include redoing all the blocks after it.
   - Proof-of-work is essentially one-CPU-one-vote.
   - The majority decision is represented by the longest chain, which has the greatest proof-of-work effort invested in it.
   - To modify a past block, an attacker would have to redo the proof-of-work of the block and all blocks after it and then catch up with and surpass the work of the honest nodes.

5. ### Network

The steps to run the network are as follows:

    1. New transactions are broadcast to all nodes.
    2. Each node collects new transactions into a block.
    3. Each node works on finding a difficult proof-of-work for its block.
    4. When a node finds a proof-of-work, it broadcasts the block to all nodes.
    5. Nodes accept the block only if all transactions in it are valid and not already spent.
    6. Nodes express their acceptance of the block by working on creating the next block in the chain, using the hash of the accepted block as the previous hash.

    - Nodes always consider the longest chain to be the correct one and will keep working on extending it.

    - New transaction broadcasts do not necessarily need to reach all nodes. As long as they reach many nodes, they will get into a block before long.

    - Block broadcasts are also tolerant of dropped messages. If a node does not receive a block, it will request it when it receives the next block and
    realizes it missed one.

6.  ### Incentive

    - The steady addition of a constant of amount of new coins is analogous to gold miners expending
      resources to add gold to circulation. In our case, it is CPU time and electricity that is expended.

    - The incentive can also be funded with transaction fees.

    - If the output value of a transaction is less than its input value, the difference is a transaction fee that is added to the incentive value of
      the block containing the transaction

    - Once a predetermined number of coins have entered circulation, the incentive can transition entirely to transaction fees and be completely inflation
      free.

    - The incentive may help encourage nodes to stay honest

7.  ### Reclaiming Disk Space

    - Once the latest transaction in a coin is buried under enough blocks, the spent transactions before it can be discarded to save disk space.

    - To facilitate this without breaking the block's hash, transactions are hashed in a Merkle Tree, with only the root included in the block's hash.

    - Old blocks can then be compacted by stubbing off branches of the tree. The interior hashes do not need to be stored.

8. ### Simplified Payment Verification

