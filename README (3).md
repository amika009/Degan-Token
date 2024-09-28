# Project: Degen Token (ERC-20)

The project creates a ERC20 token and deploy it on the AVAX FUJI Chain for Degen Gaming.

## Description

The project allows only the user to mint tokens but allows other users to transfer, redeem, check balance, and lastly
burn tokens.

## Getting Started
First, open the IDE for remix, at: https://remix.ethereum.org/ Make sure to set the compiler to version 0.8.9 and above.

### Executing program

1.) Once you are in remix, click on the far left side. Under the home icon, click file explorer. 

2.) Paste the source code from DegenToken.sol 

3.) Make sure to save your work and compile, using the Press Ctrl + S command. 

4.) Once you see the check mark beside the solidity compiler, click on the deploy and run transactions.

5.) Change the environment to Injected Provider instead of Remix VM. Otherwise it will not work.

6.) Make sure that your account address has enough AVAX for it to be able to deploy the contract.

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";
import "hardhat/console.sol";

contract DegenToken is ERC20, Ownable, ERC20Burnable {
    constructor() ERC20("Degen", "DGN") {}

    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount); 
    }

    function decimals() override public pure returns (uint8) {
        return 0;
    }

    function getBalance() external view returns (uint256) {
        return this.balanceOf(msg.sender);
    }

    function transferTokens(address _receiver, uint256 _value) external {
        require(balanceOf(msg.sender) >= _value, "Not enough DGN Tokens");
        approve(msg.sender, _value);
        transferFrom(msg.sender, _receiver, _value);
    }

    function burnTokens(uint256 _value) external {
        require(balanceOf(msg.sender) >= _value, "Not enough DGN");
        burn(_value);
    }

    function redeemTokens(uint8 input) external payable returns (bool) {
        if (input == 1) {
            require(this.balanceOf(msg.sender) >= 10, "Not enough DGN Tokens");
            approve(msg.sender, 10);
            transferFrom(msg.sender, owner(), 10);
            console.log("Degen Merch with Partnership with Pomemon [Redeemed]!");
            return true;
        }
        else if (input == 2) {
            require(this.balanceOf(msg.sender) >= 5, "Not enough Degen Tokens");
            approve(msg.sender, 5);
            transferFrom(msg.sender, owner(), 5);
            console.log("Degen Merch with Collaboration with RIOT Games [Redeemed]!");
            return true;
        }
        else {
            console.log("That is not a valid choice");
            return false;
        }
    }
}
```

## Authors
Gerald Z. Gueco
