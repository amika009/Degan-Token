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