# cbyam's Umbrel App Store

Community app store for [umbrelOS](https://umbrel.com). Currently ships:

## Solo Pool RS

[solo-pool-rs](https://github.com/cbyam/solo-pool-rs) — a self-contained solo
Bitcoin mining pool. Stratum V1 **and** Noise-encrypted Stratum V2
(NerdQAxe++-ready) auto-detected on one port; 100% of every block found pays
out to your address.

### Install

1. In umbrelOS, open the **App Store**, click the **⋯** menu → **Community App
   Stores**, paste this repository's URL, and click **Add**:

   ```
   https://github.com/cbyam/umbrel-app-store
   ```

2. Open the store and install **Solo Pool RS** (it requires the **Bitcoin
   Node** app, which umbrelOS installs first if missing).

3. **Set your payout address** (required — the pool won't produce work until
   you do). SSH into your Umbrel and edit the config:

   ```bash
   ssh umbrel@umbrel.local
   nano ~/umbrel/app-data/cbyam-solo-pool-rs/data/config.toml
   # set coinbase_address = "bc1q..." (YOUR address), save
   ```

   Then restart the app from its menu in umbrelOS.

4. Point your miners at `stratum+tcp://<umbrel-ip>:3333` — any worker name,
   any password. SV2 devices use the same host and port.

The dashboard is available from the app's tile (behind your umbrelOS login).

### Notes

- RPC credentials and the ZMQ block-notification endpoint are wired
  automatically from the Bitcoin Node app — nothing to configure.
- All state (config, all-time stats, found-block archive) lives in
  `~/umbrel/app-data/cbyam-solo-pool-rs/data/` and survives updates.
- Found blocks are archived to disk **before** submission and resubmitted
  automatically if your node hiccups — see the
  [project README](https://github.com/cbyam/solo-pool-rs) for details.

### Support

Issues and questions: https://github.com/cbyam/solo-pool-rs/issues
