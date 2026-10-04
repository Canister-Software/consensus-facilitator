# Consensus Protocol x402 Facilitator

x402 payment facilitator for the [Consensus Protocol](https://github.com/Demali-876/consensus) — verifies and settles micropayments across **ICP**, **EVM** (mainnet + Base + testnets), and **SVM** (mainnet + devnet). The Consensus orchestrator reaches it via `FACILITATOR_URL`.

Part of the Consensus multi-repo set: [consensus](https://github.com/Demali-876/consensus) · [consensus-client](https://github.com/Demali-876/consensus-client) · [consensus-node](https://github.com/Demali-876/consensus-node) · [consensus-docs](https://github.com/canister-software/consensus-docs). Architecture & cross-repo contracts: <https://docs.consensus.canister.software/protocol/architecture/>

## Development

```bash
npm install
npm run dev     # tsx watch src/index.ts
npm run build   # tsc → dist/
npm start       # node dist/index.js
```

Production runs under PM2 (`ecosystem.config.cjs`): `npm run pm2:start`.

## License

[MIT](./LICENSE) © Canister Software Inc
