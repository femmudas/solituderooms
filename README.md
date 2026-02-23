# SolitudeRooms (Devnet)

SolitudeRooms is a token-gated, RAM-only room chat for Solana Devnet.
A single `server.ts` process runs Next.js, Express, and Socket.io.
When a room is buried by its creator, messages are wiped from memory and a DRiP export pack is generated.

## Stack
- Next.js App Router
- Express + Socket.io (single server)
- Solana Web3.js + Metaplex token metadata PDA lookup
- Canvas-based image export
- Helius Devnet RPC

## Environment
Create `.env` from `.env.example`:

```bash
cp .env.example .env
```

Example values:

```env
SOLANA_RPC=https://devnet.helius-rpc.com/?api-key=YOUR_HELIUS_API_KEY
NEXT_PUBLIC_SOLANA_RPC=https://devnet.helius-rpc.com/?api-key=YOUR_HELIUS_API_KEY
NEXT_PUBLIC_DRIP_STUDIO_URL=https://drip.haus/
```

## Install and Run

```bash
npm install
npm run dev
```

Open `http://localhost:3000`.

## Production Build

```bash
npm run build
npm run start
```

## Required Stone Assets
The following files must exist and be transparent `400x600` PNGs:
- `public/assets/stones/stone_1.png`
- `public/assets/stones/stone_2.png`
- `public/assets/stones/stone_3.png`
- `public/assets/stones/stone_4.png`
- `public/assets/stones/stone_5.png`

## Demo Token Setup (Metaplex CLI)

```bash
npm i -g @metaplex-foundation/cli
solana config set --url devnet
solana airdrop 2

mplx toolbox token create \
  --name "Solitude Demo Token" \
  --symbol "SOLI" \
  --description "Demo token for SolitudeRooms" \
  --image ./logo.png \
  --decimals 6 \
  --mint-amount 1000000 \
  --rpc "https://devnet.helius-rpc.com/?api-key=YOUR_HELIUS_API_KEY"
```

## Demo Flow
1. Connect wallet (Phantom, Solflare, or Backpack) on Devnet.
2. Paste token mint.
3. Wait for auto metadata scan.
4. Verify holdings (`/api/verify`, requires balance > 0).
5. Create room.
6. Chat.
7. Creator buries room.
8. Download DRiP export pack from `/api/drip/pack/:snapshotId?wallet=<creator_wallet>`.
9. Verify zip contains `memory_stone.png`, `metadata.json`, and `README.txt`.

## Canvas Troubleshooting
If `npm install` fails on `canvas`, install native dependencies:

### Windows
- Recommended: Node.js 20 or 22 with Visual Studio Build Tools.
- If source build is required, install GTK/cairo libraries and ensure `cairo.h` is available.

### macOS
```bash
xcode-select --install
brew install pkg-config cairo pango libpng jpeg giflib librsvg pixman
```

### Ubuntu/Debian
```bash
sudo apt-get update
sudo apt-get install -y build-essential libcairo2-dev libpango1.0-dev libjpeg-dev libgif-dev librsvg2-dev pkg-config
```

Re-run:

```bash
npm install
```
