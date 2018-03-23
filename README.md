SIKENCORE Node
============

A SIKEN full node for building applications and services with Node.js. A node is extensible and can be configured to run additional services.

## Install

```bash
npm install -g SIKENcore-node
SIKENcore-node start
```

## Configuration

SIKENCORE includes a Command Line Interface (CLI) for managing, configuring and interfacing with your SIKENCORE Node.

```bash
SIKENcore-node create -d <data-dir> mynode
cd mynode
SIKENcore-node install <service>
SIKENcore-node install https://github.com/yourname/helloworld
```

This will create a directory with configuration files for your node and install the necessary dependencies. For more information about (and developing) services, please see the [Service Documentation](docs/services.md).

## Add-on Services

There are several add-on services available to extend the functionality of SIKENCORE:

- [SIKEN Insight API](https://github.com/SIKENEKIS/SIKEN-api)
- [SIKEN Explorer](https://github.com/SIKENEKIS/SIKEN-explorer)

## Contributing



## License
