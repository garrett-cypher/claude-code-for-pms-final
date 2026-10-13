# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

I'm the new PM for **Rook Dispatch**, started Monday 24 Aug 2026, replacing Priya. Source of truth is the
rook-wiki connector (search_wiki → read_page / read_database). Read comments on pages; they carry decisions.

### Company and products
- Rook Industries sells coordination software to masked responders (independent, not employees) and their
  handlers/quartermasters. Subscription, priced per active responder. 241 staff, HQ Site Aleph; offices in
  Berlin, Singapore, Cornwall.
- **Rook Dispatch** (mine): availability, proximity, who gets pinged for a callout, whether they take it.
  Users: handlers (mostly in the console) and responders (phone app). Monthly release train, 4.x numbering.
- **Rook Supply**: requisitions, maintenance, failure reports. Users: handlers, quartermasters. Not mine.
- **Confidentiality is contractual.** Cover identities are never mapped to legal identities; never design
  anything that assumes we can. Read Security Policy 4.1 before touching responder records. Never try to work
  out who anyone is.

### People (Team directory, 2 Sep)
| Name | Role | Notes |
|---|---|---|
| Helen Achebe | Director of Product (my boss) | Owns roadmap and commitments; gives room |
| Marcus Oyelaran | Eng Manager, Dispatch | Straight talker; first call on engineering questions |
| Wen Li | Staff Engineer, Dispatch (Berlin) | Built the ranking logic; no doc exists, talk to her |
| Sofia Marino | Product Designer | Console and phone app; ran the September interviews |
| Ravi Menon | Data Analyst (Singapore) | Owns the real weekly numbers, incl. how often responders take pings |
| Nadia Hoffmann | Support Lead (Berlin) | Hears handler pain first; wants a standing 15 min |
| Priya Raghunathan | Prior PM, left 21 Aug | Wrote the handoff; her framing is a hypothesis, not a fact |

Wiki comments use roles, not names: "Engineering manager" = Marcus, "Staff engineer" = Wen Li, "Support lead" =
Nadia, "Product designer" = Sofia, "Director of Product" = Helen. Handlers seen in the wiki: Aunt Dot (Vesper),
Mr. Ambrose (Captain Vantage), Halloran (Sgt. Bulwark; also Supply quartermaster voice), Kip (Meteor Mite, The Gale).

### Vocabulary
- **Callout**: request for a responder to attend an incident. **Ping**: a callout offered to one responder.
  Outcomes: **taken**, **turned down**, **missed** (ping wait ran out; logged separately, both move it on).
- **Ping wait**: how long a ping sits on a phone before counting as missed; same for everyone.
- **Acceptance rate** (headline metric): taken / all pings. **Time-to-accept**: median seconds ping to taken.
- **Coverage gap**: no available responder had the needed capability tags. Different from low acceptance.
- **Routing priority**: score that ranks responders: proximity (travel-time estimate), availability, capability
  match, recent acceptance history. Turning down or missing pings lowers recent acceptance, so lowers future rank.
- **Capability tags**: flight, structural-entry, hazmat-tolerant, cold-weather, aquatic, crowd-management,
  de-escalation. **Responder Availability Record**: written by Dispatch, read by Supply.
- Supply: requisition, field failure report, service interval. **Mutual aid**: cross-area cover, unsupported, Q4 idea.

### Release history (Dispatch)
- **4.0** (7 Apr): console nav, responder profile redesign, routing-override audit log.
- **4.1** (16 Jun): proximity uses travel-time estimate; bulk callout; push delivery reliability.
- **4.2** (12 Aug): proximity weighted up vs. recent acceptance; **ping wait 90s to 60s**; console filters persist;
  three defect fixes.

### Where things stand
**The 4.2 problem (my first priority).** Callout tickets ~3x normal since ~12 Aug and flat as of 26 Aug. Split
about two thirds "my phone never goes off" to one third "gone before I could answer". A handler emailed Nadia
directly. No real weekly acceptance numbers have been pulled yet (Marcus offered only a rough pull).
- Two changes shipped together (ranking and 60s ping wait), so effects are confounded. The 60s cut plausibly
  explains the "gone before I could answer" third. Nothing yet explains "never goes off".
- Open question from Marcus (14 Aug), unanswered: does the ranking change apply to responders who have been
  turning pings down? Config does not distinguish. Wen Li was away and said to DM her.
- Priya's view: seasonal August dip, don't revert, the change was long requested. Marcus echoed "August dip" on
  26 Aug. **Untested.** Nobody has shown data. Treat as a hypothesis; get Ravi's weekly numbers, compare with
  prior Augusts, and separate ranking from ping wait before concluding. Marcus agreed to regroup after my first week.
- Reverting 4.2 is politically loaded (it served wide-area responders). Don't frame it as revert vs. keep.

**Q3 roadmap** (last reviewed 30 Jun, all owned by Priya, so stale):
| Item | Target | Status | Note |
|---|---|---|---|
| Change to who gets pinged | 4.2 | Committed | Shipped |
| Ping timeout tuning | 4.2 | Committed | Shipped as a global 90 to 60 cut (config.py, changelog); no rationale found. Ask Marcus/Helen if it was the intended tuning |
| Availability Confidence | 4.2 | Committed | Not in 4.2 notes; likely one of the squeezed-out items |
| Requisition approval chains | 4.3 | Committed | Supply; brief has heavy scope creep |
| Handler phone app | Q4 | Exploring | Sofia exploring; unstaffed |
| Shared cover between responders | Q4 | Exploring | |
Roadmap changes to committed items go through Product. Priya flagged that a conversation with Helen about which
Q3 commitments still stand has not happened. Need to have it.

**Briefs:** Bulk Callout (Jan; shipped in 4.1 though brief still says "nobody's picked it up"; no baseline for
time-to-full-coverage). Routing Override Audit Log (Mar; shipped in 4.0). Handler Phone App (8 Sep; exploring,
scoped to notifications, status view, one-tap contact; Aunt Dot's request). Requisition Approval Chains (Supply,
Jul; core ask is second approval over $2,000, everything else is scope creep).

**Other to-dos from the handoff:** write down how routing decides who gets pinged (nothing exists); console filter
tickets are noise; the 4.2 filter change will not matter much. Priya's "I made calls faster than I checked them":
look for unexamined decisions, especially in routing. Code is in `00-rook/code/dispatch-routing/`.

### Figured out from the code (session 1)
- Three candidate causes of the 4.2 drop, none yet tested: (1) proximity weight 0.45 to 0.60 pushes distant,
  reliable responders down; (2) 60s ping wait makes more misses; (3) a miss costs the same -0.12 as a
  turn-down and the acceptance score never recovers (2019 TODO in `history.py`). 4.2's lower acceptance weight
  (0.40 to 0.25) makes cause 3 weaker, not stronger. Only per-responder data from Ravi can separate them.
- The ping wait is one global value in `config.py` (`OFFER_TIMEOUT_SECONDS`); no rationale for 60s found anywhere.
- Written in `00-rook/company/notes/`: `how-dispatch-decides-who-gets-pinged.md` (routing explanation, needs
  Wen Li/Marcus review) and `draft-request-to-ravi.md` (data request, **not sent**).

### How to work with me
- Name sources and dates. Separate what the wiki says from inference. Flag stale or contradictory pages.
- Don't accept "seasonal" or "revert" as a conclusion without data. Numbers come from Ravi, not guesses.
