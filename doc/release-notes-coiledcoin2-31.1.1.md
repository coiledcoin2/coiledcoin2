# CoiledCoin II Core 31.1.1 – "ASERT with Shears StallGuard"

**Upgrade required before block 9,925.** Every node, wallet, pool and exchange must run 31.1.1:
older versions stop at block 9,924.

## Why

On 30–31 October 2026 a very large merge-miner (about 4.7 PH/s, roughly 300 times the usual network)
mined CoiledCoin II for a few hours and then left. The old difficulty rule (LWMA, 60 blocks) had climbed
to about 126 million and only comes down a little with each new block, so the remaining miners could no
longer find blocks: the chain stopped at block 9,924 (06:50 UTC, 31 Oct). Without a change it would have
taken weeks to recover.

## What changes (consensus – from block 9,925)

**ASERT with Shears StallGuard** replaces LWMA:

1. **Step-by-step ASERT** (the aserti3-2d maths used by Bitcoin Cash, by Mark Lundeberg and
   Jonathan Toomim). Each block's difficulty follows how long the previous block took, with a
   **30-minute half-life**: it rises smoothly when blocks come fast and falls when they come slowly.
   It is measured block to block, so there is no "timetable" to catch up after a quiet spell.
2. **Shears StallGuard.** If no block has been found for **3 hours**, the next block may be easier:
   the difficulty halves for every further **30 minutes** of waiting. A big miner leaving can no
   longer freeze the chain.
3. **A floor of difficulty 1,000,000** – about one block an hour for a single 1.2 TH/s Bitaxe. However
   long the chain is idle, the difficulty never drops below this, so it never becomes "free" to mine.
4. **Block times may be at most 10 minutes in the future** (Bitcoin allows 2 hours), so timestamps
   cannot usefully be pushed ahead to make StallGuard kick in early.

Block 9,925 restarts at difficulty 1,000,000. All blocks up to 9,924 are checked exactly as before;
no coins or balances change.

Mining software also refreshes merge-mining work at least once a minute (`createauxblock`), and
`getblocktemplate` recomputes the difficulty as time passes, so pools see StallGuard's easier
difficulty during a stall.

## What it cannot do

No difficulty rule can stop a miner with far more hashrate from finding most blocks while they are
mining. What 31.1.1 guarantees is that the chain **never hangs** (worst case a few hours), **never
becomes easy** (the floor), and adjusts quickly both ways.

## Upgrading

- Back up your wallet (File → Backup Wallet), stop the old node/wallet, start 31.1.1. The data
  directory and wallets are used as they are.
- If your computer clock is badly wrong (more than a few minutes), set it to update automatically –
  with the 10-minute limit, a node with a fast clock could reject valid blocks until its clock is right.

## Credits

ASERT: Mark Lundeberg and Jonathan Toomim (Bitcoin Cash Node). StallGuard and the floor: Shears Mining.
