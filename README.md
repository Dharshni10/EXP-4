# Experiment 4: DeFi Lending and Borrowing Protocol
## Aim:
To build a decentralized lending protocol where users can deposit assets to earn interest and borrow assets by providing collateral. This experiment introduces concepts like overcollateralization, liquidity pools, and interest accrual in DeFi.

## Algorithm:
Step 1: Setup Lending and Borrowing Mechanism Users deposit ETH into the contract as liquidity.

Depositors receive interest based on their deposits.

Borrowers can borrow ETH but must provide collateral (e.g., 150% of the borrowed amount).

Interest on borrowed funds is calculated dynamically based on utilization rate.

Step 2: Implement Overcollateralization If a borrower’s collateral value drops below a certain liquidation threshold, their collateral is liquidated to repay the debt.

Step 3: Allow Liquidation If collateral < liquidation threshold, liquidators can repay the borrower's debt and claim their collateral at a discount.

## Program:
### Name : Dharshni V M
### Register Number : 212223240029
```
//SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract DeFiLending {
    address public owner;
    uint256 public interestRate = 5; // 5% interest per cycle
    uint256 public liquidationThreshold = 150; // 150% collateralization
    mapping(address => uint256) public deposits;
    mapping(address => uint256) public borrowed;
    mapping(address => uint256) public collateral;

    event Deposited(address indexed user, uint256 amount);
    event Borrowed(address indexed user, uint256 amount, uint256 collateral);
    event Liquidated(address indexed user, uint256 debtRepaid, uint256 collateralSeized);

    constructor() {
        owner = msg.sender;
    }

    function deposit() public payable {
        require(msg.value > 0, "Deposit must be greater than zero");
        deposits[msg.sender] += msg.value;
        emit Deposited(msg.sender, msg.value);
    }

    function borrow(uint256 amount) public payable {
        require(msg.value >= (amount * liquidationThreshold) / 100, "Nota enough collateral");
        borrowed[msg.sender] += amount;
        collateral[msg.sender] += msg.value;
        payable(msg.sender).transfer(amount);
        emit Borrowed(msg.sender, amount, msg.value);
    }
    function reduceCollateral(address user, uint256 amount) public {
    require(msg.sender == owner, "Only owner can reduce");
    require(collateral[user] >= amount, "Not enough collateral to reduce");
    collateral[user] -= amount;
    }

    function liquidate(address borrower) public {
        require(collateral[borrower] < (borrowed[borrower] * liquidationThreshold) / 100, "Not eligible for liquidation");
        uint256 debt = borrowed[borrower];
        uint256 seizedCollateral = collateral[borrower];

        borrowed[borrower] = 0;
        collateral[borrower] = 0;
        payable(msg.sender).transfer(seizedCollateral);
        emit Liquidated(borrower, debt, seizedCollateral);
    }
}
```
## Expected Output:
Users can deposit ETH and earn interest.

Users can borrow ETH by providing collateral.

If collateral < 150% of borrowed amount, liquidators can seize the collateral.
## Ouput:

![Output 1](https://github.com/user-attachments/assets/a05f1ab4-b55b-4d05-9e18-a5567d4d4c55)

![Output 2](https://github.com/user-attachments/assets/7cc12720-2269-4a4a-839a-48d5218d1ab0)

![Output 3](https://github.com/user-attachments/assets/3e951852-35d1-4759-86f3-0d6796a38e2d)

![Output 4](https://github.com/user-attachments/assets/3b477e07-a1c3-49ba-8da5-761a62f131c8)

## High-Level Overview:
Teaches key DeFi concepts: lending, borrowing, collateral, liquidation.

Introduces risk management: overcollateralization and liquidation.

Directly related to DeFi protocols like Aave and Compound.

## Result:
Thus, the decentralized lending protocol where users can deposit assets to earn interest and borrow assets by providing collateral is executed succesfully.

