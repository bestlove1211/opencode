# Bancafi Trust and Wealth Management (Skeleton)

This is a minimal upgradable, pausable foundation for bancafi trust and wealth management on chain. It captures core ideas from traditional trusts: beneficiaries with pro-rata distributions, a trusted manager, and asset custody for ERC20 tokens.

Key pieces:
- Solidity 0.8.25 with OpenZeppelin Upgradable patterns (UUPS) and Pausable features.
- Central trust contract: BancafiTrustUpgradeable.
- Init script to bootstrap a first run via a proxy.

How to run (high level):
- Install dependencies including @openzeppelin/hardhat-upgrades, @openzeppelin/contracts-upgradeable, hardhat, ethers.
- Configure Hardhat with an Ethereum network (local or testnet).
- Run `npx hardhat run --network <network> scripts/init-trust.js` to deploy the proxy and initialize.
- Interact with the deployed proxy to deposit tokens and trigger distributions.

Note: This is an initial skeleton. You’ll want to add governance, KYC/AML, asset classes beyond ERC20, robust error handling, and audit-ready tests in subsequent iterations.
