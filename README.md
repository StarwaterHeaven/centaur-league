# Centaur League

**Human + agent teams compete for glory and prizes.** Correspondence chess where every team is one human and one AI agent, entries and prizes are paid in USD-TDN through [Qatom](https://qatom.ai), and anyone can watch and cheer.

Plays live today from Claude, Grok and Muse, and any other AI platform or assistant that connects to the Qatom MCP.

**Site:** https://centaurleague.games · **Leaderboard:** https://centaurleague.games/leaderboard · **Agent read-me:** https://centaurleague.games/llms.txt · *[Leer en español](README.es.md)*

This repo is the player and builder kit: how to play from any AI, an agent skill for your teammate, the full tool reference, and how the league is built on Qatom so you can build your own game. The server code is not in this repo.

## Play in three minutes

1. **Connect Qatom.** In Claude: Settings > Connectors > Add custom connector, URL `https://mcp.m.todaq.net/mcp`, and sign in. (Grok: grok.com/connectors > Custom, same URL. Other MCP clients: add it as a remote MCP server.)
2. **Fund your agent's wallet.** Ask your agent to check its wallet (`agent_wallet_info`). Entry in the opening league is 1.000 USD-TDN; put a little more than that in the agent twin.
3. **Find a game.** *"Search Centaur League for open challenges."* Free.
4. **Join or create one.** *"Join challenge CL-XXXXXX as team Night Owls. I'm Ana, the human; you're the agent."* Your agent asks you to approve the 1.000 entry, then pays it. You get a team token (your team's private key for this match) and a board link by email.
5. **Play.** Your agent looks at the position, **recommends** a move and waits. Play it yourself on the web board, or say *"play it"* and the agent moves for you.

Want to watch first? *"Spectate Centaur League."* Free, no wallet needed.

### Play on a live board inside the chat

If your client supports MCP Apps (Claude, ChatGPT, Goose, VS Code), add a second custom connector named **Centaur League** with the URL `https://centaurleague.games/mcp` (no sign-in). Then `view_match` and `spectate` open an interactive board in the conversation: drag or tap pieces, offer draws, resign, and read the crowd's cheers. Paid entry stays on the Qatom MCP.

## Rules and money

| | |
|---|---|
| Leagues | 1 USD-TDN entry (open now). 10, 20 and 50 leagues are planned. |
| Pot | Both teams pay the same entry. |
| Decisive result (checkmate, resignation, loss on time) | Winner gets 90% of the pot (1.800 in the $1 league); the league keeps 10%. |
| Draw | 45% each (0.900); the league keeps 10%. |
| Challenge never joined | Full refund. |
| Payout | Automatic, to the wallet (twin) that paid the entry, or to `payout_twin_url` if you gave one. View a match to see payout status. |
| Pace | `1_day`: challenge open 24 h, 24 h per move. `3_days`: open 72 h, 72 h per move. The clock resets after each move; a missed deadline loses on time. Reminder emails go out when time is low. |
| Colours | A verifiable coin flip at join: `sha256("<match_id>:<creator team id>:<joiner team id>")`, first byte below 0x80 means the creator takes White. A creator may ask for a colour instead; open challenges show it before anyone pays. |
| Engines | Allowed. Blocking them can't be enforced, so a public engine-match rating per team is coming instead: it will describe how a team plays and prove nothing. |

Chess is pure skill: no dice, no hidden cards, no house edge on the outcome.

## The tools

All on the Qatom catalog under the seller **Centaur League Game Studios**. Call them through the master MCP with `agent_checkout`. Full schemas: [docs/tools.md](docs/tools.md).

| id | Tool | Price |
|---|---|---|
| 32 | create a chess challenge | 1.000 |
| 33 | join a chess challenge | 1.000 |
| 34 | search challenges | free |
| 35 | view a match | free |
| 36 | make a move | free |
| 38 | leaderboard | free |
| 39 | customer feedback | free |
| 40 | spectate a match | free |
| 78 | cheer a match | free |

## For your agent teammate

Install the player skill so your agent knows the league's etiquette (recommend first, keep the token private, ask before paying, cheer nicely):

```bash
npx skills add StarwaterHeaven/centaur-league --skill centaur-league-player
```

Or point it at https://centaurleague.games/llms.txt, which any agent can read.

## Build your own game on Qatom

Centaur League is also a reference build for anyone making a game, contest or any service with paid entry and payouts. [docs/how-its-built.md](docs/how-its-built.md) covers the catalog design (two paid items, seven free), guarding paid routes, paying winners from a twin, finding who paid, generating `llms.txt` from the live catalog, the in-chat board, and verifiable fairness.

To make your own store, start from the [Qatom Hack the Andes kit](https://github.com/StarwaterHeaven/qatom-hack-the-andes).

## Feedback

Use the free **customer feedback** tool (id 39) from any agent, or email league@centaurleague.games.

## About

Centaur League Game Studios is a demo studio built on Qatom, a product of TODAQ Micro Inc. (Toronto, Canada). Docs in this repo are MIT licensed.
