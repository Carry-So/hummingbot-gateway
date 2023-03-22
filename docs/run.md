# Run hummingbot

## 1. Start gateway

Run the following commands to start gateway:

```shell
cd ${HOME}/hummingbot-gateway
# yarn start --passphrase=daniel
yarn start --passphrase=<passphrase>
```

![1679479660200](https://user-images.githubusercontent.com/7695325/226870035-f060c2d7-bd1b-4d9c-9622-2344c37b48cf.png)

## 2. Start client

Run the following commands to start client:

```shell
cd ${HOME}/hummingbot
conda activate hummingbot
bin/hummingbot.py
```

## 3. Connect xsswap

Run the command `gateway connect xsswap` in client.

![1679640895282](https://user-images.githubusercontent.com/7695325/227447487-5f736dc6-85d6-41e0-807f-e644bffe1e31.png)
