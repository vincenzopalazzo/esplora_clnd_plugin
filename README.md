# esplora_clnd_plugin

![GitHub Workflow Status](https://img.shields.io/github/workflow/status/lightningamp/esplora_clnd_plugin/ci?style=for-the-badge)

[c-lightning](https://github.com/ElementsProject/lightning) C plugin allows c-lightning to use [esplora web explorer](https://blockstream.info) for fetching bitcoin data and send transactions.

Based on [sauron plugin](https://github.com/lightningd/plugins/tree/master/sauron) c-lightning plugin with esplora integration by [darosior](https://github.com/darosior).

#### Deps
Install libcurl and libssl, on linux:
```
sudo apt-get install libcurl4-openssl-dev libssl-dev
```

#### Build
1. copy `esplora.c` into `lightning/plugins` folder
2. apply `Makefile.patch` to add esplora in clightning plugins build (tested for clightning v0.9.3)
```
patch -p1 < Makefile.patch
```
3. add libcurl to LDLIBS dep (needed for esplora plugin)
```
sed -i 's/LDLIBS = /LDLIBS = -lcurl -lssl -lcrypto /g' Makefile
```
4. run make on your lightning folder

#### Run
Disable `bcli` plugin in order to fetch bitcoin data from `esplora` plugin, and set plugin options, as the following:
```
./lightningd/lightningd --testnet --disable-plugin bcli --log-level=debug
```

Full available options:
- `--esplora-api-endpoint=<url>`: set esplora endpoint (as https://blockstream.info/testnet/api for testnet). If it is not specified, the plugin set the @Blockstream API by default in accord with the lightningd network conf.
- `--esplora-verbose=1`: enable curl verbosity
- `--esplora-cainfo=<path>`: set path to Certificate Authority (CA) bundle (CA certificates extracted from Mozilla at https://curl.haxx.se/docs/caextract.html)
- `--esplora-capath=<path>`: specify directory holding CA certificates.
- `--esplora-disable-proxy`: ignore the proxy conf from the lightnind node and use esplora without proxy, if this option is missed esplora use the same proxy of lightnind (if there is one).
