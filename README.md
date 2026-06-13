<p align="center">
  <img src="assets/nyxara-banner.svg" alt="Nyxara" width="100%"/>
</p>

<h1 align="center">Nyxara</h1>

<p align="center">
  <em>On chain space exploration on Stellar. Scan procedurally generated nebulae, mint resources, own your ships, build alliances, no combat, no pay to win.</em>
</p>

<p align="center">
  <a href="https://github.com/noctics-labs/Nyxara/actions"><img alt="CI" src="https://img.shields.io/badge/CI-passing-brightgreen"/></a>
  <a href="https://github.com/noctics-labs/Nyxara/blob/main/LICENSE"><img alt="License" src="https://img.shields.io/badge/license-MIT-blue"/></a>
  <img alt="Built on" src="https://img.shields.io/badge/built%20on-Stellar%20Soroban-7b4bd8"/>
  <img alt="Language" src="https://img.shields.io/badge/language-Rust-dea584"/>
</p>

<p align="center">
  <a href="#overview">Overview</a> ·
  <a href="#design-principles">Design principles</a> ·
  <a href="#game-systems">Game systems</a> ·
  <a href="#architecture">Architecture</a> ·
  <a href="#getting-started">Getting started</a> ·
  <a href="#roadmap">Roadmap</a> ·
  <a href="#contributing">Contributing</a>
</p>

---

## Overview

Nyxara is an open source decentralized space exploration game built entirely on Stellar Soroban. Players scan procedurally generated nebulae, harvest resources, upgrade their explorer ships, form alliances and participate in a player driven economy, with every action recorded on chain.

Procedural generation uses Stellar ledger data as the random seed, which makes every nebula scan deterministic and verifiable. Two players running the same scan get the same result. Nobody can fake what they found because the math behind it is fully reproducible from public ledger state.

The game is intentionally cooperative and exploration focused. There is no combat, no pay to win, and no premium currency that bypasses gameplay. Anyone with a Stellar account can play.

## Design principles

- **Ledger seeded procedural generation.** Randomness comes from the Stellar ledger, which means the game can prove what a player found without any centralized service.
- **True ownership.** Ships, resources, blueprints and achievements are Soroban native tokens. Nobody can revoke them.
- **Player driven economy.** Resources are mintable and tradeable. Markets and prices emerge from player behaviour, not from a developer dashboard.
- **No combat, no pay to win.** Progress comes from exploration, crafting and collaboration, not from spending real money or destroying other players.
- **Verifiable fairness.** The same seed gives the same result for every player. No hidden weighting, no preferred accounts.

## Game systems

Nyxara is a large project. The codebase ships with over seventy on chain modules covering the full game stack. Some of the major systems:

### Exploration and resources

- `nebula_*`: procedural nebula generation and scanning
- `resource_minter`: mint discovered resources as tradeable Soroban tokens
- `fractional_resources`: split high value resources into tradeable fractions
- `recycling_crafter`: combine and break down resources into new materials
- `treasure_vault`: time locked rewards from rare discoveries

### Ships and progression

- `ship_nft`: explorer ships as Soroban native tokens
- `ship_registry`: ownership, history and lineage tracking
- `ship_upgrade`: modular ship improvements through blueprints
- `blueprint_factory`: blueprint creation and trading
- `fleet_manager`: group multiple ships under one operator
- `achievement_engine`, `achievements`, `badges`: long lived player progression

### Economy and markets

- `market_oracle`: on chain price discovery for player traded resources
- `escrow_trader`: secure peer to peer trades with no third party custody
- `yield_farming`, `yield_forecast`: passive yield on staked resources
- `dex_integration`: bridges to external Stellar DEXes for swaps
- `bounty_board`, `bug_bounty_payout`: bounties for completed missions and reported issues

### Social and cooperation

- `alliance_manager`: form and govern player alliances
- `gifting_system`: send resources and ships to other players
- `referral_system`: rewards for bringing new explorers into the game
- `entanglement_comms`: privacy preserving in game messaging

### Infrastructure

- `gas_sponsor`, `gas_recovery`: reduce friction for new players
- `rate_limiter`: protect contract endpoints from abuse
- `audit_logger`: append only log for cross system events
- `emergency_controls`: circuit breakers for critical paths
- `health_monitor`: contract level health metrics
- `governance`: protocol parameter updates via on chain vote

A full list of modules is in `src/`. Each module has its own focused responsibility and can be developed and tested independently.

## Architecture

```
nyxara/
  src/                Soroban smart contract modules (Rust, 70+ modules)
    nebula_*          Procedural region generation
    ship_*            Ship ownership and progression
    resource_*        Resource discovery, minting and trading
    market_*          Pricing and trading infrastructure
    alliance_*        Social and cooperative systems
    governance, audit_logger, emergency_controls, ...
    bridge/           Cross chain bridge module
    lib.rs            Crate entry point
    shared_lib.rs     Shared types and utilities
  tests/              Integration tests across modules
  test_snapshots/     Snapshot fixtures for deterministic state
  examples/           Example scripts and clients
  monitoring/         Observability and metrics pipeline
  scripts/            Deployment automation
  docs/               Architecture, yield math, privacy stats, upgrade guide
```

### Data flow

- **On chain**: every scan, ship, resource, trade, alliance, achievement, governance vote.
- **Off chain (indexer)**: aggregated views for fast UI rendering, no authoritative state.
- **Determinism**: nebula scans are reproducible from ledger seed + scan parameters, no server side randomness.

## Tech stack

- **Smart contracts**: Rust, Soroban SDK
- **Build system**: Cargo workspace
- **Testing**: Soroban contract test framework, snapshot fixtures
- **Tooling**: Make, shell scripts, monitoring stack

## Getting started

### Prerequisites

- Rust 1.78 or newer
- Soroban CLI v20 or newer
- A Stellar futurenet or mainnet account with funded XLM
- (Optional) Docker for the monitoring stack

### Clone and build

```bash
git clone https://github.com/noctics-labs/Nyxara.git
cd Nyxara
cargo build --release --target wasm32-unknown-unknown
```

### Run the tests

```bash
cargo test
```

### Run the monitoring stack

```bash
./setup-monitoring.sh
```

See [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) for the deployment topology and [docs/UPGRADE_GUIDE.md](docs/UPGRADE_GUIDE.md) for the upgrade process.

## Roadmap

- [x] Core exploration and resource minting modules
- [x] Ship ownership, registry and upgrade system
- [x] Player alliances and cooperative systems
- [x] On chain market oracle and escrow trading
- [x] Achievement and badge progression
- [x] Privacy preserving messaging
- [x] Monitoring and observability stack
- [ ] Futurenet end to end pass for all modules (in progress)
- [ ] External audit of the governance and emergency_controls modules
- [ ] Cross chain bridge integration
- [ ] Public mainnet launch
- [ ] Community led seasonal events

## Contributing

Nyxara is built by a wide group of contributors and is structured to make it easy to pick up a single module without understanding the whole game. Good first issues are tagged by module.

1. Pick an unassigned issue tagged for a module you want to work on.
2. Comment on the issue so a maintainer can assign you.
3. Fork the repo, create a branch (`feat/123-short-description`), open a pull request that references the issue.
4. A maintainer will review within 48 hours.

Module changes that touch `governance`, `emergency_controls`, `audit_logger` or the bridge require two reviewers.

## Security

If you find a vulnerability:

- Do **not** open a public issue.
- Use the disclosure flow documented in [SECURITY.md](SECURITY.md) (where present) or contact a maintainer directly.

The `bug_bounty_payout` contract powers an in protocol bug bounty program. See its module documentation for the scope and payout terms.

## License

[MIT](LICENSE)
