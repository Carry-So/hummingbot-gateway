# Test globiance exchange

Test hummingbot API with [globiance exchange](https://app.xspswap.finance/pool)

**Notice: The globiance exchange is not ready on testnet(apothem) now.**

According to https://raw.githubusercontent.com/pro100skm/xdc-token-list/master/mainnet.tokenlist.json, 0x951857744785E80e2De051c32EE7b25f9c458C42 is WXDC, 0x8A3cc832Bb6B255622E92dc9d4611F2A94d200DA is GBEX-WXDC.

## Preparations

### 1. Setup enviroment

Setup enviroment according to [instructions](./setup-test-enviroment.md) before test.

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
    "spender": "globiance",
    "token": "GBEX"
}' | jq

curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/evm/approve" -d '{
    "chain": "xdc",
    "network": "xinfin",
    "address": "'"$XDC_ADDRESS"'",
    "spender": "globiance",
    "token": "SRX"
}' | jq
```

#### 1.2 If use httpie

**xinfin**

```shell
# set GBEX allowance for globiance
https --print=h POST ${SERVER}/evm/approve chain=xdc network=xinfin address=${XDC_ADDRESS} spender=globiance token=GBEX

# set SRX allowance for globiance
https --print=h POST ${SERVER}/evm/approve chain=xdc network=xinfin address=${XDC_ADDRESS} spender=globiance token=SRX
```

![1666779725755](https://user-images.githubusercontent.com/7695325/198002274-fb670684-481b-4c5e-a57c-c857d9196ac4.png)

### 2. Query allowances

#### 2.1 If use curl

**xinfin**

```shell
curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/evm/allowances" -d '{
    "chain": "xdc",
    "network": "xinfin",
    "address": "'"$XDC_ADDRESS"'",
    "spender": "globiance",
    "tokenSymbols": ["GBEX", "SRX"]
}' | jq
```

#### 2.2 If use httpie

**xinfin**

```shell
https --print=b POST ${SERVER}/evm/allowances chain=xdc network=xinfin address=${XDC_ADDRESS} spender=globiance tokenSymbols:='["GBEX", "SRX"]'
```

![1666779804001](https://user-images.githubusercontent.com/7695325/198002507-528d438f-3450-4ee2-abc3-be41d39fb280.png)

### 3. fetch token price

#### 3.1 If use curl

**xinfin**

```shell
# buy SRX, quote with GBEX
curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/amm/price" -d '{
    "connector": "globiance",
    "chain": "xdc",
    "network": "xinfin",
    "base": "SRX",
    "quote": "GBEX",
    "amount": "1",
    "side": "BUY"
}' | jq

# sell SRX, quote with GBEX
curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/amm/price" -d '{
    "connector": "globiance",
    "chain": "xdc",
    "network": "xinfin",
    "base": "SRX",
    "quote": "GBEX",
    "amount": "1",
    "side": "SELL"
}' | jq
```

#### 3.2 If use httpie

**xinfin**

```shell
# buy SRX, quote with GBEX
https --print=b POST ${SERVER}/amm/price connector=globiance chain=xdc network=xinfin quote=GBEX amount=1 base=SRX side=BUY

# sell SRX, quote with GBEX
https --print=b POST ${SERVER}/amm/price connector=globiance chain=xdc network=xinfin quote=GBEX amount=1 base=SRX side=SELL
```

![1666779895385](https://user-images.githubusercontent.com/7695325/198002823-963eb1ef-183e-4e81-a233-a14145678575.png)

### 4. Trade

We will check balances of base and quote tokens before and after trade. **Notice: wait 3-4 seconds to send next request.**

#### 4.1 If use curl

**xinfin**

```shell
# buy SRX, quote with GBEX
curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/amm/trade" -d '{
    "address": "'"$XDC_ADDRESS"'",
    "base": "SRX",
    "quote": "GBEX",
    "amount": "1",
    "side": "BUY",
    "chain": "xdc",
    "network": "xinfin",
    "connector": "globiance"
}' | jq

# sell SRX, quote with GBEX
curl -s -X POST -k --key $GATEWAY_KEY --cert $GATEWAY_CERT -H "${HEADER}" "${SERVER}/amm/trade" -d '{
    "address": "'"$XDC_ADDRESS"'",
    "base": "SRX",
    "quote": "GBEX",
    "amount": "1",
    "side": "SELL",
    "chain": "xdc",
    "network": "xinfin",
    "connector": "globiance"
}' | jq
```

#### 4.2 If use httpie

**xinfin**

```shell
# query GBEX and SRX balances before buy
https --print=b POST ${SERVER}/network/balances chain=xdc network=xinfin address=${XDC_ADDRESS} tokenSymbols:='["GBEX", "SRX"]'

# buy SRX, quote with GBEX
https --print=h POST ${SERVER}/amm/trade connector=globiance chain=xdc network=xinfin address=${XDC_ADDRESS} quote=GBEX amount=1 base=SRX side=BUY

# query GBEX and SRX balances after buy
https --print=b POST ${SERVER}/network/balances chain=xdc network=xinfin address=${XDC_ADDRESS} tokenSymbols:='["GBEX", "SRX"]'
```

![1666780493107](https://user-images.githubusercontent.com/7695325/198004848-25381332-e345-4275-bd5b-556dab7c16d5.png)

```shell
# query GBEX and SRX balances before sell
https --print=b POST ${SERVER}/network/balances chain=xdc network=xinfin address=${XDC_ADDRESS} tokenSymbols:='["GBEX", "SRX"]'

# sell SRX, quote with GBEX
https --print=h POST ${SERVER}/amm/trade connector=globiance chain=xdc network=xinfin address=${XDC_ADDRESS} quote=GBEX amount=1 base=SRX side=SELL

# query GBEX and SRX balances after sell
https --print=b POST ${SERVER}/network/balances chain=xdc network=xinfin address=${XDC_ADDRESS} tokenSymbols:='["GBEX", "SRX"]'
```

![1666780585280](https://user-images.githubusercontent.com/7695325/198005148-a03db32b-686d-4efa-ad85-3217ebac1271.png)
