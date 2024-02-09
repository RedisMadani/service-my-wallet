# Blockchain Wallet API V2

Interact programmatically with your Blockchain.info wallet through this API.

## Contents

  * [Getting Started](#getting-started)
  * [Upgrading](#upgrading)
  * [API Documentation](#api-documentation)
  * [RPC API](#rpc)
  * [Installation](#installation)
  * [Troubleshooting](#troubleshooting)
  * [Usage](#usage)
  * [Development](#development)
  * [Deployment](#deployment)

## Getting Started

To utilize this API, set up a local service to manage your Blockchain.info wallet. Your application communicates with this service via HTTP API calls.

Follow these steps:

  1. Follow the [installation instructions](#installation).
  2. Launch the server: `$ blockchain-wallet-service start --port 3000`.
  3. Consult the [documentation](#api-documentation) and begin interacting with your wallet programmatically!

Note that `blockchain-wallet-service` operates locally and only accepts connections from `localhost`. Modifying it for external connections requires appropriate firewall rules to prevent unauthorized access.

An API code is necessary for wallet creation and higher request limits. For basic usage, no API code is required. Request an API code [here](https://blockchain.info/api/api_create_code).

## Upgrading

For existing applications using [Blockchain.info's Wallet API](https://blockchain.info/api/blockchain_wallet_api), follow the Getting Started steps and replace calls to `blockchain.info/merchant/...` with `localhost:<port>/merchant/...` in your application code.

## API Documentation

Refer to the [original documentation](https://blockchain.info/api/blockchain_wallet_api).

All endpoints listed in the API documentation are supported in Blockchain Wallet API V2. Notable differences:

  * The "consolidate addresses" endpoint is omitted.
  * All endpoints support both `GET` and `POST` methods and can only be accessed from `localhost`.

### Creating a new Blockchain Wallet

Endpoint: `/api/v2/create`

Query Parameters:

  * `password` - main wallet password (required)
  * `api_code` - blockchain.info wallet API code (required)
  * `priv` - private key to import into wallet as the first address (optional)
  * `label` - label for the first address generated in the wallet (optional)
  * `email` - email associated with the newly created wallet (optional)

### Make Payment

Endpoint: `/merchant/:guid/payment`

Query Parameters:

  * `to` - bitcoin address to send to (required)
  * `amount` - amount in satoshi to send (required)
  * `password` - main wallet password (required)
  * `second_password` - second wallet password (required, if enabled)
  * `api_code` - blockchain.info wallet API code (optional)
  * `from` - bitcoin address or account index to send from (optional)
  * `fee` - specify transaction fee in satoshi
  * `fee_per_byte` - specify transaction fee-per-byte in satoshi

### Send to Many

Endpoint: `/merchant/:guid/sendmany`

Query Parameters:

  * `recipients` - URI encoded JSON object, with bitcoin addresses as keys and satoshi amounts as values (required)
  * `password` - main wallet password (required)
  * `second_password` - second wallet password (required, if enabled)
  * `api_code` - blockchain.info wallet API code (optional)
  * `from` - bitcoin address or account index to send from (optional)
  * `fee` - specify transaction fee in satoshi
  * `fee_per_byte` - specify transaction fee-per-byte in satoshi

### Fetch Wallet Balance

Endpoint: `/merchant/:guid/balance`

Query Parameters:

  * `password` - main wallet password (required)
  * `api_code` - blockchain.info wallet API code (required)

### Enable HD Functionality

Endpoint: `/merchant/:guid/enableHD`

Query Parameters:

  * `password` - main wallet password (required)
  * `api_code` - blockchain.info wallet API code (optional)

### List Active HD Accounts

Endpoint: `/merchant/:guid/accounts`

Query Parameters:

  * `password` - main wallet password (required)
  * `api_code` - blockchain.info wallet API code (optional)

### List HD xPubs

Endpoint: `/merchant/:guid/accounts/xpubs`

Query Parameters:

  * `password` - main wallet password (required)
  * `api_code` - blockchain.info wallet API code (optional)

### Create New HD Account

Endpoint: `/merchant/:guid/accounts/create`

Query Parameters:

  * `label` - label for the newly created account (optional)
  * `password` - main wallet password (required)
  * `api_code` - blockchain.info wallet API code (optional)

### Get Single HD Account

Endpoint: `/merchant/:guid/accounts/:xpub_or_index`

Query Parameters:

  * `password` - main wallet password (required)
  * `api_code` - blockchain.info wallet API code (optional)

### Get HD Account Receiving Address

Endpoint: `/merchant/:guid/accounts/:xpub_or_index/receiveAddress`

Query Parameters:

  * `password` - main wallet password (required)
  * `api_code` - blockchain.info wallet API code (optional)

### Check HD Account Balance

Endpoint: `/merchant/:guid/accounts/:xpub_or_index/balance`

Query Parameters:

  * `password` - main wallet password (required)
  * `api_code` - blockchain.info wallet API code (optional)

### Archive HD Account

Endpoint: `/merchant/:guid/accounts/:xpub_or_index/archive`

Query Parameters:

  * `password` - main wallet password (required)
  * `api_code` - blockchain.info wallet API code (optional)

### Unarchive HD Account

Endpoint: `/merchant/:guid/accounts/:xpub_or_index/unarchive`

Query Parameters:

  * `password` - main wallet password (required)
  * `api_code` - blockchain.info wallet API code (optional)

### List Addresses (deprecated, use the HD API instead)

Endpoint: `/merchant/:guid/list`

Query Parameters:

  * `password` - main wallet password (required)
  * `api_code` - blockchain.info wallet API code (optional)

### Fetch Address Balance (deprecated, use the HD API instead)

Endpoint: `/merchant/:guid/address_balance`

Query Parameters:

  * `address` - address to fetch balance for (required)
  * `password` - main wallet password (required)
  * `api_code` - blockchain.info wallet API code (optional)

### Generate Address (deprecated, use the HD API instead)

Endpoint: `/merchant/:guid/new_address`

Query Parameters:

  * `password` - main wallet password (required)
  * `label` - label for the address (optional)
  * `api_code` - blockchain.info wallet API code (optional)

### Archive Address (deprecated, use the HD API instead)

Endpoint: `/merchant/:guid/archive_address`

Query Parameters:

  * `address` - address to archive (required)
  * `password` - main wallet password (required)
  * `api_code` - blockchain.info wallet API code (optional)

### Unarchive Address (deprecated, use the HD API instead)

Endpoint: `/merchant/:guid/un

archive_address`

Query Parameters:

  * `address` - address to unarchive (required)
  * `password` - main wallet password (required)
  * `api_code` - blockchain.info wallet API code (optional)

## RPC

Bitcoind compatible RPC API. Full documentation available [here](https://blockchain.info/api/json_rpc_api).

To start the RPC server:

```
$ blockchain-wallet-service start-rpc [options]
```

Differences from server API:

  * Option `-rpcssl` is not supported.
  * Method `listsinceblock` is not supported.
  * Param `minConfimations` is not supported for methods `listreceivedbyaccount` and `listreceivedbyaddress`.
  * Param `minimumConfirmations` is not supported for method `getbalance`.
  * Param `confirmations` is not supported for method `listaccounts`.
  * Responses representing transactions have a different format.

## Installation

Ensure you have [`nodejs`](https://nodejs.org) and [`npm`](https://npmjs.com) installed.

Installation:

```sh
$ npm install -g blockchain-wallet-service
```

To check the version:

```sh
$ blockchain-wallet-service -V
```

To update:

```sh
$ npm update -g blockchain-wallet-service
```

Requires:

  * node >= 6.0.0
  * npm >= 3.0.0

If encountering installation issues, refer to the troubleshooting section below.

## Troubleshooting

Installation errors:

  * If encountering `EACCESS` or permissions-related errors, try running the install as root using `sudo`.
  * For node-gyp or python errors, install with `npm install --no-optional`.

Startup errors:

  * If encountering `/usr/bin/env: node: No such file or directory`, node might not be installed or installed with a different name. Create a symlink to your node binary or install node through Node Version Manager.

Runtime errors:

  * If encountering a `TypeError` stating an object `has no method 'compare'`, it might be due to using a Node version older than 0.12. Upgrade to at least Node version 0.12.
  * Wallet decryption errors despite correct credentials might indicate a missing Java installation, required by a dependency. Install the JDK.

Timeout Errors:

  * If experiencing timeout responses, additional authorization from your blockchain wallet may be needed. Check your email for an authorization request.

For further assistance, open a github issue or visit the [support center](https://blockchain.zendesk.com).

## Usage

After installation, use the `blockchain-wallet-service` command.

### Options

  * `-h, --help` - display usage information
  * `-V, --version` - display the version number
  * `-c, --cwd` - use the current directory as the wallet service module (for development)

### Commands

#### start

Usage: `blockchain-wallet-service start [options]`

Starts the service, making Blockchain Wallet API V2 available on a specified port.

Command options:

  * `-h, --help` - display usage information
  * `-p, --port` - specify the server port (default: `3000`)
  * `-b, --bind` - bind to a specific IP address (default: `127.0.0.1`)
  * `--ssl-key` - path to SSL key (optional)
  * `--ssl-cert` - path to SSL certificate (optional)

To allow all incoming connections, bind to `0.0.0.0`.

#### start-rpc

Usage: `blockchain-wallet-service start-rpc [options]`

Starts the JSON RPC server.

Options:

  * `-k, --key` - API code for server requests (required)
  * `-p, --rpcport` - specify the RPC server port (default: `8000`)
  * `-b, --bind` - bind to a specific IP address (default: `127.0.0.1`)

Get an API code [here](https://blockchain.info/api/api_create_code).

### Examples

To start the Wallet API service on port 3000:

```sh
$ blockchain-wallet-service start --port 3000
```

## Development

  1. Clone this repository.
  2. Run `yarn --ignore-engines`.
  3. Run `yarn start`.
  4. The development server is now running on port 3000.

For developing `blockchain-wallet-client` alongside this module, create a symlink to `my-wallet-v3`:

```sh
$ ln -s ../path/to/my-wallet-v3 node_modules/blockchain-wallet-client
```

### Testing

```sh
$ yarn test
```

### Configuration

Configure optional parameters in a `.env` file:

  * `PORT` - port number for running the dev server (default: `3000`)
  * `BIND` - IP address to bind the service to (default: `127.0.0.1`)

## Deployment

For UNIX production servers, run:

```sh
$ nohup blockchain-wallet-service start --port 3000 &
```
