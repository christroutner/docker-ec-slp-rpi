# Docker SLP
This repository contains Docker containers, orchestrated with Docker Compose.
The purpose is to create the infrastructure needed to validate Simple Ledger
Protocol (SLP) token transactions.

Two Docker containers are created:
1. An [ElectrumX server](https://electrumx.readthedocs.io/en/latest/) which
monitors the BCH blockchain and creates its own
local database of transaction data. This takes up about 25GB of disk space.

2. An [Electron Cash SLP](https://github.com/simpleledger/Electron-Cash-SLP)
wallet running in daemon mode. This wallet has an RPC interface that
[slp.js](https://github.com/simpleledger/slpjs) uses for SLP transactions. The
wallet pulls its raw data from the ElextrumX server.

See the READMEs in the sub-directories for more information.

# Installation and Usage
It's assumed that you are starting with a fresh installation of Ubuntu
18.04 LTS on a 64-bit machine.
It's also assumed that you are installing as a
[non-root user with sudo privileges](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-04).

- Install Docker on the host system.
[This tutorial](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04)
shows how to install Docker on a Ubuntu system. It's specifically targeted to
Digital Ocean's cloud servers, but should work for any Ubuntnu system.

- Install Docker Compose too.
[This tutorial](https://www.digitalocean.com/community/tutorials/how-to-install-docker-compose-on-ubuntu-16-04) shows how to do so on a Ubuntu system.

- Clone this repository in your home directory with the following command:
`git clone https://github.com/christroutner/docker-slp`

- Run the [electrumx](electrumx) container by itself and allow it to sync to
the blockchain. This will take 1-2 days and will take up approximately 25GB of
disk space.

- Build the [python base](python-base) image first, as it will be needed by the
[Electron Cash SLP wallet](electron-cash-slp) image.

- customize the [docker-compose.yml](docker-compose.yml) file. Replace the
placeholders in the `DAEMON_URL` environment variable with the RPC information
for your own Bitcoin Cash full node.

- Build the docker containers:
`docker-compose build --no-cache`

- Bring everything online by running the following command:
`docker-compose up -d`

This curl command can be used to test the system to see if it accurately validating
SLP transactions:
`curl --data-binary '{"jsonrpc": "2.0", "id":"testing", "method": "slpvalidate", "params": ["2504b5b6a6ec42b040a71abce1acd71592f7e2a3e33ffa9c415f91a6b76deb45", false, false] }' -H 'content-type: text/plain;' http://localhost:5111`

## License

MIT © Chris Troutner
