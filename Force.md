#Force

One way we can force ETH into a contract without a payable function or a fallback function is to
selfdestruct a smart contract with ETH and send the ETH balance to the target contract.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract Force {

    constructor() payable {

    }

    function attack(address _to) public payable{
        selfdestruct(payable(_to));
    }
}
```

Calling the attack function will destroy the contract and send its ETH to the target

**IMPORTANT NOTE**
Don't rely on address(this).balance to check for contract logic
