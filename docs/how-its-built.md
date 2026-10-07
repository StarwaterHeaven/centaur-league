# How Centaur League is built on Qatom

Centaur League is a small service (a Cloudflare Worker with a D1 database, a cron every five minutes, and email through Resend) sold entirely through the Qatom catalog. These are the patterns worth copying for any game, contest or paid service with payouts.

## 1. Two paid items, seven free ones

| Paid (1.000 USD-TDN) | Free (price 0) |
|---|---|
| create a challenge, join a challenge | search challenges, view a match, make a move, leaderboard, spectate, cheer, customer feedback |

Money changes hands only at entry. Everything else is free, so agents explore, watch and play without friction, and the paid moment is clear. A Qatom catalog item priced at 0 is forwarded with no payment, so free and paid tools live side by side in one catalog and one connection.

Tip: give every item the same name prefix (`Centaur League: ...`) so `catalog_search` groups them.

## 2. Guarding the paid routes

Qatom calls a paid item's endpoint server to server after the payment settles. The paid routes answer only at a secret path:

```
POST https://centaurleague.games/api/paid/<SERVICE_PATH>/create
POST https://centaurleague.games/api/paid/<SERVICE_PATH>/join
```

Any other path returns 404. The secret lives in the stored endpoint URL of the catalog item (buyers never see it) and in a Worker secret. Rotate it if it ever leaks. An optional allow-list of Qatom's egress addresses adds a second check.

The league tier is pinned the same way: `?entry=10` in the stored URL selects the $10 league, never a buyer argument.

## 3. Arguments and validation

Only properties declared in an item's input schema are forwarded, and undeclared ones are rejected, so the schemas are exact and use `additionalProperties: false`. POST arguments arrive as a JSON body; the Worker also accepts GET query parameters.

When the endpoint rejects a call (a missing input, a match no longer open, a move out of turn), the buyer may see a generic payment error from Qatom, so `llms.txt` lists the common causes.

## 4. Paying the winner

1. **Who paid?** Each paid item has a commodity hash. `GET /v4/commodity/{hash}/transactions` lists its payments; the league matches the entry by amount and time to find the paying twin, and records it as the team's payout address (or uses `payout_twin_url` if the team gave one).
2. **Pay out.** `POST /v4/transfer` with `address` (the twin URL) and `amount`. Transfers spend from the account's **primary** twin, while entries land on the items' twins, so funds are moved to the primary (Qatom auto-distribution, a few times a day) before payouts.
3. **Queue and retry.** A payout that cannot be paid yet is queued; the cron retries it, the team sees `payouts[].status` in view a match, and gets an email when it lands.

Server-side calls use API client credentials for the account (token from `/v4/account/oauth/token`). Ask the Qatom team for them.

## 5. A read-me that cannot drift

`/llms.txt` is generated from the live Qatom catalog (`GET https://pay.m.todaq.net/v4/catalog/items`) every ten minutes: worked examples with the real item ids, the catalog table, every input schema, the rules, and the error causes. Nothing is hand-maintained. Agents that find one item can read the whole storefront.

## 6. A board for every client

- **Everywhere:** view a match returns the position as FEN, a text board, a PNG image rendered in the Worker, and a link to a web board where the human drags pieces.
- **MCP Apps clients:** a second, free MCP server (`/mcp`) serves an interactive board inside the chat. Moves made on it are reported back to the agent, so it can recommend the reply. Hosts cache app views by URI, so the view's URI carries a version suffix that changes on every board update.

## 7. Fairness you can check

Colours are decided at join by `sha256("<match_id>:<creator team id>:<joiner team id>")`: first byte below 0x80, the creator takes White. The input and the hash are published with the match. The server validates every move, handles draws by rule (threefold repetition, fifty moves, stalemate, insufficient material) and forfeits on missed deadlines.

Engines can't be detected reliably, so they are allowed. A public engine-match score (agreement with a fixed-depth engine, and average centipawn loss) is being added per team, to describe play rather than police it.

## 8. Agents that behave

The item descriptions and `llms.txt` set the agent's default: look at the position, recommend a move to the human, wait, and play only when asked. Cheers are positive by construction: they are assembled from fixed vocabularies, so there is nothing to moderate.

## Make your own

Start from the [Qatom Hack the Andes kit](https://github.com/StarwaterHeaven/qatom-hack-the-andes): `template/` gives you the catalog file, validation, the secret paid route, the feedback item and `/llms.txt`. Add your game's state (D1 or KV), a cron for deadlines, and the payout steps above.
