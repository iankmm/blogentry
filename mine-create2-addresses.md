Date: 11-21-25
Title: Hacking $350 dollars (legally) through CREATE2 mining
---
Earlier this month, I was randomly doomscrolling on twitter and found [this tweet](https://x.com/0xKaden/status/1986850038912598185?s=20) by Kaden. He had deployed an onchain CTF challenge loaded with 0.1 ETH and whoever managed to exploit it could keep the fund.

This immediately got me hooked and I started looking at the contract code.

Unfortunately, the source code was not provided, but the bytecode looks like the following:
```
0x5f3580610011575f545f803681845af4005b8060ff166003810415610045579060081c9080600c016008029091908091901b901c803b9091600390041415610045575f55005b5f5ffd 
```

Decompiling the code through [dedaub](https://app.dedaub.com/), we get the following code:
```js=
uint256 ___function_selector__; // STORAGE[0x0]



// Note: The function selector is not present in the original solidity code.
// However, we display it for the sake of completeness.

function __function_selector__( function_selector) public payable { 
    if (function_selector) {
        require(uint8(function_selector) / 3);
        require(uint8(function_selector) / 3 == (function_selector >> 8 << (12 + uint8(function_selector) << 3) >> (12 + uint8(function_selector) << 3)).code.size);
        ___function_selector__ = function_selector >> 8 << (12 + uint8(function_selector) << 3) >> (12 + uint8(function_selector) << 3);
        exit;
    } else {
        v0 = ___function_selector__.delegatecall(MEM[0:msg.data.length], MEM[0:0]).gas(msg.gas);
        exit;
    }
}
```
This contract is deceptively simple. There is a `delegatecall` function call and we can pass in the implementation address to execute code.

So we just create a contract that self destructs and sends the funds to our address, right? However, it turns out that the contract performs some sanity check of the implementation address.

Simply put, it checks the code size of the contract and that the implementation address contains $3size$ leading zeros. 

Last year, I participated in the [Uniswap's best salt challenge](https://blog.uniswap.org/uniswap-v4-address-mining-challenge) and learned that prefixing more than $7$ custom bytes is computationally expensive. Hence, I knew that the maximum code size of the contract must be 2 bytes. Coming up with the contract code was simple enough:
```
0x33FF
```
`0x33` loads the caller's address and `0xFF` represents the `selfdestruct` opcode. Hence, making the contract self destruct and dran its balance to the caller.  However, now I needed to make the contract address have 12 leading zeros.

From the Uniswap challenge, I already had a good tooling setup for this kind of challenge where I use  0age's [create2crunch](https://github.com/0age/create2crunch) tool, his factory address, and [vast.ai](https://vast.ai/) as a GPU provider. Hence, I immediately booted up around 15 RTX 5090 using vast.ai and continued to mine the contract address. After around an hour and 20 minutes of mining, I was able to obtain the correct salt value and deploy the contract.

Now simply calling the challenge contract's function drained the ETH to my calling address.

Amusingly, the price of 0.1 ETH was worth around 350 dollars the day I solved the challenge, but now it's worth around 270 dollars. Not sure whether to laugh or cry here considering my ETH bags. Anyways, this was a fun challenge. Big tahnks to Kaden for putting it together.