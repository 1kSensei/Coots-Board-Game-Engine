# ⚔ Coot's Board Game Engine

> *craft · host · conquer*

A fully browser-based board game creation and multiplayer hosting platform. Design your own board games from scratch, share them with friends, and play together online — no server, no sign-up, no install required.

---

## 🌐 Live Demo

Deploy to GitHub Pages and share the link. See [Deployment](#-deployment) below.

---

## ✨ Features

### 🛠 Game Creator

| Feature | Details |
|---|---|
| **Board Builder** | Adjustable grid from 2×2 up to 30×30 tiles |
| **Tile Painting** | 40-color palette + custom hex color input |
| **Fill Tools** | Paint individual tiles, fill all, checkerboard pattern, erase |
| **Tile Labels** | Add text labels to any tile on the board |
| **Cell Size** | Resize tiles from 30px to 120px for any screen |
| **Piece Designer** | Upload custom image icons or pick from built-in emoji |
| **Starting Positions** | Place each piece at a defined starting cell |
| **Piece Colors** | Assign a unique border color to each piece |
| **Dice System** | Add D4, D6, D8, D10, D12, D20, or any custom-sided die |
| **Multiple Dice** | Add multiple copies of any die type |
| **Custom Items** | Create collectible items that can be given to players mid-game |
| **Turn Order** | Choose: Roll-to-Decide, Random, Manual, or Host-First |
| **Rules Panel** | Full instructions text, player count range, play time, searchable tags |
| **Save & Edit** | Games saved to browser local storage, editable at any time |

### 🎮 Gameplay

| Feature | Details |
|---|---|
| **Drag & Drop** | Drag pieces across the board to any tile |
| **Turn Enforcement** | Only the active player can move pieces or roll dice |
| **Dice Tray** | Click any die to roll it with a spin animation |
| **Roll Results** | Results appear instantly in the game log for all players |
| **Player List** | Shows all connected players, their piece, and whose turn it is |
| **Item Grants** | Host can give custom items to any player mid-game |
| **Piece Tooltips** | Hover a piece to see its name, owner, and held items |
| **Game Log** | Real-time scrolling log of moves, rolls, chat, and system events |
| **In-Game Chat** | Send messages visible to all players |
| **End Turn Button** | Explicitly pass the turn to the next player |

### 🌐 Multiplayer

- **Peer-to-peer** via [PeerJS](https://peerjs.com/) — no backend server required
- **Room codes** — host shares a short alphanumeric code; friends enter it to connect
- **Works across the US** using PeerJS cloud relay (`0.peerjs.com`)
- **Full state sync** — piece positions, dice rolls, chat, turn order, and items all sync in real time
- **Lobby system** — players choose their piece before the game starts
- **Disconnect handling** — players are removed gracefully if they disconnect
- **Test Mode** — creators can test their game solo without needing other players

---

## 🚀 Deployment

This is a single-file application. The entire engine lives in `index.html`.

### GitHub Pages (Recommended)

1. Create a new GitHub repository
2. Upload `index.html` (and this `README.md`) to the root of the repo
3. Go to **Settings → Pages**
4. Under **Source**, select `Deploy from a branch` → `main` → `/ (root)`
5. Click **Save**
6. Your site will be live at `https://yourusername.github.io/your-repo-name`

> ⚠️ GitHub Pages requires the repo to be **public** for free accounts, or you need GitHub Pro for private repo pages.

### Other Hosts

Since everything is one file, you can also host it on:
- **Netlify** — drag and drop `index.html` into the Netlify dashboard
- **Vercel** — deploy as a static site
- **Any web server** — just serve `index.html` over HTTPS

> **HTTPS is required** for multiplayer. PeerJS will not establish peer connections over plain HTTP.

---

## 🎲 How to Use

### Creating a Game

1. Click **Create a Game** on the landing page
2. Use the **Board** tab to name your game, set board dimensions, and paint tiles
3. Use the **Pieces** tab to add game pieces — upload an image or pick an emoji, set the starting position and color
4. Use the **Dice** tab to add the dice your game uses
5. Use the **Rules** tab to write instructions, set player count, add custom items, and set turn order
6. Click **💾 Save Game** — your game appears in the Browse library
7. Click **▶ Test Game** to try it solo before hosting

### Hosting a Game

1. Find your game in the **Browse** tab
2. Click **Host Game**
3. Enter your name when prompted
4. Share the **room code** shown in the lobby with your friends
5. Wait for players to join and choose their pieces
6. Click **▶ Start Game**

### Joining a Game

1. Click **Join a Game** on the landing page, or use the Join button in Browse
2. Enter your name and the room code from the host
3. Choose your piece in the lobby
4. Wait for the host to start

### Playing

- **Move pieces** by dragging and dropping them onto any board tile
- **Roll dice** by clicking any die in the dice tray at the bottom
- **Chat** using the text bar below the game log
- **End your turn** with the **End Turn →** button
- **Leave** at any time with the Leave Game button

---

## 🏗 Architecture

```
index.html
├── CSS (custom properties, dark theme, all layouts)
├── HTML (landing, creator, browse, play, lobby, modals)
└── JavaScript
    ├── Creator Engine (board rendering, tile painting, piece/dice/item management)
    ├── Save System (localStorage-based game persistence)
    ├── PeerJS Multiplayer (P2P connections, state broadcast, message handling)
    ├── Lobby System (room codes, piece selection, turn order determination)
    └── Play Engine (drag & drop, dice rolling, game log, chat)
```

**No build step. No dependencies to install. No backend.**

The only external resources loaded at runtime are:
- [PeerJS 1.5.4](https://unpkg.com/peerjs@1.5.4/dist/peerjs.min.js) — peer-to-peer networking
- [Google Fonts](https://fonts.google.com/) — Cinzel Decorative, Crimson Pro, JetBrains Mono

---

## 🔌 Multiplayer Technical Notes

Coot's BGE uses a **host-authoritative P2P model**:

- The player who hosts the game is the **authority** — all game state changes (moves, rolls, turns) are sent to the host, validated, and then broadcast to all connected peers
- Connections use WebRTC data channels via PeerJS
- The PeerJS public cloud (`0.peerjs.com`) handles the initial signaling handshake; after that, data flows directly peer-to-peer where possible, or through the TURN relay when direct connection isn't possible
- Room codes are derived from the host's PeerJS peer ID and are valid for the duration of the browser session

---

## 📦 What's Saved Locally

Games you create are saved to your browser's `localStorage` under the key `cge_games`. This means:

- Games persist across browser sessions on the same device
- Games are **not** synced to the cloud or other devices
- Clearing browser data will delete saved games
- To share a game definition with someone else, you would need to manually export/import (future feature)

---

## 🗺 Roadmap / Possible Future Features

- [ ] Export/import game files (JSON download)
- [ ] Card deck system (draw piles, hands)
- [ ] Snap-to-grid animations during piece movement
- [ ] Spectator mode (watch without playing)
- [ ] Victory condition triggers
- [ ] Piece stat tracking (HP, gold, etc.)
- [ ] Board zones with special effects
- [ ] Timer per turn
- [ ] Mobile touch drag-and-drop

---

## 📄 License

MIT License — free to use, modify, and distribute.

---

*Built with love for tabletop fans everywhere. Roll well.* 🎲
