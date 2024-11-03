# Contract Review

This CollateralManager is a contract designed by the Synthetix DeFi platform to manage collateralized loans and the issuance of synthetic assets ("synths"). It tracks debt, collateral types, and supports borrowing and shorting. This review would entail a line-by-line explanation to understand its components and functionality.

### Imports

#### A. Inheritances

- **Owned**: import "./Owned.sol";
- **Pausable**: import "./Pausable.sol";
- **MixinResolver**: import "./MixinResolver.sol";
- **ICollateralManager**: import "./ICollateralManager.sol";


#### B. Libraries

- **AddressSetLib**: import "./AddressSetLib.sol";
- **Bytes32SetLib**: import "./Bytes32SetLib.sol";
- **SafeDecimalMath**: import "./SafeDecimalMath.sol";


#### C. Internal References

- **CollateralManagerState**: The collateral stores the state variables of the collateral Manager. import "./CollateralManagerState.sol";
- **IIssuer**: import "./IIssuer.sol";
- **IExchangeRates**: import "./IExchangeRates.sol";
- **IERC20**: import "./IERC20.sol";
- **ISynth**: import "./ISynth.sol";

These import additional contracts that `CollateralManager` interacts with:
- `CollateralManagerState` likely stores state variables.
- `IIssuer` and `IExchangeRates` provide interfaces for working with issuance and exchange rates.
- `IERC20` and `ISynth` allow for ERC20-compliant operations and synth token management.

### Contract Declaration and Libraries
Defines `CollateralManager`, inheriting `ICollateralManager`, `Owned`, `Pausable`, and `MixinResolver`. Libraries are applied to handle safe math and sets.

### Constants

    bytes32 private constant sUSD = "sUSD";
    uint private constant SECONDS_IN_A_YEAR = 31556926 * 1e18;
    bytes32 public constant CONTRACT_NAME = "CollateralManager";
    bytes32 internal constant COLLATERAL_SYNTHS = "collateralSynth";

- `sUSD` represents the synthetic USD token.
- `SECONDS_IN_A_YEAR` helps convert annual rates to per-second rates.
- `CONTRACT_NAME` and `COLLATERAL_SYNTHS` may be used in the resolver for function access.

### State Variables
These variables store and track the contracts data like -collateral balances, borrow rates, and sets of assets managed by the contract. This contract contains 12 state variables of which 2 are mappings. All variables are outlined below and their corresponding functions.

- `CollateralManagerState public state`:  This references the  `CollateralManagerState` for debt and borrow rates.
- `AddressSetLib.AddressSet internal _collaterals`: This store sets of collateral.
- `Bytes32SetLib.Bytes32Set internal _currencyKeys`: This store sets of currencies by their keys.
- `Bytes32SetLib.Bytes32Set internal _synths`: This stores the synths
- `mapping(bytes32 => bytes32) public synthsByKey`:
- `Bytes32SetLib.Bytes32Set internal _shortableSynths`:
- `mapping(bytes32 => bytes32) public shortableSynthsByKey`:
- `uint public utilisationMultiplier = 1e18`:
- `uint public maxDebt`:
- `uint public maxSkewRate`:
- `uint public baseBorrowRate`:
- `uint public baseShortRate`:



### Constructor

The constructor initializes the contract with parameters and calls inherited constructors for ownership, pausing, and resolver setup. Initial debt, skew, and rate settings are defined here.

### Views and Helper Functions

#### `resolverAddressesRequired`

This function returns required addresses (like `Issuer` and `ExchangeRates`) from the resolver, combining static and dynamic addresses if needed.

#### `isSynthManaged`

Checks if a synth is managed by confirming it exists in `synthsByKey`.

#### `_issuer`, `_exchangeRates`, `_synth`

Internal helper functions that return instances of `Issuer`, `ExchangeRates`, and `Synth` contracts.

#### `totalLong`, `totalShort`, `totalLongAndShort`

These functions calculate the total value of long and short positions in terms of sUSD, and indicate if any exchange rate is invalid.

#### `getBorrowRate`, `getShortRate`

Calculate the current borrow and short rates based on utilization and skew, scaling these values by `utilisationMultiplier` and checking for rate validity.

#### `exceedsDebtLimit`
Verifies if a new issuance would exceed the maximum debt limit.

### Mutative Functions

#### Setters
Functions to set various limits and rates, callable only by the contract owner.

#### `addCollaterals`, `removeCollaterals`
Add or remove collateral addresses from `_collaterals`, emitting events for each change.

#### `addSynths`
Registers new synths for issuance and trading, mapping names to their keys.



