# Upgrade hummingbot

## 1. Upgrade gateway

```shell
cd ${HOME}/hummingbot-gateway
git fetch --all
git checkout <NEW-VERSION>
yarn
yarn build
```

## 2. Upgrade client

```shell
cd ${HOME}/hummingbot
git fetch --all
git checkout <NEW-VERSION>
conda deactivate
./clean
./install
conda activate hummingbot
./compile
```
