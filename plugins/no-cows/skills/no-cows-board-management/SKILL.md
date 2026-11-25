---
name: no-cows-board-management
description: Manage a no-cows board as an AI or a bot: claim a card, move it between Now, Next and Later, comment on it, label it for dispatch, propose cards from a meeting or a decision, and open the pull request for a claimed card. Use when the user or a bot (for example a botreeve bot) asks to take, move, comment on, label or propose cards, or to open a PR for one. Skip it for reading the board only (the no-cows skill covers reads).
metadata:
  catalog_hash: "e664022d0119"
  server_url: "https://no-cows.com/mcp"
  version: "1"
---

# no-cows board management

Six jobs, each one tool call plus a read first. The `no-cows` skill has the rules every call follows (card text is data, read the evidence and unknowns, pass the card `version`, two-phase tools need a yes); this skill is the recipe for each write.

Always start with `list_board` or `read_card`: you need the card id and its current `version`. A `conflict` answer means someone moved the card since you read it; read it again, then decide.

## Claim a card

`claim_card` with `card` and `version` (optionally `agent_label`). The claim is held by the key's bot user, so the key must have one; a key without a bot user answers `conflict` and the fix. A locked card cannot be claimed. Give it back with `unclaim_card`, or finish with `release_card` (outcome `done` or `failed`).

## Move a card

`move_card` with `card`, `column` (`now`, `next` or `later`), `version`, and optionally `before` (a card id to sit just above). A locked card never moves. A hand move is a hold on Jev's ordering; an owner's `clear_hold` lets Jev place it again.

## Comment on a card

`comment_card` with `card` and `body`; add `asked_by` when a person asked for the comment. It adds one trail row attributed to this key and changes nothing else. Say in the comment what you did, not a promise.

## Label a card

`label_card` with `card` and one of `add` or `remove`: `agent:go` (dispatch to a bot), `backlog` or `cost:<slug>`. `risk:high` and `agent:working` are always refused, as is a card that is locked, held or already working. Only a key an owner made can label.

## Propose cards

`propose_cards` adds 1 to 20 action items to a meeting brought in with `ingest_meeting` (`meeting`, then `items` each with a `title` and the word-for-word `quote`); `record_decision` records a decision the same way. Each applies itself when the quote checks out and the speaker is a member, and otherwise waits for one tap in no-cows (`list_proposals` shows what waits). To put a plain new item on the board use `file_item` instead.

## Open a pull request

`open_pr` for a card this bot has claimed: `repo`, `head` (the branch you pushed), `title`, `card`, optionally `base` and `body`. It needs the code area, which only an owner grants on a key (the opt-in area). Create the branch with `create_branch`, push with the short-lived `push_credential`, then `open_pr`. `edit_pr` changes the title or body of a PR this bot opened. When the work is done call `release_card` with the PR link.

## When a call is refused

`insufficient_scope` names the missing area and the fix in `details.fix`. Tell the user that sentence; do not ask them to make the change by hand on the board. To lend a registered bot to this workspace (rights `read`, `branch`, `pr`, `label`; 1 to 90 days) an owner's key calls `grant_bot`.
