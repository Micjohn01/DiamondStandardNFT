# DiamondNFT: ERC721, Merkle Distribution, and Presale

## Overview

DiamondNFT is an Ethereum-based NFT system utilizing the Diamond pattern for modular and upgradeable smart contracts. The project features ERC721 NFT functionality, a Merkle tree-based distribution system, and a presale mechanism that enables users to purchase NFTs with predefined pricing and limits.

## Key Features

Diamond Pattern: A highly modular and upgradeable contract system.

ERC721 Functionality: Full ERC721 NFT compliance via ERC721Facet.

Merkle Distribution: Gas-efficient claiming system using Merkle proofs in MerkleFacet.

Presale Mechanism: Purchase NFTs in a presale period via PresaleFacet using the formula 1 ETH = 30 NFTs with a minimum purchase of 0.01 ETH.

Project Structure
bash
Copy code
DiamondNFT/
├── src/
│   ├── Diamond.sol
│   ├── facets/
│   │   ├── ERC721Facet.sol
│   │   ├── MerkleFacet.sol
│   │   └── PresaleFacet.sol
├── test/
│   └── DiamondNFT.t.sol
├── script/
│   └── DeployDiamondNFT.s.sol
├── lib/
├── merkle/
│   ├── generateMerkleTree.ts
│   └── whitelistAddresses.json
├── foundry.toml
└── README.md

## Prerequisites

Foundry for smart contract compilation, testing, and deployment.

Node.js for Merkle tree generation.

Yarn or npm for package management.

## Setup Instructions

1. Clone the Repository
bash
Copy code
git clone https://github.com/Micjohn01/DiamondStandardNFT.git
cd DiamondStandardNFT

2. Install Dependencies
Foundry Dependencies
bash
Copy code
forge install
Node.js Dependencies
bash
Copy code
yarn install
# or
npm install

3. Generate the Merkle Tree
Generate the Merkle tree for the whitelist of addresses:

bash
Copy code
ts-node merkle/generateMerkleTree.ts
This will produce a Merkle root and proof that will be used for claiming NFTs in the MerkleFacet.

Compilation
Compile the smart contracts using Foundry:

bash
Copy code
forge build
Testing
Run the Foundry tests to verify the functionality of the DiamondNFT system:

bash
Copy code
forge test
The test suite covers the deployment of the diamond contract, presale purchases, and Merkle proof-based claims.

## Deployment
1. Set Environment Variables
Copy the example environment file and add your private key and RPC URL:

bash
Copy code
cp .env.example .env
Edit the .env file with your deployment configuration:

PRIVATE_KEY: Your Ethereum private key.
RPC_URL: The RPC URL of the network you are deploying to.
2. Deploy the DiamondNFT Contracts
Run the deployment script with Foundry:

bash
Copy code
forge script script/DeployDiamondNFT.s.sol:DeployDiamondNFT --rpc-url $RPC_URL --broadcast --verify -vvvv
This script deploys the DiamondNFT contract, including the ERC721Facet, MerkleFacet, and PresaleFacet.

## Usage

1. Presale Participation
Users can participate in the presale by sending ETH to the buyPresale() function in the PresaleFacet. The presale price is set as 1 ETH = 30 NFTs, with a minimum purchase of 0.01 ETH.

Example:

solidity
Copy code
PresaleFacet.buyPresale{value: 0.1 ether}();
This will purchase 3 NFTs for the user.

2. Claiming NFTs with Merkle Proof
Whitelisted users can claim their NFTs by providing a valid Merkle proof via the claim() function in the MerkleFacet.

To set the Merkle root:

solidity
Copy code
MerkleFacet.setMerkleRoot(_merkleRoot);
Users must provide their Merkle proof (generated from the TypeScript script) to claim their NFTs:

solidity
Copy code
MerkleFacet.claim(_merkleProof);
3. ERC721 Standard Functions
All standard ERC721 functions are available through the ERC721Facet, including:

transferFrom()
approve()
balanceOf()
ownerOf()
Upgradeability
The Diamond pattern allows easy upgradeability of the system by adding, replacing, or removing facets. This enables the contract to evolve without needing to be redeployed, keeping user data and token states intact.

To add or replace a facet, use the diamond’s upgrade functionality by providing the new facet contract address.

## Security Considerations
Access Control: Ensure only authorized addresses can set the Merkle root and modify presale parameters.
Presale Limits: The presale is capped by a limit to avoid overselling.
Merkle Distribution: Carefully manage the whitelist and ensure the Merkle root is accurate before enabling claims.
Multisig: Consider using a multisig wallet for admin operations to improve security.
Contributing
We welcome contributions to this project! If you’d like to contribute:

## Fork the repository.
Create a new branch (git checkout -b feature-branch).
Commit your changes (git commit -m 'Add new feature').
Push to the branch (git push origin feature-branch).
Create a pull request.

## License
This project is licensed under the MIT License.

## Disclaimer
This project is provided "as-is" and should be thoroughly audited before any production use. The authors take no responsibility for any potential losses or issues that may arise.