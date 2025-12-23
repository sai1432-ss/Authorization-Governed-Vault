**An Authorization-Governed Vault System for Controlled Asset Withdrawals**

This project implements a secure decentralized vault system where fund custody and permission validation are handled separately. The core idea is simple and powerful: funds can only be withdrawn if a valid cryptographic signature is generated off-chain by an authorized admin.

**Project Overview**

The system allows users to withdraw ETH from a vault only when they present a valid off-chain authorization. The authorization is signed by an admin and verified on-chain before funds are released.

The vault itself does not verify signatures. Instead, it delegates that responsibility to a dedicated authorization contract. This separation keeps the vault minimal, safer, and easier to audit.

**System Architecture**

The system consists of two smart contracts, each with a clearly defined role.

*SecureVault.sol*

This contract is responsible for holding ETH and executing transfers.

It does not perform signature verification.

Before releasing funds, it calls the AuthorizationManager to validate permissions.

It passes its own contract address to ensure signatures are valid only for this specific vault.

*AuthorizationManager.sol*

This contract is responsible for verifying cryptographic signatures.

It uses ECDSA to recover the signer’s address from the provided signature.

It ensures that each authorization can only be used once by tracking authorization IDs.

It enforces replay protection at the contract level.

**How to Run the Project Using Docker**

This project is fully containerized. You do not need to install Node.js, Hardhat, or any blockchain tools locally.

**Step 1 Start the System**

Run the following command:

docker-compose up --build


This will start a local Hardhat blockchain on port 8545, compile the contracts, and deploy them automatically.

You will see the deployed contract addresses and the chain ID in the logs.

**Step 2 Run the Tests**

Open a new terminal while the containers are running and execute:

docker exec -it week5bonustask-secure-vault-system-1 npx hardhat test tests/system.spec.js


The tests cover successful withdrawals using valid signatures and prevention of replay attacks using the same authorization twice.

**Project Structure**

contracts/
  SecureVault.sol
  AuthorizationManager.sol

scripts/
  deploy.js

tests/
  system.spec.js

docker/
  Dockerfile
  entrypoint.sh

docker-compose.yml
README.md

