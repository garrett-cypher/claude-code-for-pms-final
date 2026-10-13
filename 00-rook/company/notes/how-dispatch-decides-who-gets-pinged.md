# How Dispatch decides who gets pinged

Draft for review by Wen Li and Marcus Oyelaran. Written from the code in `00-rook/code/dispatch-routing/` (as of 4.2) and the wiki Glossary. Priya's handoff asked for this; no prior write-up existed. Items marked **Confirm** are inferences.

## The flow
1. A callout arrives for a region and time. `availability.available_for()` returns responders who are free.
2. `routing.rank_for_callout()` scores each one and sorts high to low. Nobody is removed; ranking sets order only.
3. `offer.dispatch()` offers the callout to the top responder, waits, then moves down the list one at a time until someone takes it (or the list ends with no taker).

## The score
`score = W_prox x proximity + W_accept x recent acceptance + W_cap x capability match`

| Input | How it is computed | Weight before 4.2 | Weight in 4.2 |
|---|---|---|---|
| Proximity | 1 - (travel minutes / 45); 0 at 45+ min. Travel-time estimate since 4.1 | 0.45 | **0.60** |
| Recent acceptance | Running score, 0 to 1, starts at 0.5 | 0.40 | **0.25** |
| Capability match | Share of required tags the responder has (1.0 if none required) | 0.15 | 0.15 |

Weights are relative, not required to sum to 1. All values live in `config.py`, which says not to change them without telling the engineering manager.

## The recent-acceptance score
- Taken: +0.08. Turned down: -0.12. **Missed (ping wait ran out): also -0.12**, same as turned down.
- Clamped to 0 to 1. Held in memory per responder name in `history.py`. **Confirm** how it persists in production.
- It never decays. A 2019 TODO in `history.py` asks whether it should ease back toward 0.5; nobody built that. A responder who is ranked low gets fewer pings and so fewer chances to earn credit back. **Confirm** whether this matters in practice.
- Net effect: it takes about 1.5 accepted pings to undo one decline or miss.

## The ping wait
`OFFER_TIMEOUT_SECONDS` = 60 (90 before 4.2). One global value for every responder and callout. Unanswered offers are withdrawn and recorded as missed.

## What 4.2 changed (12 Aug 2026)
- Proximity weight 0.45 to 0.60; acceptance weight 0.40 to 0.25.
- Ping wait 90s to 60s.

Worked example (capability equal; A is 20 min away with acceptance 0.2, B is 40 min away with acceptance 0.9):
- Before: A 0.48, B 0.56, so B was asked first.
- After: A 0.53, B 0.44, so A is asked first.
That is the intended effect: nearby responders are no longer skipped for a distant one with a better record. The same change pushes distant responders down regardless of how reliable they are.

## Known gaps and open questions
- No distinction between a responder who turns pings down and one who simply didn't see the ping (Marcus, 14 Aug). Missed and turned-down are recorded separately in reporting but scored identically.
- A shorter ping wait produces more misses, and each miss lowers the responder's score.
- No data yet showing which of the 4.2 changes explains the post-release drop in taken pings; see the data request to Ravi.
- Not covered here: `availability.py` internals, `capability` tag management, mutual aid (unsupported).
