# Deploying duc-blockscout for Ducros

Status: base merge + Ducros config done locally, not deployed yet.

## What's done

- Merged upstream [`blockscout/blockscout`](https://github.com/blockscout/blockscout)
  `v11.2.6` into this repo on branch `vendor/blockscout-v11.2.6` (history
  preserved, same pattern as `duchain`'s go-ethereum upgrade — an `upstream`
  remote is set so future Blockscout releases can be merged in later).
- `docker-compose/envs/common-blockscout.env`: `ETHEREUM_JSONRPC_VARIANT=geth`
  (already correct — duchain is a geth fork), `COIN=DUC`,
  `COIN_NAME=Ducros`, `BLOCKSCOUT_HOST`, fresh `SECRET_KEY_BASE`.
- `docker-compose/envs/common-frontend.env`: network name/id (`271017`),
  currency `DUC`, `NEXT_PUBLIC_IS_TESTNET=false`.
- Everything else (Postgres, redis, stats, sig-provider, etc.) is left at
  upstream defaults — no reason to touch it for a basic explorer.

## What's still needed before this is live

1. **Pick the target server.** Needs Docker + Docker Compose. Given the
   RPC dependency, either run this on the same box as the RPC node
   (simplest — `host.docker.internal` default works untouched) or on its
   own box pointed at the RPC node's public/internal IP.
2. **Fill in the real RPC URL** in `common-blockscout.env`
   (`ETHEREUM_JSONRPC_HTTP_URL` / `ETHEREUM_JSONRPC_TRACE_URL`) if not
   running on the same host as the RPC node — search for the `TODO` markers.
3. **Make sure the RPC node exposes `debug`/`trace` APIs**, not just
   `eth,net,web3` (needed for internal transactions). Check the node's
   `--http.api` flag.
4. **DNS**: point `explorer.ducros.org` (or whatever hostname you pick — the
   configs currently assume this one) at the target server's IP.
5. **TLS**: the bundled `proxy` service (`docker-compose/services/nginx.yml`)
   is plain HTTP nginx on port 80/8080/8081, no Let's Encrypt built in. Same
   pattern as the pool dashboard — put your own nginx + certbot in front,
   or terminate TLS at that layer and reverse-proxy to this stack's port 80.
6. **Regenerate `SECRET_KEY_BASE`** again if you want a value nobody else
   has ever seen (`openssl rand -base64 48`) — the one committed here was
   generated for this repo but treat it like any other secret.
7. **Bring it up**:
   ```bash
   cd docker-compose
   docker compose -f docker-compose.yml -f geth.yml up -d
   ```
8. **Verify**: once synced, open the explorer and check that recently mined
   blocks show up with the correct miner/coinbase address (the RandomX
   reward address) and that a normal transaction's page loads.
9. **Block reward display**: Ducros mints a fixed 9 DUC/block with a 10%
   treasury cut (`core/state_transition.go` in `duchain`). Blockscout's
   default reward calculation assumes standard Ethereum-style emission —
   worth checking once real blocks are indexed to see if the displayed
   reward matches reality, and adjusting `EMISSION_FORMAT`/reward logic if
   not.

## License note (read before going public)

Blockscout ships under a **source-available**, not OSI-open-source, license
(`LICENSE` in this repo, "Blockscout Software Licence"). Two things that
apply here:

- **Attribution is mandatory**: the frontend must keep a visible "Powered by
  Blockscout" notice/link, in the same style as the footer on
  `eth.blockscout.com`. Don't strip branding.
- **Monetized/hosted use requires a separate commercial license.** Running
  this as a free, self-hosted explorer for your own chain (which is the
  plan here) is covered by the base license. If you ever want to charge
  anyone for access/hosting/support built on this software, that needs a
  Commercial Licence from Blockscout Limited first — see `LICENSE` section
  4 or https://eaas.blockscout.com/#contact.
