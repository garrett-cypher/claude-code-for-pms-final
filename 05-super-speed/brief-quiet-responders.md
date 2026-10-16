# Brief: a way back for responders who have gone quiet

Draft for Helen Achebe · Rook Dispatch · Sources: Glossary (4 Aug), Team directory (2 Sep), 4.2 release comments (13-28 Aug), Priya's handoff (21 Aug), `how-dispatch-decides-who-gets-pinged.md`, Helen's note. Items marked **(inference)** are not measured.

## The problem in one line
Since 4.2 (12 Aug), a responder who misses a few pings drops down the ranking, gets fewer pings, and so gets fewer chances to answer one. The score never recovers on its own (the 2019 TODO in `history.py` that Wen Li wondered about). Four responders (Vesper, The Undertow, Meteor Mite, Farlight) went from 12-14 pings a week to 3-5 and missed 33 of 56 pings (59%) after 12 Aug. Callout tickets are about 3x normal; about two thirds say "my phone never goes off."

## Who it is for
1. **Responders who have gone quiet.** They are still available and still capable. Their pings dried up after a short run of misses, and they have no way to know why or to get back in.
2. **Handlers like Kip (Meteor Mite) and Aunt Dot (Vesper).** They see a responder fall silent and can neither explain it nor fix it. They write to Nadia's team instead.

## What changes for them
- **Responder:** a run of misses no longer ends in silence. Quiet stretches count for less over time, so the responder drifts back into the offer order and gets pinged again without anyone having to ask first. A miss counts for less than an active "no", because a miss may only mean the phone was in a pocket (the code scores them the same today).
- **Handler:** the console shows which of their responders have gone quiet, with a plain reason ("pings down from about 13 a week to 3; 5 of the last 6 missed, none turned down"). They no longer find out from a complaint.
- **Both:** the first ping after a quiet spell is recognisable as a fresh start, not a continuation of the penalty.

## What it deliberately does not do
- **No revert of 4.2.** Proximity stays weighted up; wide-area responders keep what they asked for.
- **No ping-wait change in this brief.** The 90s to 60s cut is the most likely trigger **(inference, about 65-75%)**, but whether to move it is a separate decision for Helen and Marcus. This brief fixes the trap, not the trigger.
- **No guaranteed pings.** A quiet responder is not placed above better-ranked people. They only stop being ranked down for the silence itself.
- **No judging or labelling responders.** "Quiet" is a count of pings, shown to their own handler. Nothing here links a cover identity to a legal one (Security Policy 4.1), and nothing is shown across handlers.
- **No new setting.** There is no knob for a handler to tune; the behaviour is the product.

## Open before anyone builds
- Ravi's seconds-to-answer data (request not yet sent). It would show whether the four were slow answerers, and so whether this fix is enough.
- Wen Li: is the acceptance score kept only in memory? Could a deploy have reset it? Was the equal penalty for miss and turn-down a decision?
- Marcus and Helen: was the global 60s cut intended?
- Success measure, **proposed**: share of responders with at least one ping in the last 14 days, and missed-ping rate for the four. Acceptance rate alone would hide this.

## Next
A clickable mock of the handler console view and the responder's phone notification, for Helen to try.
