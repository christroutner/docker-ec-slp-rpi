# Docker Electron Cash SLP, Raspbery Pi Edition
This repository contains Docker container code for running the
[Electron Cash SLP](https://github.com/simpleledger/Electron-Cash-SLP) fork on
a Raspberry Pi. Furthermore, the software is run in daemon mode so the RPi can
be used as a stand-alone server for validating SLP transactions over JSON RPC.

There are a lot of moving targets with this software. Electron Cash is constantly
changing, SLP protocol is changing, Python and associated libraries are also
constantly changing. Things break. No guarantee this repo will work.

# Installation and Usage
It's assumed that you are starting with a fresh installation of Raspbian on
a Raspberry Pi B+ v3.

- Clone this repository in your home directory with the following command:
`git clone https://github.com/christroutner/docker-slp`

- Initialze the RPi by running the `./init-rpi.sh` bash script. This will remove
a lot of unneeded software, update the OS, install Node.js and Docker.

- Build the [python base](python-base) image first, as it will be needed by the
[Electron Cash SLP wallet](ec-slp) image.

- Build the [ec-slp](ec-slp) container by running the `./build-image.sh` shell
script.

This curl command can be used to test the system to see if it accurately validating
SLP transactions:

`curl --data-binary '{"jsonrpc": "2.0", "id":"testing", "method": "slpvalidate", "params": ["2504b5b6a6ec42b040a71abce1acd71592f7e2a3e33ffa9c415f91a6b76deb45", false, false] }' -H 'content-type: text/plain;' http://localhost:5111`

The first call will fail, but calls should return:
`{"result": "Valid", "id": "testing", "jsonrpc": "2.0"}`

## License

MIT © Chris Troutner
