# cbyam's Umbrel App Store

Community app store for [umbrelOS](https://umbrel.com). Currently ships:

## Solo Pool RS

[solo-pool-rs](https://github.com/cbyam/solo-pool-rs): a self-contained solo
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

3. **Set your payout address** (required: the pool hands out no work until
   you do). Open the app from its tile and save your address on the
   dashboard's **Settings** page. No SSH or config editing is needed, and the
   address can be changed there at any time. Every block reward goes to it.

4. Point your miners at `stratum+tcp://<umbrel-ip>:3333` with any worker
   name and any password. SV2 devices use the same host and port.

The dashboard is available from the app's tile (behind your umbrelOS login).

### Notes

- RPC credentials and the ZMQ block-notification endpoint are wired
  automatically from the Bitcoin Node app, so there is nothing to configure.
- All state (config, all-time stats, found-block archive) lives in
  `~/umbrel/app-data/cbyam-solo-pool-rs/data/` and survives updates.
- Each found block is archived to disk while it is submitted, and
  resubmitted automatically if your node hiccups. See the
  [project README](https://github.com/cbyam/solo-pool-rs) for details.

### Tested on umbrelOS

The app has been tested in an umbrelOS 1.7.4 virtual machine (KVM, with the
Bitcoin Node 1.4.0 app on regtest):

- **Fresh install** (0.6.7, 2026-09-06): added this store, installed the
  app, saved a payout address, and a Stratum client connected and received
  work. Block notifications arrived over ZMQ from the Bitcoin Node app.
- **Restart** (0.6.8, 2026-09-06): 12 blocks mined through the pool, then
  an app restart. The found blocks, the payout address and the pool's SV2
  identity all survived, and work resumed.
- **Update in place** (0.6.8 to 0.6.11, 2026-10-16): Umbrel's own app update
  on the install above. The stats database migrated with its found blocks
  and payout address intact, the SV2 identity key was unchanged and
  `config.toml` was untouched. Afterwards, three blocks mined over Stratum V1
  were accepted by the node, and a Stratum V2 client completed the Noise
  handshake and received work.
- **Update in place** (0.6.11 to 0.6.12, 2026-10-23): the same steps. The
  payout address, the 15 found blocks, the SV2 identity key and
  `config.toml` carried over unchanged. Three blocks mined over Stratum V1
  were accepted by the node, and after an app restart the same state held
  and a fourth block was accepted. Stratum V2 was not re-tested for this
  update.
- **Update in place** (0.6.12 to 0.6.13, 2026-10-23): the same steps, with
  the same results: state carried over unchanged, and four blocks were
  accepted across a restart. The boot log held no warnings. Stratum V2 was
  not re-tested for this update.

The restart and update tests ran the package submitted to the official
Umbrel store, which uses the same image and wiring as this one under a
different app id and Stratum port. Their steps and results are recorded in
[getumbrel/umbrel-apps#6064](https://github.com/getumbrel/umbrel-apps/pull/6064).

### Support

Issues and questions: https://github.com/cbyam/solo-pool-rs/issues
