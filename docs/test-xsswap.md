# Test xsswap exchange

Test hummingbot gateway API with [xsswap exchange](https://app.xspswap.finance/pool)

## Preparations

### 1. Setup enviroment

Setup enviroment according to [instructions](./setup-test-enviroment.md) before test.

### 2. Trade pools

I created some trade pools on xsswap and testnet:

| Token1 | Token2 | Price(Token1/Token2) |
| -----: | -----: | -------------------: |
|  WBTC2 |  USDC2 |                20000 |
|   YFI2 |  USDC2 |                 8000 |
|   MKR2 |  USDC2 |                  600 |
|  AAVE2 |  USDC2 |                   70 |
|   UNI2 |  USDC2 |                    5 |

## Test cases

### 1. Approve allowance

Set allowances to maximum for each test token.

#### 1.1 If use curl

**xinfin**

```shell
curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/evm/approve" -d '{
    "chain": "xdc",
    "network": "xinfin",
    "address": "'"$XDC_ADDRESS"'",
    "spender": "xsswap",
    "token": "WXDC"
}' | jq

curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/evm/approve" -d '{
    "chain": "xdc",
    "network": "xinfin",
    "address": "'"$XDC_ADDRESS"'",
    "spender": "xsswap",
    "token": "XTT"
}' | jq
```

**apothem**

```shell
curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/evm/approve" -d '{
    "chain": "xdc",
    "network": "apothem",
    "address": "'"$XDC_ADDRESS"'",
    "spender": "xsswap",
    "token": "USDC2"
}' | jq

curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/evm/approve" -d '{
    "chain": "xdc",
    "network": "apothem",
    "address": "'"$XDC_ADDRESS"'",
    "spender": "xsswap",
    "token": "WBTC2"
}' | jq
```

#### 1.2 If use httpie

**xinfin**

```shell
# set WXDC allowance for xsswap
https --print=h POST ${SERVER}/evm/approve chain=xdc network=xinfin address=${XDC_ADDRESS} spender=xsswap token=WXDC

# set XTT allowance for xsswap
https --print=h POST ${SERVER}/evm/approve chain=xdc network=xinfin address=${XDC_ADDRESS} spender=xsswap token=XTT
```

![1666777201581](https://user-images.githubusercontent.com/7695325/197992826-0d4168b2-a60d-41c0-9a8f-9fe0282b9308.png)

**apothem**

```shell
# set USDC2 allowance for xsswap
https --print=h POST ${SERVER}/evm/approve chain=xdc network=apothem address=${XDC_ADDRESS} spender=xsswap token=USDC2

# set WBTC2 allowance for xsswap
https --print=h POST ${SERVER}/evm/approve chain=xdc network=apothem address=${XDC_ADDRESS} spender=xsswap token=WBTC2
```

![1666774991914](https://user-images.githubusercontent.com/7695325/197983708-1d166542-d25c-44fb-a8c0-14fc5ce3c450.png)

### 2. Query allowances

#### 2.1 If use curl

**xinfin**

```shell
curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/evm/allowances" -d '{
    "chain": "xdc",
    "network": "xinfin",
    "address": "'"$XDC_ADDRESS"'",
    "spender": "xsswap",
    "tokenSymbols": ["WXDC", "XTT"]
}' | jq
```

**apothem**

```shell
curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/evm/allowances" -d '{
    "chain": "xdc",
    "network": "apothem",
    "address": "'"$XDC_ADDRESS"'",
    "spender": "xsswap",
    "tokenSymbols": ["WBTC2", "USDC2"]
}' | jq
```

#### 2.2 If use httpie

**xinfin**

```shell
# query allowances of WXDC and XTT
https --print=b POST ${SERVER}/evm/allowances chain=xdc network=xinfin address=${XDC_ADDRESS} spender=xsswap tokenSymbols:='["WXDC", "XTT"]'
```

![1666778539223](https://user-images.githubusercontent.com/7695325/197997814-dff8a1e7-2d9a-4e9c-9a4d-431a1fb9ebad.png)

**apothem**

```shell
# query allowances of WBTC2 and USDC2
https --print=b POST ${SERVER}/evm/allowances chain=xdc network=apothem address=${XDC_ADDRESS} spender=xsswap tokenSymbols:='["WBTC2", "USDC2"]'
```

![1666775412785](https://user-images.githubusercontent.com/7695325/197985347-176d758b-ec46-47a0-8ec1-207173b038cb.png)

### 3. fetch token price

#### 3.1 If use curl

**xinfin**

```shell
# buy XTT, quote with WXDC
curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/amm/price" -d '{
    "connector": "xsswap",
    "chain": "xdc",
    "network": "xinfin",
    "base": "XTT",
    "quote": "WXDC",
    "amount": "1",
    "side": "BUY"
}' | jq

# sell XTT, quote with WXDC
curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/amm/price" -d '{
    "connector": "xsswap",
    "chain": "xdc",
    "network": "xinfin",
    "base": "XTT",
    "quote": "WXDC",
    "amount": "1",
    "side": "SELL"
}' | jq
```

**apothem**

```shell
# buy WBTC2, quote with USDC2
curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/amm/price" -d '{
    "connector": "xsswap",
    "chain": "xdc",
    "network": "apothem",
    "base": "WBTC2",
    "quote": "USDC2",
    "amount": "1",
    "side": "BUY"
}' | jq

# sell WBTC2, quote with USDC2
curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/amm/price" -d '{
    "connector": "xsswap",
    "chain": "xdc",
    "network": "apothem",
    "base": "WBTC2",
    "quote": "USDC2",
    "amount": "1",
    "side": "SELL"
}' | jq
```

#### 3.2 If use httpie

**xinfin**

```shell
# buy XTT, quote with WXDC
https --print=b POST ${SERVER}/amm/price connector=xsswap chain=xdc network=xinfin quote=WXDC amount=1 base=XTT side=BUY

# sell XTT, quote with WXDC
https --print=b POST ${SERVER}/amm/price connector=xsswap chain=xdc network=xinfin quote=WXDC amount=1 base=XTT side=SELL
```

![1666777532897](https://user-images.githubusercontent.com/7695325/197994039-94414986-4563-4768-a1c3-b33f9cef8170.png)

**apothem**

```shell
# buy WBTC2, quote with USDC2
https --print=b POST ${SERVER}/amm/price connector=xsswap chain=xdc network=apothem quote=USDC2 amount=1 base=WBTC2 side=BUY

# sell WBTC2, quote with USDC2
https --print=b POST ${SERVER}/amm/price connector=xsswap chain=xdc network=apothem quote=USDC2 amount=1 base=WBTC2 side=SELL
```

![1666600887376](https://user-images.githubusercontent.com/7695325/197485115-402b76f8-645f-4a32-b09a-9af28cb0dd43.png)

### 4. Trade

We will check balances of base and quote tokens before and after trade. **Notice: wait 3-4 seconds to send next request.**

#### 4.1 If use curl

**xinfin**

```shell
# buy XTT, quote with WXDC
curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/amm/trade" -d '{
    "address": "'"$XDC_ADDRESS"'",
    "base": "XTT",
    "quote": "WXDC",
    "amount": "1",
    "side": "BUY",
    "chain": "xdc",
    "network": "xinfin",
    "connector": "xsswap"
}' | jq

# sell XTT, quote with WXDC
curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/amm/trade" -d '{
    "address": "'"$XDC_ADDRESS"'",
    "base": "XTT",
    "quote": "WXDC",
    "amount": "1",
    "side": "SELL",
    "chain": "xdc",
    "network": "xinfin",
    "connector": "xsswap"
}' | jq
```

**apothem**

```shell
# buy WBTC2, quote with USDC2
curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/amm/trade" -d '{
    "address": "'"$XDC_ADDRESS"'",
    "base": "WBTC2",
    "quote": "USDC2",
    "amount": "1",
    "side": "BUY",
    "chain": "xdc",
    "network": "apothem",
    "connector": "xsswap"
}' | jq

# sell WBTC2, quote with USDC2
curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/amm/trade" -d '{
    "address": "'"$XDC_ADDRESS"'",
    "base": "WBTC2",
    "quote": "USDC2",
    "amount": "1",
    "side": "SELL",
    "chain": "xdc",
    "network": "apothem",
    "connector": "xsswap"
}' | jq
```

#### 4.2 If use httpie

**xinfin**

```shell
# query balances before buy
https --print=b POST ${SERVER}/network/balances chain=xdc network=xinfin address=${XDC_ADDRESS} tokenSymbols:='["WXDC", "XTT"]'

# buy XTT, quote with WXDC
https --print=h POST ${SERVER}/amm/trade connector=xsswap chain=xdc network=xinfin address=${XDC_ADDRESS} quote=WXDC amount=1 base=XTT side=BUY

# query balances after buy
https --print=b POST ${SERVER}/network/balances chain=xdc network=xinfin address=${XDC_ADDRESS} tokenSymbols:='["WXDC", "XTT"]'
```

![1666778040514](https://user-images.githubusercontent.com/7695325/197995925-0e05a3f1-828a-4cff-8b27-f63900d017e5.png)

```shell
# query WXDC and XTT balances before sell
https --print=b POST ${SERVER}/network/balances chain=xdc network=xinfin address=${XDC_ADDRESS} tokenSymbols:='["WXDC", "XTT"]'

# sell XTT, quote with WXDC
https --print=h POST ${SERVER}/amm/trade connector=xsswap chain=xdc network=xinfin address=${XDC_ADDRESS} quote=WXDC amount=1 base=XTT side=SELL

# query WXDC and XTT balances after sell
https --print=b POST ${SERVER}/network/balances chain=xdc network=xinfin address=${XDC_ADDRESS} tokenSymbols:='["WXDC", "XTT"]'
```

![1666778145685](https://user-images.githubusercontent.com/7695325/197996331-b6c780c8-00c8-41d7-9488-c9bd0bfe3052.png)

**apothem**

```shell
# query WBTC2 and USDC2 balances before buy
https --print=b POST ${SERVER}/network/balances chain=xdc network=apothem address=${XDC_ADDRESS} tokenSymbols:='["WBTC2", "USDC2"]'

# buy WBTC2, quote with USDC2
https --print=h POST ${SERVER}/amm/trade connector=xsswap chain=xdc network=apothem address=${XDC_ADDRESS} quote=USDC2 amount=1 base=WBTC2 side=BUY

# query WBTC2 and USDC2 balances after buy
https --print=b POST ${SERVER}/network/balances chain=xdc network=apothem address=${XDC_ADDRESS} tokenSymbols:='["WBTC2", "USDC2"]'
```

![1666602199336](https://user-images.githubusercontent.com/7695325/197489651-0407e3c6-4151-481e-b34a-e0dee3369f27.png)

```shell
# query WBTC2 and USDC2 balances before sell
https --print=b POST ${SERVER}/network/balances chain=xdc network=apothem address=${XDC_ADDRESS} tokenSymbols:='["WBTC2", "USDC2"]'

# sell WBTC2, quote with USDC2
https --print=h POST ${SERVER}/amm/trade connector=xsswap chain=xdc network=apothem address=${XDC_ADDRESS} quote=USDC2 amount=1 base=WBTC2 side=SELL

# query WBTC2 and USDC2 balances after sell
https --print=b POST ${SERVER}/network/balances chain=xdc network=apothem address=${XDC_ADDRESS} tokenSymbols:='["WBTC2", "USDC2"]'
```

![1666602830052](https://user-images.githubusercontent.com/7695325/197491647-c8b24ff2-e2b2-4279-b4cc-e420dd705feb.png)
