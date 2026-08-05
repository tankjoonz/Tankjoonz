# ARENA — online multiplayer

This folder is everything needed to run the game with real multiplayer:
players see each other, shoot each other, and share the same bots and loot.

```
arena-server/
├── server.js        the relay server
├── package.json
└── public/
    └── index.html   the game itself
```

The server does two jobs at once: it serves the game **and** relays the shared
world state, so a single deploy is all you need.

## Why not Firebase?

Firebase (and most Western realtime backends) are unreachable from Iran, both
for setup and — more importantly — for the players themselves during a match.
This relay avoids that entirely: host it somewhere reachable from wherever your
players are, and nothing else is needed.

Any Node.js host works: **Liara**, **ArvanCloud**, **ParsPack**, or a plain VPS.

## Deploying on Liara (example)

1. Install the CLI on a computer: `npm i -g @liara/cli`
2. `liara login`
3. From inside this folder: `liara deploy --platform=node`
4. Liara gives you a URL such as `https://arena.liara.run`

Open that URL — the game is served from it and multiplayer works immediately,
because the client defaults to connecting back to the host that served it.

## Running it locally first (recommended)

```
npm install
npm start
```

Then open `http://localhost:3000`. Open it in two browser windows, join with the
same team name, and you should see both tanks in the same world.

## Hosting the game and server separately

If you'd rather keep the HTML on another host, open `public/index.html` and set:

```js
const SERVER_URL = 'wss://your-server-address';
```

Note: a page served over `https://` must use `wss://`, not `ws://`.

## Rooms

Everyone with the same `ROOM_ID` shares a world. To run separate matches, change
`ROOM_ID` at the top of `public/index.html`, or host multiple copies.

## Notes

- State is kept in memory only. Restarting the server resets the arena, which is
  fine for this game.
- When a player disconnects, the server removes their tank automatically.
- Boss waves are deliberately per-player: each player faces their own bosses
  based on their own kill count.
