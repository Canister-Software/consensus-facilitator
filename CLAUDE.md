# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

This is **`consensus-facilitator`** — the x402 payment facilitator for the [Consensus Protocol](https://github.com/Demali-876/consensus). It verifies and settles x402 payments across **ICP**, **EVM** (mainnet + Base + testnets), and **SVM** (mainnet + devnet). The Consensus orchestrator reaches it via `FACILITATOR_URL`.

**Canonical cross-repo reference:** the architecture + cross-repo contracts live in `consensus-docs` → https://docs.consensus.canister.software/protocol/architecture/ ([source](https://github.com/canister-software/consensus-docs/blob/main/src/content/docs/protocol/architecture.md)). Read it before changing the verify/settle API or the scheme/network identifiers — those are a contract with `consensus` (server `x402ResourceServer`) and `consensus-client`. Related repos: [`consensus`](https://github.com/Demali-876/consensus) (orchestrator), [`consensus-client`](https://github.com/Demali-876/consensus-client) (SDK + CLI), [`consensus-node`](https://github.com/Demali-876/consensus-node) (worker-node runtime), [`consensus-docs`](https://github.com/canister-software/consensus-docs) (docs).

## Commands

Node + TypeScript (`tsx` for dev, `tsc` for the build); PM2 in production.

```bash
npm install
npm run dev        # tsx watch src/index.ts
npm run build      # tsc → dist/
npm start          # node dist/index.js
npm run typecheck  # tsc --noEmit
npm run lint       # eslint src/
```

PM2: `npm run pm2:start | pm2:stop | pm2:restart | pm2:logs | pm2:status` (see `ecosystem.config.cjs`).

## Architecture

Fastify HTTP server (`src/index.ts`) exposing the x402 facilitator API (verify + settle). Logic is split per network:

- `src/icp/` — ICP scheme: `agent`, `identity`, `ledger`, `codec`, `verify`, `scheme`, `nonces`. Uses `@dfinity/*` + `@icp-sdk/core`.
- EVM + SVM verification/settlement via `@x402/evm` and `@x402/svm` (with `viem` and `@solana/kit`).
- `src/config.ts` — network + key configuration. `src/db.ts` — `better-sqlite3` persistence (e.g. nonce tracking). `src/types.ts` — shared types.

## Conventions

- ESM, TypeScript strict. `tsx` for dev, `tsc` for the `dist/` build.
- **Default branch is `master`** (the other Consensus repos use `main`).
- Keep real keys out of git — `src/icp/test-x402.pem` is a test identity only.
- Keep scheme/network identifiers (EVM `eip155:*`, SVM `solana:*`, ICP `icp:*`) aligned with the server and client; see the canonical reference.
