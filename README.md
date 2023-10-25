![Profil Picture](https://raw.githubusercontent.com/kayaaicom/Bitcoin-s-Security-Blockchain-Storage-and-Potential-Threat-Models/main/bitcoin-security.jpg)
This research topic will be edited and added to as it progresses. It is purely for the purpose of understanding and developing the structure of bitcoin.
Example code:
    
    import hashlib
    import time

    class Block:
        def __init__(self, index, previous_hash, timestamp, data, hash):
          self.index = index
          self.previous_hash = previous_hash
          self.timestamp = timestamp
          self.data = data
          self.hash = hash


    def calculate_hash(index, previous_hash, timestamp, data):
      return hashlib.sha256(f"{index}{previous_hash}{timestamp}{data}".encode('utf-8')).hexdigest()

    def create_genesis_block():
      # Genesis bloğunu (ilk blok) manuel olarak oluşturma
      return Block(0, "0", time.time(), "Genesis Block", calculate_hash(0, "0", time.time(), "Genesis Block"))

    def create_new_block(prev_block, data):
      index = prev_block.index + 1
      timestamp = time.time()
      hash = calculate_hash(index, prev_block.hash, timestamp, data)
      return Block(index, prev_block.hash, timestamp, data, hash)


    # Initializing blockchain with the genesis block
    blockchain = [create_genesis_block()]
    previous_block = blockchain[0]

    # Let's add 10 new blocks
    for i in range(1, 11):
      new_data = f"Block #{i} data"
      new_block = create_new_block(previous_block, new_data)
      blockchain.append(new_block)
      previous_block = new_block
      print(f"Block #{new_block.index} has been added to the blockchain!")
      print(f"Hash: {new_block.hash}\n")

This simple code shows the basics of how a blockchain works. Each block contains the hash value of the previous block, a timestamp, some data and its own hash value.
This example is not a full-featured blockchain for real-world applications, but just an example to illustrate the basic concepts.

If we think of making a malicious modification to this script (i.e. changing the data of one block and then updating the hashes of all subsequent blocks), we can perform an "attack" in this way:

    def malicious_change(blockchain, block_index, new_data):
    # Bloğun verisini değiştirme
    blockchain[block_index].data = new_data
    blockchain[block_index].hash = calculate_hash(blockchain[block_index].index, 
                                                 blockchain[block_index].previous_hash, 
                                                 blockchain[block_index].timestamp, 
                                                 new_data)
    
      # Update all blocks after the modified block
      for i in range(block_index + 1, len(blockchain)):
          blockchain[i].previous_hash = blockchain[i-1].hash
          blockchain[i].hash = calculate_hash(blockchain[i].index, 
                                            blockchain[i].previous_hash, 
                                            blockchain[i].timestamp, 
                                            blockchain[i].data)

    # Maliciously modify block 3's data
    malicious_change(blockchain, 3, "Hacked Data")

This breaks the integrity of the blockchain. To prevent this, you can add a function that checks if the hash value of each block matches the hash value of the previous block:

    def is_chain_valid(blockchain):
        for i in range(1, len(blockchain)):
            if blockchain[i].hash != calculate_hash(blockchain[i].index, 
                                                blockchain[i].previous_hash, 
                                                blockchain[i].timestamp, 
                                                blockchain[i].data):
                return False
            if blockchain[i].previous_hash != blockchain[i-1].hash:
                return False
        return True

    # Checking whether the blockchain is valid
    print(is_chain_valid(blockchain))


# Bitcoins Security Blockchain Storage and Potential Threat Models
An in-depth examination of Bitcoin's decentralized structure, how the blockchain is stored, and the potential security threats this system could potentially face.

<h2>Storing the Blockchain:</h2>

A blockchain, as the name suggests, is a chain of interconnected blocks. Each block contains a reference (hash value) to the previous block and a series of transactions.
<h2>Full Nodes:</h2>

There are many computers participating in the Bitcoin network. Some of these computers operate as "full nodes". A full node is a computer that stores a full copy of Bitcoin's blockchain and verifies all transactions on the network.

Example: Imagine that in a library, there are thousands of copies of the same book. Different people have each of these books. If someone tries to change the content of one book, the change is detected and rejected by comparing it with thousands of other copies.

Full nodes hold thousands of copies of Bitcoin's blockchain like these books. If someone tries to cheat or add false information, the other nodes check it and reject it if they find it to be false. This system keeps the Bitcoin network secure.
<h2>Decentralization:
</h2>

In traditional banking, you keep your money in a bank and that bank records all your transactions. If the bank's system crashes or cheats, your money could be at risk. But with Bitcoin, transaction records are kept on thousands of computers. This means that even if a few computers make a mistake or try to cheat, thousands of other computers will have the right information.

This is why Bitcoin is "decentralized". That is, there is no single central authority or control point. This is Bitcoin's strength because this structure makes it very difficult to cheat or manipulate the system.

Note: By "thousands of computers" we mean the specialized computers that participate in the Bitcoin network and host a copy of the blockchain. These computers are often called "nodes". Some of these nodes store an exact copy of the blockchain, and such nodes are called "full nodes".

<h2>So where is this full node hosted, that is, on which servers?</h2>

Personal Computers: Any individual can run a full node on their own computer if they have the appropriate hardware and internet connection. This is part of the decentralized nature of Bitcoin. Typically, these computers are set up to be online all the time and need to have enough storage space, as the size of the blockchain will continue to grow over time.

Data Centers: Some organizations or individuals may run full nodes in data centers where they have more powerful and continuously running machines.

On Dedicated Servers (VPS): Many people run full nodes on virtual private servers (VPS) on cloud service providers such as Amazon Web Services (AWS), DigitalOcean, Google Cloud, etc.

On Devices like Raspberry Pi: Some enthusiasts even run full nodes on mini computers like Raspberry Pi. However, storage space and other hardware requirements should be considered.

On Specialized Hardware: Some custom-built devices are optimized for use as full nodes. These devices are usually energy efficient, small and quiet.

These full nodes are distributed around the world. This means that you can find full nodes all over the world, from the Americas to Asia, Europe to Africa. This decentralized structure increases the resilience of the Bitcoin network. If all nodes in one region go offline, nodes in other regions will keep the network running.

<h2>So, if you access all these nodes, won't you get hacked?</h2>

Full nodes store a full copy of the Bitcoin blockchain and verify transactions on the network. However, hacking a full node does not directly compromise the overall security of the Bitcoin network. Here are the reasons why:

Access vs. Control: Just because someone has "access" to a full node does not mean they have control over it. Also, hacking a node does not mean manipulating the blockchain data stored on that node. And if such manipulation is attempted, other nodes will consider the change invalid.

Decentralized Structure: The Bitcoin network consists of thousands of nodes around the world. This decentralized structure helps maintain the overall security of the network even if a few nodes are attacked. For example, in a network of 10,000 nodes, hacking 100 nodes will not harm the overall integrity of the network.

Private Keys and Wallet Security: Full nodes take care of transaction verifications, so a user's private keys are not stored on these nodes. These keys control a user's access to their Bitcoins. Private keys are usually stored in wallets, and the security of these wallets is the responsibility of the user.

Backed Up Data: Since each copy of the blockchain is stored on thousands of full nodes, hacking a few or having incorrect data does not affect the overall data integrity. Because other nodes have the correct data, nodes that have misleading data or are hacked are isolated.

As a result, Bitcoin's decentralized structure and consensus mechanism provides several layers of security to protect the integrity and security of the network. However, this does not mean that it is completely safe from attacks or hacks. Instead, this structure ensures that individual events do not jeopardize the overall health and security of the network.

<h3>Extra:</h3>

Bitcoin's decentralized structure and consensus mechanism indeed make the network more resilient to attacks. However, no system is completely immune to attacks. Here are some possible risks that could threaten the Bitcoin network and users:

51% Attack: Theoretically, if a miner or group of miners owns 51% or more of the network's total hashing power, they could temporarily launch double-spending attacks. This means the capacity to spend the same Bitcoin twice. However, such an attack is both very costly and unsustainable. Furthermore, such an attack is unlikely to compromise the network in the long run.

Software Bugs: If a bug or weakness is found in the Bitcoin software, this could potentially compromise the network. However, Bitcoin software is open source and is constantly being reviewed and tested by many developers around the world.

Theft of Private Keys: Bitcoin's security is based on private keys. If a user's private key is stolen, their Bitcoins could be at risk. This is not a threat to the overall security of the network, but it is a serious risk to individual users.

Zero Day Weaknesses: Like all software, Bitcoin software is not completely immune to zero-day vulnerabilities. These are vulnerabilities that were previously unknown and have not yet been fixed.

Cybernetic Attacks: Full nodes can be vulnerable to DDoS (Distributed Denial of Service) attacks. Such attacks can temporarily prevent nodes from functioning, but do not damage the overall integrity of the network.

Global Regulations: Rather than a hack affecting the operation of Bitcoin, the introduction of strict regulations or a ban on Bitcoin in some countries could pose a threat to the Bitcoin ecosystem.

In conclusion, while the Bitcoin network is subject to various threats, it is difficult for these threats to cause serious damage to the overall integrity of the network. However, as individual users, you should be careful to keep your private keys and wallet information secure.

More coming soon...
