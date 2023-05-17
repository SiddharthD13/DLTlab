# DLTlab-
Experiments of DLT Lab 


### Exp-2  Deploy a smart contract on the Ethereum blockchain using the Solidity

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract MyContract {
    uint256 public myNumber;

    string public person="Hello World !!";

    constructor(uint256 _number) {
        myNumber = _number;
    }
}
```

### Exp-3 DAPP in Solidity
``` 
sample input
name = "My Token";
  const symbol = "MTK";
  const decimals = 18;
  const initialSupply = 1000000;
```


```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract MyToken {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;
    
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    
    constructor(string memory _name, string memory _symbol, uint8 _decimals, uint256 _initialSupply) {
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        totalSupply = _initialSupply * (10 ** uint256(decimals));
        balanceOf[msg.sender] = totalSupply;
    }
    
    function transfer(address _to, uint256 _value) public returns (bool) {
        require(balanceOf[msg.sender] >= _value, "Insufficient balance");
        _transfer(msg.sender, _to, _value);
        return true;
    }
    
    function transferFrom(address _from, address _to, uint256 _value) public returns (bool) {
        require(balanceOf[_from] >= _value, "Insufficient balance");
        require(allowance[_from][msg.sender] >= _value, "Not allowed to transfer");
        _transfer(_from, _to, _value);
        _approve(_from, msg.sender, allowance[_from][msg.sender] - _value);
        return true;
    }
    
    function approve(address _spender, uint256 _value) public returns (bool) {
        _approve(msg.sender, _spender, _value);
        return true;
    }
    
    function _transfer(address _from, address _to, uint256 _value) internal {
        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;
        emit Transfer(_from, _to, _value);
    }
    
    function _approve(address _owner, address _spender, uint256 _value) internal {
        allowance[_owner][_spender] = _value;
        emit Approval(_owner, _spender, _value);
    }
}
```
### Exp-4 Basic Functions in Solidity
```
pragma solidity ^0.8.0;

contract BasicFunctions {
    uint256 public myNumber;
    string public myString;
    bool public myBoolean;
    address public myAddress;
    mapping(address => uint256) public myMapping;
    uint256[] public myArray;
    
    constructor() {
        myNumber = 0;
        myString = "";
        myBoolean = false;
        myAddress = address(0);
    }
    
    function setNumber(uint256 _number) public {
        myNumber = _number;
    }
    
    function setString(string memory _string) public {
        myString = _string;
    }
    
    function setBoolean(bool _boolean) public {
        myBoolean = _boolean;
    }
    
    function setAddress(address _address) public {
        myAddress = _address;
    }
    
    function addToMapping(address _address, uint256 _value) public {
        myMapping[_address] = _value;
    }
    
    function addToArray(uint256 _value) public {
        myArray.push(_value);
    }
}
```
### Exp-6 Inheritance 
```
pragma solidity ^0.8.0;

contract Parent {
    uint256 public parentVariable;
    
    function modifyParentVariable(uint256 _value) public {
        parentVariable = _value;
    }
}

contract Child is Parent {
    uint256 public childVariable;
    
    function modifyParentVariable(uint256 _value) public override {
        childVariable = _value;
    }
}
```
### Exp-8 Merkle Tree
```
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.0;

contract MerkleTree {
    bytes32 private root;

    constructor(bytes32[] memory leaves) {
        require(leaves.length == 6, "Invalid number of leaves");

        root = generateRoot(leaves);
    }

    function generateRoot(bytes32[] memory leaves) private pure returns (bytes32) {
        bytes32[] memory nodes = new bytes32[](12);
        for (uint256 i = 0; i < leaves.length; i++) {
            nodes[i + 6] = leaves[i];
        }

        for (uint256 i = 5; i > 0; i--) {
            nodes[i] = hash(nodes[i * 2] ^ nodes[i * 2 + 1]);
        }

        return nodes[1];
    }

    function hash(bytes32 data) private pure returns (bytes32) {
        return sha256(abi.encodePacked(data));
    }

    function getRoot() public view returns (bytes32) {
        return root;
    }
}


Input to be put next to deploy button in remix: [     "0x1111111111111111111111111111111111111111111111111111111111111111",     "0x2222222222222222222222222222222222222222222222222222222222222222",     "0x3333333333333333333333333333333333333333333333333333333333333333",     "0x4444444444444444444444444444444444444444444444444444444444444444",     "0x5555555555555555555555555555555555555555555555555555555555555555",     "0x6666666666666666666666666666666666666666666666666666666666666666"   ]
```

### Exp-9 Different Data Locations

```
// SPDX-License-Identifier: MIT
pragma solidity >=0.5.0 <0.9.0;

contract DataLocations {
    uint256 public storageVariable; // Stored in storage
    
    function setStorage(uint256 _value) public {
        storageVariable = _value;
    }
    
    function getStorage() public view returns (uint256) {
        return storageVariable;
    }
    
    function sum(uint256 a, uint256 b) public pure returns (uint256) {
        uint256 result = a + b; // Stored in memory
        return result;
    }
    
    function concatenate(string calldata _str1, string calldata _str2) public pure returns (string memory) {
        bytes memory str1Bytes = bytes(_str1); // Stored in calldata
        bytes memory str2Bytes = bytes(_str2); // Stored in calldata
        
        string memory concatenated = new string(str1Bytes.length + str2Bytes.length);
        bytes memory concatenatedBytes = bytes(concatenated);
        
        uint256 k = 0;
        for (uint256 i = 0; i < str1Bytes.length; i++) {
            concatenatedBytes[k++] = str1Bytes[i];
        }
        for (uint256 i = 0; i < str2Bytes.length; i++) {
            concatenatedBytes[k++] = str2Bytes[i];
        }
        
        return string(concatenatedBytes);
    }
}
```

### Lab-10 Supply Chain Management 
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SupplyManagementSystem {
    struct Product {
        uint256 quantity;
        bool exists;
    }
    
    mapping(uint256 => Product) public products;
    
    function addProduct(uint256 _productId, uint256 _quantity) public {
        require(!products[_productId].exists, "Product already exists");
        
        Product memory newProduct = Product({
            quantity: _quantity,
            exists: true
        });
        
        products[_productId] = newProduct;
    }
    
    function updateProductQuantity(uint256 _productId, uint256 _newQuantity) public {
        require(products[_productId].exists, "Product does not exist");
        
        products[_productId].quantity = _newQuantity;
    }
    
    function getProductQuantity(uint256 _productId) public view returns (uint256) {
        require(products[_productId].exists, "Product does not exist");
        
        return products[_productId].quantity;
    }
}
```

### Exp-1 Create a basic blockchain
```

# generates timestamps
import datetime
# contains hashing algorithms
import hashlib

# defining the 'block' data structure


class Block:
    # each block has 7 attributes

    # 1 number of the block
    blockNo = 0
    # 2 what data is stored in this block?
    data = None
    # 3 pointer to the next block
    next = None
    # 4 The hash of this block (serves as a unique ID and verifies its integrity)
    # A hash is a function that converts data into a number within a certain range.
    hash = None
    # 5 A nonce is a number only used once
    nonce = 0
    # 6 store the hash (ID) of the previous block in the chain
    previous_hash = 0x0
    # 7 timestamp
    timestamp = datetime.datetime.now()

    # We initialize a block by storing some data in it
    def __init__(self, data):
        self.data = data

    # Function to compute 'hash' of a block
    # a hash acts as both a unique identifier
    # & verifies its integrity
    # if someone changes the hash of a block
    # every block that comes after it is changed
    # this helps make a blockchain immutable
    def hash(self):
        # SHA-256 is a hashing algorithm that
        # generates an almost-unique 256-bit signature that represents
        # some piece of text
        h = hashlib.sha256()
        # the input to the SHA-256 algorithm
        # will be a concatenated string
        # consisting of 5 block attributes
        # the nonce, data, previous hash, timestamp, & block #
        h.update(
            str(self.nonce).encode('utf-8') +
            str(self.data).encode('utf-8') +
            str(self.previous_hash).encode('utf-8') +
            str(self.timestamp).encode('utf-8') +
            str(self.blockNo).encode('utf-8')
        )
        # returns a hexademical string
        return h.hexdigest()

        # SHOW DEMO 2, change data

    def __str__(self):
        # print out the value of a block
        return "Block Hash: " + str(self.hash()) + "\nBlockNo: " + str(self.blockNo) + "\nBlock Data: " + str(self.data) + "\nHashes: " + str(self.nonce) + "\n--------------"

    # defining the blockchain datastructure
# consists of 'blocks' linked together
# to form a 'chain'. Thats why its called
# 'blockchain'


class Blockchain:

    # mining difficulty
    diff = 20
    # 2^32. This is the maximum number
    # we can store in a 32-bit number
    maxNonce = 2**32
    # target hash, for mining
    target = 2 ** (256-diff)

    # generates the first block in the blockchain
    # this is called the 'genesis block'
    block = Block("Genesis")
    # sets it as the head of our blockchain
    head = block

    # adds a given block to the chain of blocks
    # the block to be added is the only parameter
    def add(self, block):

        # set the hash of a given block
        # as our new block's previous hash
        block.previous_hash = self.block.hash()
        # set the block # of our new block
        # as the given block's # + 1, since
        # its next in the chain
        block.blockNo = self.block.blockNo + 1

        # set the next block equal to
        # itself. This is the new head
        # of the blockchain
        self.block.next = block
        self.block = self.block.next

    # Determines whether or not we can add a given block to
    # the blockchain
    def mine(self, block):
        # from 0 to 2^32
        for n in range(self.maxNonce):
            # is the value of the given block's hash less than our target value?
            if int(block.hash(), 16) <= self.target:
                # if it is,
                # add the block to the chain
                self.add(block)
                print(block)
                break
            else:
                block.nonce += 1

    # Show demo 3 ! Mine a block


# initialize blockchain
blockchain = Blockchain()

# mine 10 blocks
for n in range(10):
    blockchain.mine(Block("Block " + str(n+1)))

# print out each block in the blockchain
while blockchain.head != None:
    print(blockchain.head)
    blockchain.head = blockchain.head.next
 ```
