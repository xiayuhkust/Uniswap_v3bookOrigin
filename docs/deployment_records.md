# Deployment Records

## Local Development Environment

### Anvil Blockchain (Port 8546)

Deployment Date: February 26, 2025

| Contract | Address | Description |
|----------|---------|-------------|
| WETH | 0x5FbDB2315678afecb367f032d93F642f64180aa3 | Wrapped Ether token |
| UNI | 0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0 | Uniswap governance token |
| USDC | 0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512 | USD Coin stablecoin |
| USDT | 0xDc64a140Aa3E981100a9becA4E685f962f0cF6C9 | Tether USD stablecoin |
| WBTC | 0xCf7Ed3AccA5a467e9e704C703E8D87F634fB0Fc9 | Wrapped Bitcoin token |
| Factory | 0x5FC8d32690cc91D4c39d9d3abcBD16989F875707 | Uniswap V3 Factory contract |
| Manager | 0x0165878A594ca255338adfa4d48449f69242Eb8F | Uniswap V3 Manager contract |
| Quoter | 0xa513E6E4b8f2a923D98304ec87F64353C4D5C853 | Uniswap V3 Quoter contract |

### Pools

| Pool | Address | Fee |
|------|---------|-----|
| USDT/USDC | 0x9c3f4FE3FC619824B75d0b82AbC58a07c996DEe3 | 0.3% |
| WBTC/USDT | 0x480D004850cbA0A5Aa79B63DE5E47CCd42E077d1 | 0.3% |
| WETH/UNI | 0x7a070b8C6F095096a23d47DbAE8eE27bD82cF146 | 0.3% |
| WETH/USDC | 0x0E3C89E6cbde8036d4f374862F30079383607716 | 0.3% |
| UNI/USDT | 0xf2F1a866E77fd7227F508A21EB5D271d1bc79e4e | 0.05% |

## Backend Service

The backend service is configured to connect to the Anvil blockchain on port 8546 and listen for pool events from the Factory contract.

```
RPC_URL=http://localhost:8546
FACTORY_ADDRESS=0x5FC8d32690cc91D4c39d9d3abcBD16989F875707
PORT=3001
```

## Frontend Configuration

The frontend is configured to connect to the Anvil blockchain on port 8546 and use the deployed contract addresses.

```javascript
const config = {
  wethAddress: '0x5FbDB2315678afecb367f032d93F642f64180aa3',
  factoryAddress: '0x5FC8d32690cc91D4c39d9d3abcBD16989F875707',
  managerAddress: '0x0165878A594ca255338adfa4d48449f69242Eb8F',
  quoterAddress: '0xa513E6E4b8f2a923D98304ec87F64353C4D5C853',
  // ... token configuration ...
};
```
