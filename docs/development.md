# Setting up Development Environment

## Install Node.js

Install Node.js by your favorite method, or use Node Version Manager by following directions at https://github.com/creationix/nvm

```bash
nvm install v4
```

## Fork and Download Repositories

To develop SIKENcore-node:

```bash
cd ~
git clone git@github.com:<yourusername>/SIKENcore-node.git
git clone git@github.com:<yourusername>/SIKEN-lib.git
```

To develop SIKEN or to compile from source:

```bash
git clone git@github.com:<yourusername>/SIKEN.git
git fetch origin <branchname>:<branchname>
git checkout <branchname>
```
**Note**: See SIKEN documentation for building SIKEN on your platform.


## Install Development Dependencies

For Ubuntu:
```bash
sudo apt-get install libzmq3-dev
sudo apt-get install build-essential
```
**Note**: Make sure that libzmq-dev is not installed, it should be removed when installing libzmq3-dev.


For Mac OS X:
```bash
brew install zeromq
```

## Install and Symlink

```bash
cd bitcore-lib
npm install
cd ../bitcore-node
npm install
```
**Note**: If you get a message about not being able to download SIKEN distribution, you'll need to compile SIKENd from source, and setup your configuration to use that version.


We now will setup symlinks in `SIKENcore-node` *(repeat this for any other modules you're planning on developing)*:
```bash
cd node_modules
rm -rf SIKEN-lib
ln -s ~/SIKEN-lib
rm -rf SIKENd-rpc
ln -s ~/SIKENd-rpc
```

And if you're compiling or developing SIKEN:
```bash
cd ../bin
ln -sf ~/SIKEN/src/SIKENd
```

## Run Tests

If you do not already have mocha installed:
```bash
npm install mocha -g
```

To run all test suites:
```bash
cd SIKENcore-node
npm run regtest
npm run test
```

To run a specific unit test in watch mode:
```bash
mocha -w -R spec test/services/SIKENd.unit.js
```

To run a specific regtest:
```bash
mocha -R spec regtest/SIKENd.js
```

## Running a Development Node

To test running the node, you can setup a configuration that will specify development versions of all of the services:

```bash
cd ~
mkdir devnode
cd devnode
mkdir node_modules
touch SIKENcore-node.json
touch package.json
```

Edit `SIKENcore-node.json` with something similar to:
```json
{
  "network": "livenet",
  "port": 3001,
  "services": [
    "SIKENd",
    "web",
    "SIKEN-api",
    "insight-ui",
    "<additional_service>"
  ],
  "servicesConfig": {
    "SIKENd": {
      "spawn": {
        "datadir": "/home/<youruser>/.SIKEN",
        "exec": "/home/<youruser>/SIKEN/src/SIKENd"
      }
    }
  }
}
```

**Note**: To install services [SIKEN-api](https://github.com/SIKENEKIS/SIKEN-api) and [SIKEN-explorer](https://github.com/SIKENEKIS/SIKEN-explorer) you'll need to clone the repositories locally.

Setup symlinks for all of the services and dependencies:

```bash
cd node_modules
ln -s ~/SIKEN-lib
ln -s ~/SIKENcore-node
ln -s ~/SIKEN-api
ln -s ~/SIKEN-explorer
```

Make sure that the `<datadir>/SIKEN.conf` has the necessary settings, for example:
```
server=1
whitelist=127.0.0.1
txindex=1
addressindex=1
timestampindex=1
spentindex=1
zmqpubrawtx=tcp://127.0.0.1:28332
zmqpubhashblock=tcp://127.0.0.1:28332
rpcallowip=127.0.0.1
rpcuser=user
rpcpassword=password
rpcport=18332
reindex=1
gen=0
addrindex=1
logevents=1
```

From within the `devnode` directory with the configuration file, start the node:
```bash
../SIKENcore-node/bin/SIKENcore-node start
```