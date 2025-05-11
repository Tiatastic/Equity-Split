# Profit Distribution Smart Contract

A Stacks blockchain smart contract for transparent and automated profit distribution among stakeholders based on ownership percentages.

## Overview

This smart contract enables organizations to manage profit distribution among stakeholders in a transparent, automated, and secure manner. It allows for registration of stakeholders with specific percentage stakes, contribution of funds, and distribution of profits according to those stakes.

## Features

- **Stakeholder Management**: Register, update, and remove stakeholders with specific ownership percentages.
- **Contribution System**: Accept STX contributions from any address during active distribution periods.
- **Automated Distribution**: Calculate and distribute profits to stakeholders based on their percentage stakes.
- **Claiming System**: Allow stakeholders to claim their profits when distributions are completed.
- **Administrative Controls**: Blacklist functionality, emergency withdrawal, and distribution lifecycle management.
- **Transparency**: All transactions and stakeholder information stored on the blockchain.

## Contract Constants

| Constant | Description |
|----------|-------------|
| `CONTRACT-OWNER` | The principal who deployed the contract and has administrative privileges |
| `ERR-OWNER-ONLY` | Error when non-owner attempts administrator functions |
| `ERR-ALREADY-INITIALIZED` | Error when trying to initialize an already initialized contract |
| `ERR-NOT-INITIALIZED` | Error when contract functions are called before initialization |
| `ERR-UNAUTHORIZED` | Error for unauthorized access attempts |
| `ERR-INVALID-PERCENTAGE` | Error when percentage is greater than 100% (10000 basis points) |
| `ERR-PERCENTAGE-SUM-EXCEEDED` | Error when total stakeholder percentages exceed 100% |
| `ERR-NO-STAKE` | Error when address with no stake tries to claim profits |
| `ERR-ZERO-AMOUNT` | Error when trying to contribute zero STX |
| `ERR-INSUFFICIENT-BALANCE` | Error when contract has insufficient balance for operation |
| `ERR-DISTRIBUTION-ACTIVE` | Error when attempting operations that require inactive distribution |
| `ERR-DISTRIBUTION-INACTIVE` | Error when attempting operations that require active distribution |
| `ERR-ALREADY-CLAIMED` | Error when stakeholder attempts to claim already claimed profits |
| `ERR-BLACKLISTED` | Error when blacklisted stakeholder tries to interact with contract |
| `ERR-NOT-FOUND` | Error when requested data is not found |

## Public Functions

### Administrative Functions

#### `initialize (minimum uint)`
Initializes the contract with a minimum stake amount. Can only be called once by the contract owner.

#### `register-stakeholder (stakeholder principal) (percentage uint)`
Registers a new stakeholder with specified percentage (in basis points, where 10000 = 100%).

#### `update-stakeholder (stakeholder principal) (percentage uint)`
Updates an existing stakeholder's percentage stake.

#### `remove-stakeholder (stakeholder principal)`
Removes a stakeholder from the contract.

#### `blacklist-stakeholder (stakeholder principal)`
Blacklists a stakeholder, preventing them from claiming profits.

#### `unblacklist-stakeholder (stakeholder principal)`
Removes a stakeholder from the blacklist.

#### `start-distribution ()`
Starts a new profit distribution period, allowing contributions.

#### `end-distribution ()`
Ends the current profit distribution period.

#### `emergency-withdraw ()`
Emergency function to withdraw all contract balance, can only be called by owner.

### Contribution Functions

#### `contribute ()`
Contributes the sender's entire STX balance to the contract.

#### `contribute-amount (amount uint)`
Contributes a specific amount of STX to the contract.

### Distribution Functions

#### `distribute-profits ()`
Distributes all contract profits to stakeholders based on their percentages.

#### `claim-profits (distribution-id uint)`
Allows a stakeholder to claim their profits from a specific distribution.

## Read-Only Functions

#### `get-stake (stakeholder principal)`
Returns the stake percentage of a stakeholder.

#### `get-stakeholder-balance (stakeholder principal)`
Returns the unclaimed balance of a stakeholder.

#### `get-distribution (id uint)`
Returns distribution information for a specific distribution ID.

#### `is-claimed (id uint) (stakeholder principal)`
Checks if a stakeholder has claimed their profits for a specific distribution.

#### `is-blacklisted (stakeholder principal)`
Checks if a stakeholder is blacklisted.

#### `is-owner ()`
Checks if the caller is the contract owner.

#### `is-initialized ()`
Checks if the contract has been initialized.

#### `is-distribution-active ()`
Checks if a distribution period is currently active.

#### `get-current-distribution-id ()`
Returns the current distribution ID.

#### `get-claimable-amount (id uint) (stakeholder principal)`
Calculates the amount claimable by a stakeholder for a specific distribution.

#### `get-contract-info ()`
Returns overall contract information for UI display.

## Usage Example

1. Deploy the contract
2. Initialize the contract with minimum stake amount
3. Register stakeholders with their percentage stakes
4. Start a distribution period
5. Contributors send STX to the contract
6. Distribute profits
7. Stakeholders claim their profits

## Technical Details

- **Percentages**: All percentages are in basis points (10000 = 100%)
- **STX Amounts**: All STX amounts are in microSTX (1,000,000 microSTX = 1 STX)
- **Distribution Lifecycle**: Distribution must be explicitly started and stopped by the owner
- **Claim System**: Stakeholders must manually claim their profits for each distribution

## Security Considerations

- Only the contract owner can perform administrative functions
- Stakeholders can only claim profits once per distribution
- Blacklisted addresses cannot claim profits
- Contract can be initialized only once
- All operations check for appropriate distribution state