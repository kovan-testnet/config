# Connecting to the Kovan Testnet

Kovoan requires the use of Parity's PoA Protocol. Ensure [Parity](https://github.com/ethcore/parity) is installed.

## Non-Authorities (Regular Users)

### OSX / Linux

Most users who want to interact with the Kovan chain simply need to run parity with the correct chain config:

* Parity 1.5.5 or greater, use `parity --chain=kovan`
* For Parity 1.5.4, use `parity --chain=kovan-config.json`

[kovan-config.json](https://github.com/kovan-testnet/config/blob/master/kovan-config.json) in this repo.

### Windows

A set up guide video for windows has also been created: https://www.youtube.com/watch?v=dTjstkfOT8E

## Authorities (Block Validators)

Additionally, authorities need to have their keystores and decryption keys set up:

* `~/.local/share/io.parity.ethereum/keys/kovan/[keystore file].json`
* `~/[file containing keystore password]`

Then start parity with the following command:

```
parity --chain=kovan-config.json --force-sealing --engine-signer=[authority address] --password=[file containing keystore password]
```

### Netstats 

For Authorities, It's advised to set up [netstats](https://github.com/cubedro/eth-net-intelligence-api), so your node status is visible on http://kovan-stats.parity.io/.

```
git clone https://github.com/cubedro/eth-net-intelligence-api;
cd eth-net-intelligence-api;
npm install;
npm install -g pm2;
```

Edit the config file `eth-net-intelligence-api/app.json`:

```
[
  {
    "name"              : "kovan-netstats",
    "script"            : "app.js",
    "log_date_format"   : "YYYY-MM-DD HH:mm Z",
    "merge_logs"        : false,
    "watch"             : false,
    "max_restarts"      : 10,
    "exec_interpreter"  : "node",
    "exec_mode"         : "fork_mode",
    "env":
    {
      "NODE_ENV"        : "production",
      "INSTANCE_NAME"   : "[company name] Kovan Authority Node #0",
      "CONTACT_DETAILS" : "[admin email]",
      "WS_SERVER"       : "wss://kovan-stats.parity.io",
      "WS_SECRET"       : "be7ky6mo",
      "VERBOSITY"       : 2
    }
  }
]
```

Then start netstats:

```
pm2 start app.json
```
