# Install hummingbot

## 1. Environment

- OS: Ubuntu 22.04
- hummingbot: v1.13.0

## 2. Install dependencies

```shell
sudo apt update -y
sudo apt install -y build-essential curl git httpie jq wget
```

## 3. Install Miniconda3

```shell
cd ${HOME}
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
chmod +x Miniconda3-latest-Linux-x86_64.sh
./Miniconda3-latest-Linux-x86_64.sh -b -u
./miniconda3/bin/conda init
# Reload .bashrc to register "conda" command
exec bash
conda install -y conda-build
```

## 4. Install client

### 4.1 Install client

```shell
cd ${HOME}
git clone https://github.com/Carry-So/hummingbot.git
cd hummingbot
git checkout xdc-release-1.13.0
./clean
./install
conda activate hummingbot
./compile
```

### 4.2 Start client

Start hummingbot by run command:

```shell
cd ${HOME}/hummingbot
bin/hummingbot.py
```

Setup your password when first login, refer to https://docs.hummingbot.org/operation/password/#creating-a-password.

### 4.3 generate certificates

Then run command `gateway generate-certs` in the input panel to generate self-signed certificates, refer to https://docs.hummingbot.org/gateway/installation/#generate-certs.

![096258ea4fc1c15ad154e99d7365ea9](https://user-images.githubusercontent.com/7695325/226868318-0188180e-0515-4a37-b919-1e14f84df693.png)

## 5. Install gateway

### 5.1 Install gateway

```shell
cd ${HOME}
git clone https://github.com/Carry-So/hummingbot-gateway.git
cd hummingbot-gateway
git checkout xdc-release-1.13.0

# Install dependencies
yarn

# Complile Typescript into Javascript
yarn build
```

### 5.2 Setup gateway

Run Gateway setup script, which helps you set configs and certificates:

```shell
cd ${HOME}/hummingbot
./gateway-setup.sh
```

Below is the interactive process:

```text
===============  SETUP GATEWAY ===============


Enter path to the Hummingbot certs folder >>> /home/liudan/hummingbot/certs

   Confirm if this is correct:

            Copy configs FROM: /home/liudan/hummingbot-gateway/src/templates
              Copy configs TO: /home/liudan/hummingbot-gateway/conf

              Copy certs FROM: /home/liudan/hummingbot/certs
                Copy certs TO: /home/liudan/hummingbot-gateway/certs

Do you want to proceed? [Y/N] >>> Y

Files successfully copied from /home/liudan/hummingbot-gateway/src/templates to /home/liudan/hummingbot-gateway/conf

Files successfully copied from /home/liudan/hummingbot/certs to /home/liudan/hummingbot-gateway/certs
```

### 5.3 Start gateway

Start gateway using passphrase:

```shell
cd ${HOME}/hummingbot-gateway
# yarn start --passphrase=daniel
yarn start --passphrase=<passphrase>
```

### 5.4 Test connection

The client should show: `Gateway: ONLINE` on the top of log pane(right) now. Run the command `gateway test-connection` in Hummingbot to test the connection. Now, you can type command `exit` in client to quit now. When you exit and restart client, it should automatically detect whether gateway is running and connect to it.

![1679479291584](https://user-images.githubusercontent.com/7695325/226868564-5a05c170-f142-4768-93f8-f7f545887213.png)
