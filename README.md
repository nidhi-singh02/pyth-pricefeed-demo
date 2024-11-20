## Pyth Price Feed Data Demo

### Build

```shell
$ forge build
```

### Test

```shell
$ forge test
```

### Format

```shell
$ forge fmt
```

### Deploy
```shell
export SEPOLIA_RPC_URL=https://rpc.sepolia.org
export PYTH_ETH_SEPOLIA_ADDRESS=0xDd24F84d36BF92C65F92307595335bdFab5Bbd21
export ETH_USD_ID=0xff61491a931112ddf1bd8147cd1b641375f79f5825126d665480874634fd0ace
export PRIVATE_KEY=0x

forge create src/PriceFeed.sol:PriceFeed --private-key $PRIVATE_KEY --rpc-url $SEPOLIA_RPC_URL --constructor-args $PYTH_ETH_SEPOLIA_ADDRESS $ETH_USD_ID

# Copy the Deployed to address
export DEPLOYMENT_ADDRESS=<<Adress of the PriceFeed contract>>
```

## Intreact with smart contract

```shell
cast call $DEPLOYMENT_ADDRESS "getEthUsdPrice()" --rpc-url $SEPOLIA_RPC_URL --trace
```

## Get price update from Hermes API
```shell
curl -s "https://hermes.pyth.network/v2/updates/price/latest?&ids[]=$ETH_USD_ID" | jq -r ".binary.data[0]" > price_update.txt
```

## Call Update and Get EthUSDPrice
```shell
noglob cast send --private-key $PRIVATE_KEY --rpc-url $SEPOLIA_RPC_URL --value 0.0005ether $DEPLOYMENT_ADDRESS "UpdateAndGetEthUsdPrice(bytes[])" [0x$(cat price_update.txt)] 

cast call $DEPLOYMENT_ADDRESS "getEthUsdPrice()" --rpc-url $SEPOLIA_RPC_URL

cast --to-dec  <<Result>>
```

### Help

```shell
$ forge --help
$ cast --help
```