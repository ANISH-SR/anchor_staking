# Anchor Staking Program

## Overview

The Anchor Staking Program is a Solana-based decentralized staking protocol built using the Anchor framework. This program enables users to stake NFTs from verified collections and earn rewards based on configurable parameters. The protocol implements a robust staking mechanism with freeze periods, reward distribution, and comprehensive state management.

## Problem Statement

Traditional staking mechanisms often lack flexibility and transparency, particularly when dealing with NFT-based assets. Users need a secure, verifiable way to stake their NFTs while maintaining control over their assets and earning predictable rewards. Additionally, protocol administrators require granular control over staking parameters without compromising security or decentralization.

## Solution

This staking program addresses these challenges by providing:

- **NFT Collection Verification**: Ensures only NFTs from verified collections can be staked through on-chain metadata validation
- **Configurable Reward System**: Flexible points-per-stake mechanism with customizable freeze periods
- **Secure State Management**: Program-derived addresses (PDAs) ensure secure storage of configuration and user data
- **Transparent Reward Distribution**: On-chain reward minting with deterministic calculation
- **User-Centric Design**: Individual user accounts track staking history and accumulated rewards

## Technical Architecture

### Core Components

#### State Management

- **StakeConfig**: Global configuration account storing staking parameters
  - Points per stake
  - Maximum stake limit
  - Freeze period duration
  - Reward mint authority
  
- **User Accounts**: Individual accounts tracking user-specific staking data
  
- **Stake Accounts**: Records for individual staking positions

#### Instructions

1. **Initialize Config**: Sets up the global staking configuration with custom parameters
2. **Initialize User**: Creates user-specific accounts for tracking stakes and rewards
3. **Stake**: Handles NFT staking with collection verification and metadata validation

### Security Features

- Collection verification through Metaplex metadata program
- Master edition validation to prevent duplicate staking
- PDA-based authority for reward minting
- Freeze period enforcement to prevent gaming
- Associated token account validation

## Documentation

- [Architecture Diagram (PDF)](https://github.com/ANISH-SR/anchor_staking/blob/main/docs/architecture.pdf)
- [Proof of Concept (PDF)](https://github.com/ANISH-SR/anchor_staking/blob/main/docs/poc.pdf)

## Prerequisites

Before setting up the project, ensure you have the following installed:

- **Rust**: v1.70.0 or higher
- **Solana CLI**: v1.18.0 or higher
- **Anchor CLI**: v0.31.0
- **Node.js**: v18.0.0 or higher
- **Yarn**: v1.22.0 or higher

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/ANISH-SR/anchor_staking.git
cd anchor_staking
```

### 2. Install Dependencies

```bash
# Install Node.js dependencies
yarn install

# Build the Anchor program
anchor build
```

### 3. Configure Solana CLI

```bash
# Set cluster to localnet for development
solana config set --url localhost

# Create a new keypair if needed
solana-keygen new
```

### 4. Start Local Validator

```bash
# In a separate terminal, start the local Solana validator
solana-test-validator
```

### 5. Deploy the Program

```bash
# Deploy to localnet
anchor deploy

# The program ID will be displayed after deployment
# Update Anchor.toml and lib.rs with the new program ID if needed
```

## Usage

### Running Tests

Execute the test suite to verify the program functionality:

```bash
anchor test
```

For running tests with verbose output:

```bash
yarn run ts-mocha -p ./tsconfig.json -t 1000000 tests/**/*.ts
```

### Interacting with the Program

The program exposes the following instructions:

#### Initialize Configuration

```typescript
await program.methods
  .initializeConfig(pointsPerStake, maxStake, freezePeriod)
  .accounts({
    user: userPublicKey,
    mint: mintPublicKey,
    collectionMint: collectionMintPublicKey,
    // ... other accounts
  })
  .rpc();
```

#### Initialize User Account

```typescript
await program.methods
  .initializeUser()
  .accounts({
    user: userPublicKey,
    // ... other accounts
  })
  .rpc();
```

#### Stake NFT

```typescript
await program.methods
  .stake()
  .accounts({
    user: userPublicKey,
    mint: nftMintPublicKey,
    // ... other accounts
  })
  .rpc();
```

## Project Structure

```
anchor_staking/
├── programs/
│   └── staking/
│       ├── src/
│       │   ├── instructions/
│       │   │   ├── initialize_config.rs
│       │   │   ├── initialize_user.rs
│       │   │   ├── stake.rs
│       │   │   └── mod.rs
│       │   ├── state/
│       │   │   ├── stake_config.rs
│       │   │   ├── stake_account.rs
│       │   │   ├── user_account.rs
│       │   │   └── mod.rs
│       │   └── lib.rs
│       └── Cargo.toml
├── tests/
│   └── staking.ts
├── migrations/
├── Anchor.toml
├── Cargo.toml
├── package.json
└── README.md
```

## Configuration

### Anchor.toml

The `Anchor.toml` file contains the project configuration:

- **Cluster**: Set to `localnet` for development
- **Wallet**: Points to your Solana keypair
- **Program ID**: `5cEEs947E9a2TCoutXHV3ZLRt12MYJs7sx4TyrBREpgx`

### Environment Variables

For production deployment, configure the following:

```bash
export ANCHOR_PROVIDER_URL=<your-rpc-url>
export ANCHOR_WALLET=<path-to-keypair>
```

## Development

### Building the Program

```bash
anchor build
```

### Generating TypeScript Types

```bash
anchor build
# Types are automatically generated in target/types/
```

### Code Formatting

```bash
# Check formatting
yarn lint

# Fix formatting issues
yarn lint:fix
```

## Testing Strategy

The test suite covers:

- Configuration initialization with various parameters
- User account creation and management
- NFT staking with collection verification
- Reward calculation and distribution
- Edge cases and error handling

## Deployment

### Devnet Deployment

```bash
# Configure for devnet
solana config set --url devnet

# Airdrop SOL for deployment
solana airdrop 2

# Deploy
anchor deploy --provider.cluster devnet
```

### Mainnet Deployment

```bash
# Configure for mainnet
solana config set --url mainnet-beta

# Deploy with your funded keypair
anchor deploy --provider.cluster mainnet-beta
```

## Security Considerations

- All accounts use PDA derivation for deterministic addressing
- Collection verification prevents unauthorized NFT staking
- Freeze periods prevent rapid stake/unstake cycles
- Token account validation ensures proper ownership
- Metadata program integration provides cryptographic proof of collection membership

## Contributing

Contributions are welcome. Please follow these guidelines:

1. Fork the repository
2. Create a feature branch
3. Commit your changes with clear messages
4. Add tests for new functionality
5. Ensure all tests pass
6. Submit a pull request

## License

ISC

## Contact

For questions or support, please open an issue on the [GitHub repository](https://github.com/ANISH-SR/anchor_staking).

## Acknowledgments

Built with the Anchor framework and Solana blockchain technology. Special thanks to the Solana and Anchor communities for their comprehensive documentation and support.
