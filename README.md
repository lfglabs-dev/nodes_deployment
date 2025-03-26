# Bitcoin, Ordinals & Starknet Node Setup

This repository contains Docker Compose configuration for running a Bitcoin Core node, an Ordinals (ord) node, a Starknet node with HTTPS support, and a Fordefi signer.

## Components

- **Bitcoin Core**: Full Bitcoin node with RPC enabled
- **Ordinals (ord)**: Indexing service for Bitcoin Ordinals and Runes
- **Starknet (Juno)**: Full Starknet node for L2 blockchain access
- **Nginx with Certbot**: Reverse proxy with automatic TLS certificate management for ord and Juno
- **Fordefi Signer**: API signer for secure transaction signing

## Prerequisites

- Docker and Docker Compose
- At least 4TB storage space for blockchain data
- 16GB of RAM
- Domain names pointing to your server (configured in `user_conf.d/mainnet.conf`)
- Fordefi account for API signer setup

## Quick Start

1. **Set up the Fordefi Signer**:
   
   Before proceeding with other components, you need to set up the Fordefi signer:
   
   a. Login to the Fordefi Docker registry:
   ```
   sudo docker login -ufordefi fordefi.jfrog.io
   ```
   (For the password, please contact th0rgal or the Fordefi team)
   
   b. Run the Fordefi API signer:
   ```
   sudo docker run --rm --log-driver local --mount source=fordefi_volume,destination=/storage -it fordefi.jfrog.io/fordefi/api-signer:latest
   ```
   
   c. When prompted, select "provision" and follow the activation steps as outlined in the Fordefi documentation:
   https://docs.fordefi.com/developers/getting-started/set-up-an-api-signer/activate-api-signer

2. Create a `.env` file with your credentials:
   ```
   # Bitcoin
   RPC_AUTH=your_rpc_auth_string
   RPC_PASSWORD=your_rpc_password
   
   # Starknet
   ETH_NODE_URL=your_ethereum_node_websocket_url
   ```

3. Update the domain names in `user_conf.d/mainnet.conf` to match your domains. The example uses:
   - `btc.federation.lfg.rs` for Bitcoin RPC
   - `ord.federation.lfg.rs` for Ordinals API
   - `starknet.federation.lfg.rs` for Starknet API

4. Start the services:
   ```
   docker compose up --build
   ```

5. For testing with Let's Encrypt staging certificates, set `STAGING=1` in `nginx-certbot.env`

## Storage Locations

- Bitcoin data: `/data/btc`
- Ordinals data: `/data/ord`
- Starknet data: `/data/starknet`
- Fordefi data: Docker volume `fordefi_volume`
- SSL certificates: managed by the nginx-certbot container

