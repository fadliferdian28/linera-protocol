# <img src="https://github.com/linera-io/linera-protocol/assets/1105398/fe08c941-93af-4114-bb83-bcc0eaec95f9" width="250" height="85" />

[![License](https://img.shields.io/github/license/linera-io/linera-protocol)](LICENSE)
[![Build Status for Docker](https://github.com/linera-io/linera-protocol/actions/workflows/docker-compose.yml/badge.svg)](https://github.com/linera-io/linera-protocol/actions/workflows/docker-compose.yml)
[![Build Status for Rust](https://github.com/linera-io/linera-protocol/actions/workflows/rust.yml/badge.svg)](https://github.com/linera-io/linera-protocol/actions/workflows/rust.yml)
[![Build Status for Documentation](https://github.com/linera-io/linera-protocol/actions/workflows/documentation.yml/badge.svg)](https://github.com/linera-io/linera-protocol/actions/workflows/documentation.yml)
[![Twitter](https://img.shields.io/twitter/follow/linera_io)](https://x.com/linera_io)
[![Discord](https://img.shields.io/discord/984941796272521226)](https://discord.com/invite/linera)

---

[**Linera**](https://linera.io) is a decentralized blockchain infrastructure designed for highly scalable, secure, and low-latency Web3 applications.
It enables developers to build next-generation decentralized applications (dApps) with **microchains**, achieving parallel execution and instant finality.

---

## 📖 Documentation

* [Developer Portal](https://linera.dev)
* [Whitepaper](https://linera.io/whitepaper)

---

## 📂 Repository Structure

The main crates and directories are organized from low to high levels in the dependency graph:

* [`linera-base`](https://linera-io.github.io/linera-protocol/linera_base/index.html) – Base definitions, including cryptography.
* [`linera-version`](https://linera-io.github.io/linera-protocol/linera_version/index.html) – Versioning utilities for binaries and services.
* [`linera-views`](https://linera-io.github.io/linera-protocol/linera_views/index.html) – Mapping complex data structures onto key-value stores.
* [`linera-execution`](https://linera-io.github.io/linera-protocol/linera_execution/index.html) – Runtime and execution logic for Linera applications.
* [`linera-chain`](https://linera-io.github.io/linera-protocol/linera_chain/index.html) – Chains of blocks, certificates, and cross-chain messaging.
* [`linera-storage`](https://linera-io.github.io/linera-protocol/linera_storage/index.html) – Storage abstractions built on top of `linera-chain`.
* [`linera-core`](https://linera-io.github.io/linera-protocol/linera_core/index.html) – Core protocol (client/server logic, node synchronization).
* [`linera-rpc`](https://linera-io.github.io/linera-protocol/linera_rpc/index.html) – RPC message types and schemas.
* [`linera-client`](https://linera-io.github.io/linera-protocol/linera_client/index.html) – Libraries and CLI tools for clients.
* [`linera-service`](https://linera-io.github.io/linera-protocol/linera_service/index.html) – Executables: CLI wallets, validator frontends, and servers.
* [`linera-sdk`](https://linera-io.github.io/linera-protocol/linera_sdk/index.html) – SDK for developing Linera apps in Rust for Wasm.
* [`examples`](./examples) – Example Linera applications in Rust.

---

## ⚙️ Prerequisites

See [`INSTALL.md`](./INSTALL.md) for software requirements.
Ensure you have **Rust**, **Cargo**, and **Docker** installed.

---

## 🚀 Quickstart (Linera CLI)

The following commands set up a **local test network** and run transfers between microchains owned by a single wallet:

```bash
# Build binaries
cargo build -p linera-storage-service -p linera-service --bins
export PATH="$PWD/target/debug:$PATH"

# Import helper
source /dev/stdin <<<"$(linera net helper 2>/dev/null)"

# Start local test network
linera_spawn linera net up --with-faucet --faucet-port 8080

# Set faucet URL
FAUCET_URL=http://localhost:8080

# Wallet setup
export LINERA_WALLET="$LINERA_TMP_DIR/wallet.json"
export LINERA_KEYSTORE="$LINERA_TMP_DIR/keystore.json"
export LINERA_STORAGE="rocksdb:$LINERA_TMP_DIR/client.db"

# Initialize wallet
linera wallet init --faucet $FAUCET_URL

# Request two chains
INFO1=($(linera wallet request-chain --faucet $FAUCET_URL))
INFO2=($(linera wallet request-chain --faucet $FAUCET_URL))
CHAIN1="${INFO1[0]}" ; ACCOUNT1="${INFO1[1]}"
CHAIN2="${INFO2[0]}" ; ACCOUNT2="${INFO2[1]}"

# Show chains
linera wallet show

# Transfer and query balances
linera transfer 10 --from "$CHAIN1" --to "$CHAIN2"
linera transfer 5 --from "$CHAIN2" --to "$CHAIN1"
linera query-balance "$CHAIN1" "$CHAIN2"

# Fund user balances
linera transfer 5 --from "$CHAIN1" --to "$CHAIN1:$ACCOUNT1"
linera transfer 2 --from "$CHAIN1:$ACCOUNT1" --to "$CHAIN2:$ACCOUNT2"

linera query-balance "$CHAIN1:$ACCOUNT1"
linera query-balance "$CHAIN2:$ACCOUNT2"
```

More complex usage examples can be found in our [Developer Manual](https://linera.dev) and [`examples`](./examples).

---

## 🤝 Contributing

We welcome contributions from the community!

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to your branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

See our [Contribution Guide](./CONTRIBUTING.md) for details.

---

## 🌍 Community

* [Twitter / X](https://x.com/linera_io)
* [Discord](https://discord.com/invite/linera)
* [Governance Forum](https://governance.linera.io)

---
