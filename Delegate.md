#Delegate

We can send a transaction with the data being the abi encoded pwn() function in order to trigger the
fallback function on Delegation.

> await contract.sendTransaction({data: web3.eth.abi.encodeFunctionSignature("pwn()") })

If we make a smart contract to trigger the fallback function for us, we mistakenly make the smart contract
the owner of the Delegation contract instead of our EOA
