# Bitcoin, Ordinals & Starknet Node Setup

This repository contains Docker Compose configuration for running a Bitcoin Core node, an Ordinals (ord) node, and a Starknet node with HTTPS support.

## Components

- **Bitcoin Core**: Full Bitcoin node with RPC enabled
- **Ordinals (ord)**: Indexing service for Bitcoin Ordinals and Runes
- **Starknet (Juno)**: Full Starknet node for L2 blockchain access
- **Nginx with Certbot**: Reverse proxy with automatic TLS certificate management for ord and Juno

## Prerequisites

- Docker and Docker Compose
- At least 4TB storage space for blockchain data
- 16GB of RAM
- Domain names pointing to your server (configured in `user_conf.d/mainnet.conf`)

## Quick Start

1. Create a `.env` file with your credentials:
   ```
   # Bitcoin
   RPC_AUTH=your_rpc_auth_string
   RPC_PASSWORD=your_rpc_password
   
   # Starknet
   ETH_NODE_URL=your_ethereum_node_websocket_url
   ```

2. Update the domain names in `user_conf.d/mainnet.conf` to match your domains. The example uses:
   - `btc.federation.lfg.rs` for Bitcoin RPC
   - `ord.federation.lfg.rs` for Ordinals API
   - `starknet.federation.lfg.rs` for Starknet API

3. Start the services:
   ```
   docker compose up --build
   ```

4. For testing with Let's Encrypt staging certificates, set `STAGING=1` in `nginx-certbot.env`

## Storage Locations

- Bitcoin data: `/data/btc`
- Ordinals data: `/data/ord`
- Starknet data: `/data/starknet`
- SSL certificates: managed by the nginx-certbot container

