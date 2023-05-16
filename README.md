# DLTlab-
Experiments of DLT Lab 


### Exp-2  Deploy a smart contract on the Ethereum blockchain using the Solidity
programming language?
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
