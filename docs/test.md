# Test Documentation

## Test Environment

- Local Anvil blockchain running on port 8546
- Foundry for contract testing
- Jest for frontend and backend testing

## Contract Deployment Tests

| Test | Status | Description |
|------|--------|-------------|
| Deploy WETH | ✅ | Successfully deployed at 0x5FbDB2315678afecb367f032d93F642f64180aa3 |
| Deploy UNI | ✅ | Successfully deployed at 0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0 |
| Deploy USDC | ✅ | Successfully deployed at 0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512 |
| Deploy USDT | ✅ | Successfully deployed at 0xDc64a140Aa3E981100a9becA4E685f962f0cF6C9 |
| Deploy WBTC | ✅ | Successfully deployed at 0xCf7Ed3AccA5a467e9e704C703E8D87F634fB0Fc9 |
| Deploy Factory | ✅ | Successfully deployed at 0x5FC8d32690cc91D4c39d9d3abcBD16989F875707 |
| Deploy Manager | ✅ | Successfully deployed at 0x0165878A594ca255338adfa4d48449f69242Eb8F |
| Deploy Quoter | ✅ | Successfully deployed at 0xa513E6E4b8f2a923D98304ec87F64353C4D5C853 |

## Pool Creation Tests

| Test | Status | Description |
|------|--------|-------------|
| Create USDT/USDC Pool | ✅ | Successfully created at 0x9c3f4FE3FC619824B75d0b82AbC58a07c996DEe3 |
| Create WBTC/USDT Pool | ✅ | Successfully created at 0x480D004850cbA0A5Aa79B63DE5E47CCd42E077d1 |
| Create WETH/UNI Pool | ✅ | Successfully created at 0x7a070b8C6F095096a23d47DbAE8eE27bD82cF146 |
| Create WETH/USDC Pool | ✅ | Successfully created at 0x0E3C89E6cbde8036d4f374862F30079383607716 |
| Create UNI/USDT Pool | ✅ | Successfully created at 0xf2F1a866E77fd7227F508A21EB5D271d1bc79e4e |

## Backend Service Tests

| Test | Status | Description |
|------|--------|-------------|
| Connect to Blockchain | ✅ | Successfully connected to Anvil blockchain on port 8546 |
| Load Initial Pools | ✅ | Successfully loaded 5 pools from the blockchain |
| Listen for Pool Events | ✅ | Successfully set up event listener for PoolCreated events |
| API Endpoint: GET /api/pools | ✅ | Successfully returns all pools |
| API Endpoint: POST /api/pools/refresh | ✅ | Successfully refreshes pool list |

## Frontend Tests

| Test | Status | Description |
|------|--------|-------------|
| Connect to Backend API | ✅ | Successfully fetches pool data from backend API |
| Local Storage Persistence | ✅ | Successfully stores and retrieves pool data from localStorage |
| Manual Pool Refresh | ✅ | Successfully refreshes pool list when button is clicked |
| MetaMask Detection | ✅ | Successfully detects when MetaMask is not installed |
| UI Rendering | ✅ | Successfully renders UI components with or without MetaMask |

## Integration Tests

| Test | Status | Description |
|------|--------|-------------|
| End-to-End Pool Creation | ✅ | Successfully creates a new pool and detects it in the backend and frontend |
| Pool List Synchronization | ✅ | Successfully synchronizes pool list between backend and frontend |
| Error Handling | ✅ | Successfully handles errors and falls back to cached data |

## Performance Tests

| Test | Status | Description |
|------|--------|-------------|
| Backend Response Time | ✅ | API endpoints respond in under 100ms |
| Frontend Load Time | ✅ | Initial page load completes in under 1s |
| Pool Refresh Time | ✅ | Pool refresh completes in under 500ms |
