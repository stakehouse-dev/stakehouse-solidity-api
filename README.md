# Stakehouse-Solidity-API
Easy plug and play Stakehouse protocol API where the protocol Solidity interfaces are already connected with the correct contract addresses for the required network.

## Installing

Simply run
```
npm i @blockswaplab/stakehouse-solidity-api
```

or
```
yarn add npm i @blockswaplab/stakehouse-solidity-api
```

And you'll get both the API and the smart contract interfaces for the Stakehouse protocol.

Solidity import path for the API as follows:
```
@blockswaplab/stakehouse-solidity-api/contracts
```

Solidity import path for the interfaces as follows:
```
@blockswaplab/stakehouse-contract-interfaces/contracts/interfaces
```

Where a full reference list of the Solidity interfaces can be found [here](https://github.com/stakehouse-dev/stakehouse-contract-interfaces).

## Example Usage

```solidity
pragma solidity ^0.8.0;

import { StakehouseAPI } from "@blockswaplab/stakehouse-solidity-api/contracts/StakehouseAPI.sol";

contract Consumer is StakehouseAPI {

    function getStakehouseForBlsPublicKey(bytes calldata _blsPublicKey) external view returns (address) {
        return getStakeHouseUniverse().memberKnotToStakeHouse(_blsPublicKey);
    }

    function registerInitials(
        address _user, bytes calldata _blsPublicKey, bytes calldata _blsSignature
    ) external {
        getTransactionRouter().registerValidatorInitials(_user, _blsPublicKey, _blsSignature);
    }
}
```

Consuming contract only needs to know what functions it wants to call and the Smart Contract API takes care of the rest (including the network selection).

**Currently Supported Networks:**

- Goerli
- Mainnet

### Testing locally

In order to test the contracts locally you will either have to have a [local fork](https://hardhat.org/hardhat-network/docs/guides/forking-other-networks) of one of the networks, or override the helper functions returning mocked smart contract values. See the example below:

```solidity
pragma solidity ^0.8.0;

import { StakehouseAPI } from "@blockswaplab/stakehouse-solidity-api/contracts/StakehouseAPI.sol";
import { IStakeHouseUniverse } from "@blockswaplab/stakehouse-contract-interfaces/contracts/interfaces/IStakeHouseUniverse.sol";

contract MockContract is StakehouseAPI {

    function getStakeHouseUniverse() internal view override virtual returns (IStakeHouseUniverse) {
        return IStakeHouseUniverse(0xEA674fdDe714fd979de3EdF0F56AA9716B898ec8);
    }
}
```
