# CoiledCoin II Core 31.1.2 – important bug fix

**Please upgrade.** No rule change: 31.1.2 follows exactly the same chain as 31.1.1, with the same
blocks, balances and wallets. You can upgrade at any time, and older nodes stay on the same chain.

## The problem

Bitcoin Core 31 needs a "headers sync" setting for every network. It limits how much memory a node
spends checking a chain that a peer offers before downloading it. CoiledCoin II 31.1.1 had this
setting for its test networks but **not for the main network**. Whenever a peer offered a side
chain with **less work** than the main chain, the node hit a safety check in `headerssync.cpp`:

    Assertion `params.commitment_period > 0' failed.

and stopped. Started again, it met the same peer and crashed again. One of our nodes restarted more
than 11,000 times before we noticed. Blocks found by a node in that state can be lost.

## The fix

The main network now has its headers-sync setting (`commitment_period = 673`,
`redownload_buffer_size = 14460`, the same kind of values the test networks use). The node now
checks such side chains safely and ignores them instead of crashing.

Nothing else changes: ASERT with Shears StallGuard (from block 9,925), the difficulty floor,
AuxPoW merge-mining, rewards, supply, addresses and the network protocol are all exactly as in
31.1.1.

## How to upgrade

Back up your wallet first (File > Backup Wallet, or `backupwallet`). Then stop CoiledCoin II,
replace the program files with the 31.1.2 ones, and start it again. Your data folder and wallet are
used as they are; no resync or reindex is needed.

## Credits

Found and fixed by Shears Mining while relaunching Sixbit on 12 November 2026. Built on Bitcoin
Core. ASERT by Mark Lundeberg and Jonathan Toomim (Bitcoin Cash Node).
