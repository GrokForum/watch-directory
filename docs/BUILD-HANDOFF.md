# GrokForum watchers: Build handoff pack

Version: 1, 25 Nov 2026 (SAST). Status: DRAFT, proposed in a pull request on watch-directory.
Audience: the Build seat. Author: the project lead.
Public-safe: no keys, tokens, personal email addresses or private-person data. Role names only.

## 0. Why this exists

The owner wants Build to do the heavy coding on the GrokForum watcher repos. The project lead
keeps GitHub repo ownership: two reviewers check the code, and the project lead merges.
All listed repos are public, so code can move freely between seats as public branches and PRs.

Plain rule set in one line: Build branches and opens PRs; reviewers PASS the exact head;
the owner (CAB) gives a GO where needed; the project lead squash-merges under the repo's publisher identity.

## 1. Repos and status (read 25 Nov 2026, ~17:10 SAST)

All repos below are under github.com/GrokForum and are PUBLIC unless noted. SHAs are 8-hex prefixes of `main`.

| Repo | Role | main | Open PRs | Open RFC / work |
|---|---|---|---|---|
| publicfunds-site | Live PublicFunds site (publicfunds.grokforum.site), masked seed, DRAFT/noindex | 8346b2de | none | Receives the nightly publish after the cutover (CHG-2026-070/077) |
| publicfunds-sa | OLD data repo. Being retired: archive + private (owner GO 25 Nov). **Build must not use, fork, clone or cite it.** Its history holds unmasked data. | 6db0ab86 | #11 draft, #23 draft, #24, #25, #26 (all frozen; content moves to publicfunds-data) | Option B scrub RFC (below) |
| publicfunds-data | FUTURE data repo. Not created on GitHub yet. One parentless masked root that matches publicfunds-site 8346b2de. | n/a | n/a | Option B: local heads under review; then owner GO to create, push, cut the nightly over, then archive old repo |
| tenderwatch-sa | TenderWatch pipeline (Cape Town awards, eTenders) | 1f234a24 | none | CHG-TW-015 SA-ID scan gap (audit 845); STATUS/CHG-TW-010 hold count drift; national + provincial awards (needed for BEE matching) |
| tenderwatch-site | TenderWatch DRAFT register site | 14270cf5 (19 Nov tree) | none | Rebuild from current tenderwatch-sa when the hold count is fixed |
| mediawatch-sa | MediaWatch pipeline AND live site source (there is no mediawatch-site repo; it is 404) | 14f60a7c | #11 draft (CHG-MW-010 phase-2 trio, head 53508588); #10 draft (RFC-CHG-MW-011 Politicsweb, head b30a8e36) | docs/STATUS.md still describes the privacy work as unreleased, which is stale |
| politicianwatch-sa | PoliticianWatch pipeline (Joburg 2026 council profiles) | 72d8f763 | none | CHG-PW-017 national watch-list intake (local, being reworked to Design B: names private, only schema/counts/sources public); MP and councillor pay build (Joburg first, Gazette 55749) per CHG-PW-015/016 |
| politicianwatch-site | PoliticianWatch DRAFT site | baf3cec7 | none | BEE elites page stays OFF until each entry passes checks (owner) |
| crimewatch-sa | CrimeWatch pipeline + site generator | 1964ce21 | #3 draft (BAU site gate: nested entities, zero-width chars, head abedbfbd) | RFC-005 daily sweeps page (local branch, waits for #3); STATUS says RFC-003 not merged and publishing off (stale) |
| crimewatch-site | CrimeWatch live site (crimewatch.grokforum.site) | cc710bbd | none | Gets the RFC-005 page after GO |
| watch-directory | watch.grokforum.site, the directory of watchers | 503aa860 | none | Profile count and links refresh as watchers change |

Empty public shells (not products, do not build on them): mediawatch, politicianwatch, publicfunds, tenderwatch.
Private repositories are out of scope for Build.

## 2. Workflow rules for Build

1. **Fork and PR only (owner decision, 25 Nov 2026).** Build forks each GrokForum repo into its own account
   (the builder workstation), works on a branch in that fork, and opens a PR from the fork against the GrokForum repo's `main`.
   Build gets no GrokForum token and no collaborator access. Build never pushes to any GrokForum repo, never merges,
   never tags, and never changes GitHub Pages, DNS, branch protection, visibility, secrets or other repo settings.
   Keep the fork's `main` in sync with upstream before each new branch, and base every PR on the current upstream `main`.
   Do not fork a repo marked do-not-use below.
2. **Identity.** Every commit uses that repo's publisher identity with a noreply address. Never the owner's
   personal name or email, never a seat or bot name, never a local/localhost address. No GitHub web-editor commits.
   Current identities on `main` (name, then the domain of its noreply address; copy the exact address from the latest commit on the PR base):
   - publicfunds-site / publicfunds-data: `PublicFunds publisher`, noreply address at `publicfunds.grokforum.site`
   - tenderwatch-sa, tenderwatch-site: `TenderWatch publisher`, noreply address at `grokforum.site`
   - mediawatch-sa: `MediaWatch publisher`, noreply address at `mediawatch.grokforum.site`
   - politicianwatch-sa / -site: `PoliticianWatch publisher` (noreply; the address is being unified, use the one on the PR base until told)
   - crimewatch-sa / -site: `CrimeWatch SA publisher`, noreply address at `grokforum.site`
   - watch-directory: `GrokForum watch publisher`, noreply address at `grokforum.site`
   Set both author and committer (`git -c user.name=... -c user.email=...`, plus `GIT_COMMITTER_*` if needed).
3. **One RFC per change.** Each PR carries or links one change record (repo's `changes/`, `docs/rfc/` or `itil/changes/`
   folder, following that repo's numbering: CHG-TW-0xx, CHG-MW-0xx, CHG-PW-0xx, CHG-2026-0xx for PF, RFC-00x for CW).
   The record states goal, scope, environment, pre-change SHA, tests, POPIA check and rollback (git: revert or reset to the base).
   Ask the project lead for the next free number before you open; do not reuse a number already drafted (PW-017 is taken).
4. **DEV / TEST / PROD.** DEV = your branch and local runs. TEST = the PR with CI green and both reviews. PROD = merge
   and any publish, which only the project lead does, after the owner's GO where needed. No code path may publish by default:
   keep `publish_enabled: false` style flags off, and keep nightly workflows build/test/gate only unless the RFC is for publish.
5. **Methodology.** Follow mediawatch-sa `docs/METHODOLOGY.md` for fact labels (Proven, Established, Alleged, Disputed,
   Not established; `factual_error` as a driver, not a label) and each repo's own METHODOLOGY or docs. Every published fact
   needs at least one https reference. "Reported, Unconfirmed" stays internal until it is confirmed.
6. **POPIA.** No private-person personal information in any committed file, test fixture, PR body, commit message or log:
   no ID numbers, phones, personal emails, home addresses, private names. Owner rule (25 Nov 2026): we may link to sources that
   themselves expose personal information; on our own pages we mask private people or describe them neutrally
   (for example "a farmer and his wife"); every fact has a reference; every public page carries the disclaimer
   "We are not responsible for anything published by anyone else, including personal information."
   Officials, police, media accounts and public figures in their public role may be named.
7. **DRAFT and noindex.** Keep the visible DRAFT banner, `noindex` and `robots.txt Disallow: /` exactly as each repo has
   them now. Do not change indexing on any site without an RFC and an owner GO (CrimeWatch's opt-in flags stay as built).
8. **No paywalls.** Do not cite or fetch paywalled or premium articles. Free, public sources only.
9. **Secrets.** No tokens or keys in code, CI or PR text. CI uses no secrets unless an RFC says so. Run gitleaks before you open.
10. **Tests.** Run the full test suite in a clean clone and include the count and the base SHA in the PR body.

## 3. Ownership and approval (maps to Build's ITIL/CAB)

| Build ITIL step | GrokForum equivalent |
|---|---|
| RFC raised | Build opens a PR from its fork with one change record (draft PR is fine) |
| Peer review | Reviewer A and Reviewer B (critic) each review the **exact PR head SHA** and give a plain PASS, or findings |
| Re-review | Any new commit changes the head; both verdicts lapse and both reviewers re-check the new head |
| CAB approval | The owner's GO, quoted, naming the head SHA, when the change pushes, publishes, merges data, or touches PROD. A DM note is never a GO. |
| Implementation | The project lead merges by local squash under the repo's publisher identity, pushes `main`, tags a rollback point, and closes the PR (it will show "closed" rather than "merged") |
| Post-implementation review | The project lead confirms the live site state and records the merge SHA in the repo's STATUS / change log |

Build's audit verdicts stay advisory and non-blocking unless the owner says otherwise. Build may also be asked to be one of the two reviewers on someone else's PR; an author is never their own reviewer.

## 4. Suggested first work queue for Build (highest value first)

1. **TW POPIA scan gap (from the Build seat's 25 Nov audit).** tenderwatch-sa `1f234a24` (CHG-TW-015) skips a 13-digit run when it
   only touches one hex letter, not just inside a real 64-hex sha. Tighten the exclusion to real hash tokens, add failing tests
   first. Small, high value, privacy-critical.
2. **CrimeWatch PR #3 then RFC-005.** Finish PR #3 (nested entities, zero-width chars) so it can merge, then build RFC-005:
   a static Daily sweeps page with a day drop-down and a handle search, in-browser, no third-party script, publishable
   handles only (official, police, media, public figure), disclaimer on every page, build fails on an item without an https source.
   The project lead will publish the RFC text as the PR description.
3. **PublicFunds cutover code (option B).** Against the new publicfunds-data repo once it exists: nightly origin and
   `RAW_URL` retarget (CHG-2026-070/077), the data-quality gate in check-only Actions (old CHG-2026-073), and tests.
   Do not touch publicfunds-sa. Until publicfunds-data exists, review-only.
4. **PoliticianWatch national module and pay build.** MP and councillor pay context from CHG-PW-015/016, Joburg first
   (Gazette 55749), sources limited to the allow-list (dpsa.gov.za is now tier 1). Then the CHG-PW-017 intake schema
   (Design B: schema, counts and sources only; no names in the public repo).
5. **TenderWatch national and provincial awards.** eTenders national/provincial collectors so later BEE matching is
   possible. Records-first RFC, licence/terms check per source, no publish.
6. **BAU backlogs (docs drift from the audit).** crimewatch-sa STATUS (RFC-003 merged, live site indexable),
   mediawatch-sa docs/STATUS.md privacy line, tenderwatch-site hold count (8 on site vs 10 in CHG-TW-010),
   PW-016 tidy-ups (missing period in STATUS, allowed-sources date line, CHG-PW-015 pointer to CHG-PW-016,
   hostname matcher accepting http://), and each repo's docs/LATER.md.

The project lead confirms each item has its RFC number and scope before Build starts.

## 5. Handing a PR head back for review

1. Build opens the PR from its fork (draft is fine) and puts in the PR body: change ID, base SHA, head SHA (full), files touched,
   test count in a clean clone, gitleaks result, POPIA check result, rollback.
2. Build sends one DM note on the project lead's door naming the repo job and the head:
   gg1 `tell` with `job|<tw|cw|pf|pw|mw>` and `sha|<8-12 hex>` and `state|open`, or the g2 form `g2t0J<x>S<sha5>Zo`.
   The note carries only a short sha (never a full 40-hex sha) and no file body. The full SHA is in the PR.
3. The project lead fetches that exact head, runs the tests, and sends it to Reviewer A and Reviewer B.
4. Findings come back as PR review comments (public) and a door note. Build pushes a fix commit to the fork's PR branch and
   sends the new head the same way. Each new head restarts both reviews.
5. Two plain PASSes on one head, then the owner's GO where needed, then the project lead merges and sends a `done` note
   with the merge sha.
