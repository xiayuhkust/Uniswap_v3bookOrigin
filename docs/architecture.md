# Uniswap V3 Architecture Documentation

## Pool Management Architecture

### Factory Contract Pool Storage

The Factory contract is responsible for creating and tracking all liquidity pools in the Uniswap V3 system. It uses a triple-nested mapping to efficiently store and retrieve pool addresses:

```solidity
mapping(address => mapping(address => mapping(uint24 => address))) public pools;
```

This structure maps:
- `token0 address` → `token1 address` → `fee` → `pool address`

When a new pool is created, the Factory contract stores the pool address in this mapping and emits a `PoolCreated` event:

```solidity
event PoolCreated(
    address indexed token0,
    address indexed token1,
    uint24 indexed fee,
    int24 tickSpacing,
    address pool
);
```

### Pool Creation and Event Emission

The `createPool` function in the Factory contract creates a new pool and records it in the mapping:

```solidity
function createPool(
    address tokenA,
    address tokenB,
    uint24 fee
) external override returns (address pool) {
    // Validation and sorting of token addresses
    
    // Check if pool already exists
    require(pools[token0][token1][fee] == address(0), "Pool already exists");
    
    // Create new pool
    pool = deploy(token0, token1, fee, tickSpacing);
    
    // Store pool address in mapping (both directions)
    pools[token0][token1][fee] = pool;
    pools[token1][token0][fee] = pool;
    
    // Emit event
    emit PoolCreated(token0, token1, fee, tickSpacing, pool);
}
```

## Frontend and Backend Architecture

### Why Direct Frontend Event Listening is Not Suitable for Public Networks

In the original implementation, the frontend directly queried the blockchain for `PoolCreated` events:

```javascript
const loadPairs = () => {
  const factory = new ethers.Contract(
    config.factoryAddress,
    config.ABIs.Factory,
    new ethers.providers.Web3Provider(window.ethereum).getSigner()
  );

  return factory.queryFilter("PoolCreated", "earliest", "latest")
    .then((events) => {
      // Process events...
    });
}
```

This approach has several drawbacks for public network deployment:

1. **Resource Consumption**: Each user's browser would need to query the entire blockchain history for events, consuming significant bandwidth and processing power.

2. **Connection Stability**: Direct blockchain connections from browsers can be unstable, especially on mobile devices or with poor network conditions.

3. **Node Pressure**: If many users access the application simultaneously, it would create excessive load on the blockchain nodes.

4. **Scalability Issues**: As the blockchain grows, querying all historical events becomes increasingly inefficient.

### Improved Architecture with Backend Service

The improved architecture introduces a backend service that acts as an intermediary between the frontend and the blockchain:

1. **Backend Service**:
   - Connects to the blockchain and listens for `PoolCreated` events
   - Stores pool information in a persistent database
   - Provides a REST API for the frontend to query pool data
   - Handles event filtering and processing efficiently

2. **Frontend**:
   - Fetches pool data from the backend API instead of directly from the blockchain
   - Implements local caching using localStorage for improved performance
   - Provides a manual refresh mechanism for users to get the latest pool data
   - Falls back to cached data if the backend is temporarily unavailable

### Implementation Details

#### Backend Service

The backend service uses a `PoolService` class to handle pool event listening and storage:

```javascript
class PoolService {
  constructor(rpcUrl, factoryAddress) {
    this.provider = new ethers.providers.JsonRpcProvider(rpcUrl);
    this.factoryAddress = factoryAddress;
    this.factory = new ethers.Contract(factoryAddress, factoryABI, this.provider);
    this.pools = [];
    // Initialize and load existing pools
  }

  // Load initial pools from blockchain
  async loadInitialPools() {
    // Query for PoolCreated events
    // Process and store pool data
  }

  // Start listening for new pool events
  startListening() {
    const poolCreatedFilter = this.factory.filters.PoolCreated();
    this.factory.on(poolCreatedFilter, (token0, token1, fee, tickSpacing, pool) => {
      // Process and store new pool
    });
  }

  // Get all pools
  getAllPools() {
    return this.pools;
  }
}
```

The backend exposes REST API endpoints for the frontend to query pool data:

```javascript
app.get('/api/pools', (req, res) => {
  const pools = poolService.getAllPools();
  res.json({ success: true, data: pools });
});

app.post('/api/pools/refresh', async (req, res) => {
  try {
    await poolService.loadInitialPools();
    const pools = poolService.getAllPools();
    res.json({ success: true, data: pools });
  } catch (error) {
    res.status(500).json({ success: false, error: error.message });
  }
});
```

#### Frontend Integration

The frontend uses a custom hook `usePoolsApi` to interact with the backend:

```javascript
export function usePoolsApi() {
  const [pools, setPools] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // Function to fetch pools from the API
  const fetchPools = async () => {
    try {
      // Fetch from API
      // Update state and localStorage
    } catch (err) {
      // Handle errors and fall back to localStorage
    }
  };

  // Function to manually refresh pools
  const refreshPools = async () => {
    // Call refresh endpoint
    // Update state and localStorage
  };

  // Load pools when the component mounts
  useEffect(() => {
    // Load from localStorage first
    // Then fetch from API
    // Set up polling for updates
  }, []);

  return { pools, loading, error, refreshPools };
}
```

## Benefits of the New Architecture

1. **Reduced Resource Consumption**: The backend efficiently handles blockchain queries, reducing the load on user devices.

2. **Improved Reliability**: The frontend can still function with cached data even if the blockchain is temporarily unavailable.

3. **Better Scalability**: The backend can implement more sophisticated caching and filtering strategies as the application grows.

4. **Enhanced User Experience**: Users experience faster load times and more responsive interactions.

5. **Public Network Ready**: The architecture is suitable for deployment on public networks where direct event listening would be impractical.

## Deployment Considerations

When deploying to a public network:

1. **Backend Scaling**: The backend service should be deployed with appropriate scaling capabilities to handle user load.

2. **Caching Strategy**: Implement more sophisticated caching with TTL (Time To Live) for pool data.

3. **Error Handling**: Enhance error handling and recovery mechanisms for blockchain connection issues.

4. **Monitoring**: Add monitoring for backend service health and blockchain connectivity.

5. **Security**: Implement rate limiting and other security measures to protect the backend API.
