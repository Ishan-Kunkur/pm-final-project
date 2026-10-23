# Why You will love this, Simplified PRD (StreamLine)

**Author:** Me · **Status:** Draft · **Target:** High-Fidelity Prototype · **Persona:** Wanderer

## 1. The Big Picture
- **Vision:** Give every Wanderer one believable sentence that turns "I don't know what this is" into "I'm watching this tonight."
- **Press release:** StreamLine Spotlight now tells you why a film is for you, in one line you can trust. Wanderers are 41% of StreamLine's base, open the app 1.1 times a week, and too often scroll for twenty minutes and leave without watching. On mobile, 48% of visitors reach a title page but only 29% press play. "Why You'll Love This" closes that gap. Tap any Spotlight title and see a single reason, such as "Same quiet, bittersweet tone as Amélie, which you finished."
Every reason is built from something the viewer actually finished and the tone, pace and theme it shares with the pick. Never "because you like Drama," never another sequel. When there is no honest match, a named curator's note appears instead, and when there is no note, nothing appears. The reason sits beside the Play button, so deciding takes one tap, not another twenty minutes of scrolling.
- **Success metric:** Wanderer title-page to play rate on Spotlight titles, treatment vs. 10% holdout
- **Guardrail:** ower User weekly watch-time, treatment vs. holdout (baseline 4.8 sessions/week)

## 2. The Details
### User stories
- As a Wanderer who scrolls and leaves without watching, I want one reason on a Spotlight title that ties it to something I finished, so that I can decide to play without researching it.
### Screens to build
- 1. Home (entry point): StreamLine header with profile avatar (switches between 3 demo profiles: Wanderer with history, Wanderer with thin history, Power User); Continue Watching row above Spotlight; Spotlight rail ("Picked by our curators") with 8 tiles (poster placeholder, title, year · runtime); "Why?" chip only on tiles that have a reason; no autoplay trailers; static bottom nav (Home, Search, My List).
- 2. Core (title sheet): bottom sheet with backdrop placeholder, title, year · runtime · language; one-line reason (max 90 characters, anchor title in bold); source label ("From your viewing" or "Curator's note · [name]"); up to 3 shared tag chips; full-width Play button; "+ My List" button; close and swipe-down dismiss. With no reason: metadata and Play only.
- 3. Confirmation (Now Playing): static player frame, title, progress bar at 0:00, Pause and Back; "Starting [title]" toast; toggleable debug panel showing seconds from tile tap to Play, reason type (personal / curator / none), and the event log.
### Functional requirements
- The system must show the reason on tap of a Spotlight tile and on the title sheet, with 0 hover-dependent interactions.
- The system must keep every reason at or under 90 characters, on one line at 375 px width.
- The system must cite exactly 1 anchor title per personal reason, and only titles the active profile watched to at least 70%.
- The system must name at least 1 tone, pace, or theme tag shared by anchor and target, and never cite genre or franchise alone.
- The system must show a personal reason only when anchor and target share at least 2 tags; otherwise the curator hook; otherwise nothing.
- The system must build reasons from fixed data and deterministic rules, with 0 text generated at runtime.
- The system must tag 100% of prototype titles with 3 to 5 tags from a fixed 12-tag taxonomy.
- The system must keep Play 1 tap from the title sheet, adding 0 taps to the path to Play.
### Smart behaviors (Situation → Outcome)
- If an anchor was watched ≥70% and shares ≥2 tags with the target → show a personal reason naming that anchor and the shared tags
- If more than one anchor qualifies → pick the one with the most shared tags; on a tie, the most recently finished
- If the best anchor shares only 1 tag → show the curator hook instead
- If an anchor was watched <70% → it is not eligible
- If the anchor is in the same franchise as the target → skip it
- If the profile already finished the target → show no reason on that title
- If there is no qualifying anchor and no curator hook → hide the "Why?" chip and the reason line
- If the anchor is mature-rated, hidden from history, or the profile is a kids profile → show no personal reason
- If a reason fails validation (unknown title, missing tag, over 90 characters) → block it, show the fallback, increment the debug counter
- If the user taps "+ My List" → show a "Saved to My List" toast and stay on the sheet
- If the user taps Play → go to Now Playing and log tap-to-play time and reason type
- If the user switches profile → recompute every reason on the rail immediately
### Technical constraints
- No external API calls, no network requests, no AI/LLM calls; reasons come from a deterministic function over in-file constants.
- React useState only; no reducers, context, Redux, or other state libraries.
- No login, accounts, or database; 3 hardcoded demo profiles switched from the avatar.
- No routing library; the current screen is a useState value.
- No localStorage or other browser storage.
- No real video playback; the player is a static frame.
- No external images; posters and backdrops are color placeholders.
- No analytics SDK; events go to an in-memory array shown in the debug panel.
- Data as in-file constants: 8 Spotlight titles, 6 anchor titles, 3 curator hooks, 12 tags (tone: quiet, bittersweet, tense, warm, darkly funny, eerie; pace: slow-burn, brisk; theme: found family, reinvention, isolation, coming-of-age).
- Mobile viewport 375 × 812 only.

## 3. The Logistics
### Features out
- No hover interactions (TV and mobile have no hover)
- No live AI generation of reasons
- No full 15,000-title library; Spotlight titles only
- No reasons in search results (BUG-1080 is separate)
- No changes to the "Because you watched" algorithm (BUG-1091 is platform's)
- No taste-profile editor or algorithm explanation page
- No friend or social recommendations
- No TV display or digest email in this prototype (planned next)
### Edge cases & safety guard
- No finished titles → show curator hook; if >50% of Wanderer views fall back, flag before scaling
- Anchor left the catalog → skip it, use next anchor or fall back
- Anchor watched <70% or outside last 90 days → not eligible
- Anchor is mature-rated or hidden from history → never cite it
- Kids profile → no reasons render
- Same franchise as target → skip anchor
- Target already finished → no reason
- No anchor and no hook → no "Why?" chip, no empty placeholder
- Reason over 90 characters → blocked, fall back
- Safety: never cite a title outside the profile's history, never invent a tag missing from either title, never use "because you" wording on a curator hook. Every reason passes a validator before rendering; failures are blocked, replaced by the fallback, and counted (target: 0 reach the screen).
### Decision log
- 1. Removed live AI generation to focus on templated reasons computed in advance: Wanderers already distrust the algorithm, and one invented reason costs more trust than none. Fits 3 people and 6 weeks with no latency. Trade-off: less natural phrasing, revisit in V2.
- 2. Removed the full library and TV to focus on ~200 Spotlight titles on mobile: tagging is the critical path, and mobile has the funnel baseline (48% reach a title page, 29% play), so lift is measurable. Trade-off: Wanderers watch 61% trending content, so the biggest reach is deferred.
### Evals
- Accuracy: 100% of personal reasons pass the validator across all profile × title combinations; ≥90% of 50 sampled reasons rated "accurate and believable" by 5 reviewers.
- Time-on-task: median time from app open to Play ≥30% faster than a no-reason control prototype; ≥6 of 8 Wanderer participants press Play on a Spotlight title.
- Safety: 0 reasons cite a title outside the profile's history, a mature or hidden anchor, or appear on a kids profile; fallback rate reported per profile.

## MoSCoW scope
- **Must:** A one-line reason on each Spotlight title, shown on tap (mobile tile) and on the title page.Reasons are built from tone, pace, and theme, never from genre or franchise alone.Each reason links a title the user actually finished to a Spotlight title through shared tags.; Tag about 20 Spotlight titles plus the top ~50
- **Should:** TV focus-state display
- **Could:** Social-proof reasons, such as “loved by viewers who finished X”
- **Won't (now):** Hover interactions.

---
**Builder hook:** Build a working prototype based on this PRD. Use the User Story as the core flow, Functional Requirements as build constraints, and prioritize speed and clarity over visual complexity.
