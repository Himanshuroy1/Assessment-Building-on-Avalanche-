Here is the updated README file based on the new Solidity code for the DegenToken contract:

---

# DegenToken

This Solidity contract represents the **DegenToken**, an ERC20 token built using OpenZeppelin's ERC20 contract. Additionally, it features an in-game store (DegenStore) where users can redeem items by paying with their DegenTokens. The store tracks item supply, and once an item is redeemed, the corresponding amount of tokens is burned from the user's account.

## Description

The **DegenToken** is an ERC20 token that provides minting, burning, and transfer functionalities. It also introduces a simple in-game store where users can redeem virtual items using their tokens. Each item in the store has a price and a limited supply. When an item is redeemed, its supply decreases, and the corresponding amount of tokens is burned.

This project demonstrates a real-world use case for ERC20 tokens beyond typical transfers by introducing an in-game economy that enables the use of tokens to redeem items from a virtual store.

## Getting Started

### Requirements

To deploy and interact with the contract, you need the following:

- **Solidity** compiler (version 0.8.26 or compatible)
- **Remix IDE** for easy development and deployment
- **Metamask** or any Ethereum wallet to deploy the contract on a testnet

### Installation

1. Visit the Remix IDE: https://remix.ethereum.org/
2. Create a new file (e.g., `DegenToken.sol`) and copy-paste the Solidity code provided below.
3. Make sure to install **OpenZeppelin** contracts by adding the following import:
   ```solidity
   import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
   ```

### Code

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract DegenToken is ERC20 {

    struct Item {
        uint id;
        string name;
        uint price;
        uint supply;
    }

    Item[100000] public collections;
    Item[100000] public DegenStore;

    constructor() ERC20("Degen", "DGN") {
        DegenStore[0] = Item(1, "Armor", 1000, 1);
        DegenStore[1] = Item(2, "Wings", 800, 500);
        DegenStore[2] = Item(3, "Cloak", 900, 250);
        DegenStore[3] = Item(4, "Coin", 100, 1000);
        DegenStore[4] = Item(5, "Coin", 100, 1000);
    }

    string public degenStoreDescription = "1. Dragon Armor, 2. Phoenix Wings, 3. Shadow Cloak, 4. Emerald Coin, 5. Diamond Coin";

    function mint(uint amount) external {
        _mint(msg.sender, amount);
    }

    function burn(uint amount) external {
        require(balanceOf(msg.sender) >= amount, "Insufficient DEGEN Tokens");
        _burn(msg.sender, amount);
    }

    function balance() external view returns(uint) {
        return balanceOf(msg.sender);
    }

    function sendToken(address recipient, uint amount) external {
        require(balanceOf(msg.sender) >= amount, "Insufficient tokens");
        transfer(recipient, amount);
    }

    function redeemItem(uint256 id) public payable {
        require(DegenStore[id - 1].supply > 0, "Item is out of stock");
        require(balanceOf(msg.sender) >= DegenStore[id - 1].price, "Not enough DGN tokens");

        DegenStore[id - 1].supply -= 1;
        _burn(msg.sender, DegenStore[id - 1].price);

        collections[id - 1] = Item(id, DegenStore[id - 1].name, DegenStore[id - 1].price, collections[id - 1].supply + 1);
    }
}
```

### Compiling the Contract

1. Navigate to the **Solidity Compiler** tab in Remix.
2. Select the Solidity version 0.8.26 (or another compatible version) from the dropdown menu.
3. Click the **Compile DegenToken.sol** button to compile the contract.

### Deploying the Contract

1. Switch to the **Deploy & Run Transactions** tab.
2. Select the `DegenToken` contract from the dropdown list.
3. Click **Deploy** to deploy the contract onto the chosen network (e.g., a testnet).
4. Once deployed, you can interact with the contract through Remix or other tools like Web3.js or Ethers.js.

### Interacting with the Contract

#### Mint Tokens
To mint tokens, call the `mint` function, providing the amount of tokens to mint.

#### Burn Tokens
To burn tokens, call the `burn` function, specifying the amount of tokens you wish to destroy.

#### Check Balance
Use the `balance` function to check your token balance.

#### Transfer Tokens
To transfer tokens to another address, use the `sendToken` function, specifying the recipient's address and the amount.

#### Redeem Items
To redeem an item from the DegenStore, call the `redeemItem` function with the item's ID. Make sure you have enough tokens to cover the item's price.

## DegenStore Items

The in-game store has five redeemable items:

1. **Dragon Armor**: 1 unit, costs 1000 DGN
2. **Phoenix Wings**: 500 units, costs 800 DGN
3. **Shadow Cloak**: 250 units, costs 900 DGN
4. **Emerald Coin**: 1000 units, costs 100 DGN
5. **Diamond Coin**: 1000 units, costs 100 DGN

## Authors

- **Himanshu Roy** 

## License

This project is licensed under the MIT License.

--- 

This README now reflects the structure and logic of the updated contract.
