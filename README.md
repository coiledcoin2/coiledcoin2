<p align="center"><img src="doc/coiledcoin2/clc2-banner.png" alt="CoiledCoin II" width="640"></p>

# CoiledCoin II (CLC2)

A fair, merge-mined SHA-256 relaunch of **CoiledCoin** (2012), which was killed by a 51% attack.
This is a **new chain** started from a new genesis block. It does not continue the old chain
and carries no balances from it.

Inspired by and named after the original CoiledCoin, with thanks to the original CoiledCoin
dev team. Built on **Bitcoin Core v31.1**, with **AuxPoW (merged mining) from Namecoin**.
Thanks to everyone who wrote that code.

## Launch

**Saturday 17 October 2026, 09:00 UTC (10:00 UK time).**

The genesis block is timestamped at the launch time, so no block can be valid before launch
(apart from the normal 2-hour clock allowance every Bitcoin-based coin has). The slow start
(below) means the first blocks are worth almost nothing, so starting early gains nothing.

## Coin details

| | |
|---|---|
| Ticker | CLC2 |
| Mining | SHA-256d, can be **merge-mined with Bitcoin** (AuxPoW, chain ID 16) or mined directly |
| Block time | 2 minutes |
| Difficulty | LWMA-1 (zawy12), adjusted every block over the last 60 blocks; fixed launch difficulty (65,536) for the first 60 blocks |
| Block reward | 20 CLC2, halving every 525,000 blocks (about 2 years) |
| Tail emission | the reward never drops below 1 CLC2 per block (as the original CoiledCoin) |
| Slow start | the reward ramps up linearly over the first 1,440 blocks (about 1 day) |
| Premine | **26,000 CLC2 in block 1** (about 0.25% of the first two years' supply), see below |
| Network port | 8370 |
| RPC port | 8371 |
| Addresses | start with `C` (legacy) or `clc1` (SegWit) |
| Seed node | `clc2seed.shears.co.uk` |

## Premine: declared and locked

Block 1 pays a one-time **developer premine of 26,000 CLC2** to:

`Cdb9tuFPx42wKXmk3CJQEDpikU5kn5FoBQ`

This is enforced by the coin's own rules (`src/validation.cpp`, rule `bad-cb-premine`):
a block 1 that does not pay the 26,000 CLC2 to this address is invalid, whoever mines it. There is no other premine,
no developer fee and no hidden reward. Anyone can check this address on the chain at any time.

## Genesis block

| | |
|---|---|
| Message | `The i Paper 16/10/2026 Energy bills set for double rise in the new year` |
| Time | 1792227600 (2026-10-17 09:00:00 UTC) |
| Nonce | 802020 |
| Hash | `0000072d1e172d1ae11bdd13c5be26de45a55f2d70ffc310959d8f50b4222677` |
| Merkle root | `0ca1ff21a2fed4f419c072c9a4ccd12e4f8701bc0ec4157124e594fdddab4ee5` |

## Downloads

Ready-built programs are on the [Releases](../../releases) page: a Windows wallet
(`coiledcoin2-qt.exe`) plus the node and command-line tool, and Linux programs built on
Ubuntu 24.04. Check the SHA-256 checksums listed there.

## Building from source

Exactly like Bitcoin Core v31.1. See [doc/build-unix.md](doc/build-unix.md) and
[doc/build-windows.md](doc/build-windows.md). In short, on Ubuntu 24.04:

```bash
sudo apt-get install build-essential cmake pkgconf python3 libevent-dev libboost-dev libsqlite3-dev
cmake -B build -DBUILD_GUI=OFF
cmake --build build -j$(nproc)
```

The programs are `build/bin/coiledcoin2d` and `build/bin/coiledcoin2-cli`. Data is kept in
`~/.coiledcoin2` (Linux) or `%LOCALAPPDATA%\CoiledCoin2` (Windows), settings in `coiledcoin2.conf`.

## Mining

CoiledCoin II can be merge-mined alongside Bitcoin with no loss of Bitcoin hashrate, or
mined directly. Nodes provide `createauxblock` / `submitauxblock` for merge-mining pools and
`getblocktemplate` for direct mining.

## License

MIT, like Bitcoin Core. See [COPYING](COPYING).
