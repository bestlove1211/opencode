# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a multi-project workspace containing the **Bancafi DeFi Ecosystem** - a comprehensive suite of blockchain-based financial services. The workspace includes four interconnected projects plus BTTB root-level contracts:

1. **bancafi-contracts** - Core RWA tokenization with DAO governance (18 contracts)
2. **bancafi-credit** - P2P lending platform with credit scoring (13 contracts)
3. **bancafi-debt-collection** - Decentralized debt marketplace with DAO governance (7 contracts)
4. **bancafi-wallet** - Savings account wallet with automated interest (5 contracts)
5. **BTTB (root)** - Bancafi Transitional Token Bonds, trust management, and will protocols

## Workspace Structure

```
C:\Users\tvi\
├── contracts/               # Root BTTB contracts (trust, will, bonds)
├── bancafi-contracts/       # RWA tokenization + DAO + bailout system
├── bancafi-credit/          # P2P lending + credit scoring
├── bancafi-debt-collection/ # Debt marketplace + slashing
├── bancafi-wallet/          # Savings accounts + interest
├── bwmp-prod/               # Backend API for Will & Money Protocol (TypeScript)
├── trustchain/              # Experimental trust management contracts
├── hardhat.config.js        # Root-level Hardhat config (with gas reporter + contract sizer)
├── package.json             # Root-level dependencies (BTTB project)
└── node_modules/            # Shared dependencies
```

**Platform**: Windows (MSYS_NT-10.0) - Use Git Bash for best compatibility with scripts

## Essential Commands

### Bancafi Contracts (RWA Tokenization)
```bash
cd bancafi-contracts

# Compile all 18 contracts (requires viaIR: true)
npx hardhat compile

# Deploy all 15 core contracts
npx hardhat run scripts/deploy-all.js --network localhost

# Deploy 3 enhancement contracts
npx hardhat run scripts/deploy-enhancements.js --network localhost

# Run tests
npx hardhat test

# Run demo workflows
npx hardhat run scripts/institutional-demo.js --network localhost
npx hardhat run scripts/enhanced-features-demo.js --network localhost
```

### Bancafi Credit (P2P Lending)
```bash
cd bancafi-credit

# Compile contracts
npx hardhat compile

# Deploy to local network
npx hardhat node  # In separate terminal
npx hardhat run scripts/deploy.js --network localhost

# Run tests
npx hardhat test
```

### Bancafi Debt Collection
```bash
cd bancafi-debt-collection

# Compile contracts
npx hardhat compile

# Deploy contracts
npx hardhat run scripts/deploy.js --network localhost

# Run tests
npx hardhat test
```

### Bancafi Wallet (Savings)
```bash
cd bancafi-wallet

# Compile contracts
npx hardhat compile

# Deploy contract
npx hardhat run scripts/deploy.js --network sepolia

# Add supported token
npx hardhat run scripts/add-token.js --network sepolia

# Run tests
npx hardhat test

# Start backend API
cd backend
npm install
npm run dev
```

### BTTB Root Contracts (Trust & Bonds)
```bash
# Work in root directory (C:\Users\tvi\)
cd /c/Users/tvi

# Compile BTTB contracts
npx hardhat compile

# Run tests
npx hardhat test

# Clean artifacts
npx hardhat clean

# View gas report (generated after tests)
cat gas-report.txt
```

**Root contracts include:**
- BancafiTrustUpgradeable (V1-V4)
- BancafiTrustMultiAssetUpgradeable
- BancafiWillTemplateManager
- BWMPCoreUpgradeable (Bancafi Will & Money Protocol)
- BWMPChequesUpgradeable
- BancafiVoltPayUpgradeable

### BWMP Backend API
```bash
cd bwmp-prod

# Install dependencies
npm install

# Build TypeScript
npm run build

# Run in development
npm run dev

# Run tests
npm test

# Production start
npm start
```

## Critical Architecture Patterns

### Universal Patterns Across All Projects

**1. UUPS Upgradeable Proxy Pattern**
- All contracts use OpenZeppelin's UUPSUpgradeable
- Constructor must call `_disableInitializers()`
- Use `initialize()` function instead of constructor
- Deploy with `upgrades.deployProxy()`

**2. OpenZeppelin 5.0 Compatibility**
- `Pausable` and `ReentrancyGuard` are in `utils/` not `security/`
- `Counters` library deprecated → use plain `uint256`
- Use `_update` hook instead of deprecated `_beforeTokenTransfer`
- ERC20Votes requires `__EIP712_init(name, "1")`

**3. Hardhat Configuration Requirements**
```javascript
solidity: {
  version: "0.8.25",
  settings: {
    optimizer: { enabled: true, runs: 200 },
    viaIR: true,  // CRITICAL for complex contracts
    evmVersion: "cancun"
  }
}
```

**4. Gas Reporting & Contract Size Monitoring**
- Root config enables `gasReporter` (outputs to gas-report.txt)
- `contractSizer` enabled with `strict: true` enforces 24KB limit
- Contracts exceeding size limit will fail compilation
- Use `viaIR: true` to optimize bytecode size for large contracts

### Project-Specific Architectures

#### Bancafi Contracts
- **18 total contracts**: 15 core + 3 enhancements
- **Key flow**: Asset Tokenization → Fractionalization → Marketplace/AMM → Rental Income
- **DAO governance** controls bailouts, upgrades, treasury
- **Interdependencies**: See `bancafi-contracts/CLAUDE.md` for detailed deployment order
- **Key constraint**: Must deploy in specific order (token → timelock → DAO → contracts)

#### Bancafi Credit
- **13 contracts** for institutional and retail lending
- **Credit scoring system** with on-chain reputation
- **Multi-asset collateral** support (crypto + RWA)
- **Key flow**: KYC/Compliance → Credit Line → Collateral Lock → Borrow → Repay
- **Institutional features**: Private credit lines, portfolio management

#### Bancafi Debt Collection
- **7 contracts** with DAO governance
- **Dual debt support**: Physical (invoices, loans) + Digital (crypto loans)
- **Slashing system**: Automated collection with penalties
- **Key flow**: Tokenize Debt → List on Marketplace → Investor Buys → Collection/Repayment
- **Reputation system**: 0-1000 credit scores with rehabilitation

#### Bancafi Wallet
- **5 contracts** for savings accounts
- **Interest system**: 4.5% base APY + 2.5% loyalty bonus after 90 days
- **No lock-up**: Instant withdrawals without penalties
- **Backend integration**: REST API with PostgreSQL + Redis
- **Multi-token support**: USDC, USDT, DAI, other ERC20s

#### BTTB (Root Contracts)
- **Trust management**: Multi-version upgradeable trust contracts with beneficiary distributions
- **Will protocols**: Template-based will management with executor controls
- **Money protocols**: BWMP for digital cheques and payment authorizations
- **Volt Pay**: Payment processing with compliance features
- **Key pattern**: Most contracts use UUPS upgradeability with versioned implementations (V1-V4)

## Common Development Workflows

### Starting Work on a Specific Project

1. **Navigate to project directory**
```bash
cd bancafi-{contracts|credit|debt-collection|wallet}
```

2. **Check project-specific documentation**
- Each project has its own README.md
- bancafi-contracts has comprehensive CLAUDE.md
- Check ARCHITECTURE.md where available

3. **Install dependencies if needed**
```bash
npm install
```

4. **Compile to verify environment**
```bash
npx hardhat compile
```

### Common Issues and Fixes

**Stack Too Deep Errors**
- Ensure `viaIR: true` in hardhat.config.js
- Most common in bancafi-contracts (BancafiInstitutional) and bancafi-credit (BancafiLending)

**BigInt Serialization**
- Always use `.toString()` when saving chainId to JSON:
```javascript
chainId: (await ethers.provider.getNetwork()).chainId.toString()
```

**Role Management**
- Check if role already granted before granting:
```javascript
const role = await contract.ROLE_NAME();
if (!await contract.hasRole(role, address)) {
    await contract.grantRole(role, address);
}
```

**Missing Initialization**
- UUPS proxies require calling `initialize()` after deployment
- Use `upgrades.deployProxy()` which calls it automatically

**Contract Size Exceeded**
- Error: "Contract code size exceeds 24576 bytes (EIP-170)"
- Solution: Enable `viaIR: true` in hardhat.config.js
- Alternative: Split contract into libraries or separate contracts
- Check `gas-report.txt` after compilation for size analysis

**Windows Path Issues**
- In bash scripts, use `/c/Users/tvi/` not `C:\Users\tvi\`
- Paths with spaces require quotes: `cd "/c/Users/tvi/my project"`
- Git operations work best in Git Bash, not PowerShell

## Project Integration Points

### Shared Dependencies
- All projects use OpenZeppelin Contracts v5.0+
- All projects use Hardhat for development
- All projects use Solidity 0.8.25

### Cross-Project Integration Opportunities
1. **Wallet + Contracts**: Use savings account funds as collateral for bailouts
2. **Credit + Contracts**: Lend against fractionalized RWA tokens
3. **Debt Collection + Credit**: Default handling for P2P loans
4. **All Projects**: Shared governance token (BGT/BAFI)

### Backend Services
- **Wallet backend**: Express.js REST API (port 3000)
- **Database**: PostgreSQL for transaction history
- **Cache**: Redis for performance
- **Web3 Provider**: Connects to blockchain networks

## Testing Strategy

### Unit Tests
- Test individual contract functions in isolation
- Mock external dependencies
- Focus on edge cases and validation

### Integration Tests
- Test cross-contract workflows
- Deploy full system to local network
- Simulate real user journeys

### Upgrade Tests
- Verify UUPS upgrades preserve state
- Test upgrade authorization
- Validate storage layout compatibility

## Key Documentation Files

### Bancafi Contracts
- `bancafi-contracts/CLAUDE.md` - Comprehensive technical guide (866 lines)
- `bancafi-contracts/DEPLOYMENT-REPORT.md` - Deployment details
- `bancafi-contracts/BLOCKER_FIXES_PROGRESS.md` - Issue tracking

### Bancafi Credit
- `bancafi-credit/ARCHITECTURE.md` - System architecture
- `bancafi-credit/DEPLOYMENT_GUIDE.md` - Deployment instructions
- `bancafi-credit/COMPLIANCE_SYSTEM.md` - KYC/AML details
- `bancafi-credit/MULTI_ASSET_COLLATERAL.md` - Collateral system
- `bancafi-credit/FLASH_LOAN_PROTECTION.md` - Security measures
- `bancafi-credit/CHAINLINK_FEEDS.md` - Oracle integration

### Bancafi Debt Collection
- `bancafi-debt-collection/README.md` - Project overview
- `bancafi-debt-collection/FINAL_PROJECT_OVERVIEW.md` - Complete system overview
- `bancafi-debt-collection/PROGRESS.md` - Development status

### Bancafi Wallet
- `bancafi-wallet/README.md` - Quick start guide
- `bancafi-wallet/QUICKSTART.md` - Setup instructions
- `bancafi-wallet/docs/TECHNICAL_DOCUMENTATION.md` - API integration
- `bancafi-wallet/docs/USER_GUIDE.md` - End-user guide

### Root-Level Documentation
- `BTTB_FINAL_DOCUMENTATION.md` - BTTB project specifications
- `BTTB_Technical_Architecture_Specifications.md` - Technical architecture
- `MCP_SETUP_GUIDE.md` - MCP server configuration
- `AFTER_RESTART_CHECKLIST.md` - Post-restart verification
- `CONTRACT_FACT_CHECK_REPORT.md` - Contract verification

## Security Considerations

All contracts implement:
- **Pausable**: Emergency stop functionality
- **ReentrancyGuard**: Protection against reentrancy attacks
- **AccessControl**: Role-based permissions
- **UUPSUpgradeable**: Controlled upgrade mechanism
- **Input validation**: Comprehensive bounds checking
- **Event emission**: Complete audit trail

### Common Security Patterns

**Reentrancy Protection**
```solidity
function withdraw() external nonReentrant {
    // Safe withdrawal logic
}
```

**Role-Based Access**
```solidity
function adminFunction() external onlyRole(ADMIN_ROLE) {
    // Protected logic
}
```

**Emergency Pause**
```solidity
function sensitiveOperation() external whenNotPaused {
    // Protected operation
}
```

## MCP Server Configuration

This workspace uses several MCP servers configured in `.claude.json`:

- **filesystem**: Local file system access (should always work)
- **github**: GitHub integration (requires token)
- **postgres**: Database access (optional)
- **brave-search**: Web search (optional)

Test MCP status with: `/mcp`

## Network Configuration

### Supported Networks
- **Ethereum Mainnet**: Production deployment
- **Sepolia Testnet**: Primary testnet
- **Polygon**: L2 deployment
- **Arbitrum**: L2 deployment
- **Optimism**: L2 deployment
- **Localhost**: Development (Hardhat node on port 8545)

### Environment Variables
Each project requires `.env` file:
```bash
PRIVATE_KEY=your_deployer_private_key
SEPOLIA_RPC_URL=https://sepolia.infura.io/v3/YOUR_KEY
ETHERSCAN_API_KEY=your_etherscan_key
```

See `.env.example` in each project directory.

## Working Across Projects

### When to Work in Each Project

**bancafi-contracts**:
- RWA tokenization features
- DAO governance
- Bailout system
- Asset fractionalization
- Rental income distribution

**bancafi-credit**:
- P2P lending features
- Credit scoring
- Collateral management
- Institutional credit lines

**bancafi-debt-collection**:
- Debt tokenization
- Debt marketplace
- Collection automation
- Slashing mechanisms

**bancafi-wallet**:
- Savings accounts
- Interest calculations
- Backend API
- User onboarding

**Root BTTB contracts**:
- Trust and estate management
- Will creation and execution
- Digital cheque issuance (BWMP)
- Bond tokenization
- Payment processing (VoltPay)

### Cross-Project Commands

```bash
# Compile all Bancafi projects
for dir in bancafi-*; do cd $dir && npx hardhat compile && cd ..; done

# Run all Bancafi tests
for dir in bancafi-*; do cd $dir && npx hardhat test && cd ..; done

# Clean all build artifacts
for dir in bancafi-*; do cd $dir && npx hardhat clean && cd ..; done

# Compile everything including root BTTB
npx hardhat compile && for dir in bancafi-*; do cd $dir && npx hardhat compile && cd ..; done
```

**Note**: When working across projects on Windows, ensure you're in Git Bash for loop commands to work correctly.

## Git Workflow

Each project is independently versioned:
```bash
cd bancafi-contracts && git status  # Check contracts repo
cd bancafi-credit && git status     # Check credit repo
cd bancafi-debt-collection && git status  # Check debt repo
cd bancafi-wallet && git status     # Check wallet repo
```

## Additional Workspace Projects

### BWMP-Prod (Backend API)
- **Type**: TypeScript/Express.js REST API
- **Purpose**: Production backend for Bancafi Will & Money Protocol
- **Stack**: Express, body-parser, CORS, dotenv
- **Location**: C:\Users\tvi\bwmp-prod\
- **Key files**: src/server.ts (entry point), tsconfig.json, package.json

### TrustChain
- **Type**: Experimental blockchain contracts
- **Purpose**: Research and development for trust management
- **Location**: C:\Users\tvi\trustchain\
- **Status**: Early stage development

### Other Projects
- **homeschool-platform**: Education platform (separate from DeFi)
- **mcp-node-server**: MCP server configuration for Claude integration
- **my-hardhat-project**: Legacy/template Hardhat setup

## Development Best Practices

1. **Always navigate to project directory first** before running commands
2. **Read project-specific CLAUDE.md** if it exists (bancafi-contracts has extensive docs)
3. **Check hardhat.config.js** for network configurations
4. **Verify .env** file is properly configured
5. **Run tests** before deploying to testnet/mainnet
6. **Document deployment addresses** in deployment JSON files
7. **Use demo scripts** to verify functionality after deployment
8. **Follow upgrade procedures** carefully for UUPS contracts
9. **Review gas-report.txt** after running tests to identify optimization opportunities
10. **On Windows**: Use Git Bash for bash scripts and loops, avoid PowerShell for Hardhat operations
