# A/B Experiment Brief, StreamLine (B2C)

## Parameters
| Parameter | Decision |
|---|---|
| Feature under test | Spotlight rail on Home |
| Persona | Wanderers |
| Expected outcome | Wanderers find something to play faster, so fewer of their sessions end without a play. |
| Primary success metric | Play-start rate per Wanderer session: % of sessions with a play of at least 2 minutes (excludes accidental taps) |
| Baseline rate | 29% |
| Guardrail metric | Month 1 retention |
| Guardrail boundary | Lower bound of the 95% CI on the difference vs. control stays above −3 pts (assumed baseline 63-68%, from Sep-Nov all-user cohorts) |
| Second guardrail | · |
| Minimum Detectable Effect | 2.9 |
| Sample size per arm | 3950 |
| Traffic split | 50/50 |
| Test duration | 4 weeks |
| Significance threshold | p<0.05 |

## Control vs. Variant
- **Control (A):** Wanderer Home today with the Spotlight rail removed (holdout): StreamLine header with profile avatar, Continue Watching row, existing browse rows [list in order], static bottom nav (Home, Search, My List). No autoplay trailers.
- **Variant (B):** Control Home plus one Spotlight rail ("Picked by our curators") with 8 tiles (poster, title, year · runtime), placed directly below Continue Watching. Continue Watching keeps its control position. No "Why?" chips. No autoplay. No changes to any other row, the nav, or the header.
- **Held constant (isolation check):** Header and avatar, Continue Watching row and position, all existing rows and their order, bottom nav (Home, Search, My List), search, My List, autoplay (off), recommendation logic for existing rows, notifications, pricing. Only difference: presence of the Spotlight rail.

## Hypothesis
> I believe that Spotlight rail on Home for Wanderers will result in Wanderers find something to play faster, so fewer of their sessions end without a play., as measured by a 2.9 change in Play-start rate per Wanderer session: % of sessions with a play of at least 2 minutes (excludes accidental taps) within 4 weeks. We will protect Month 1 retention throughout the test.

## Shipping criteria
> We will **ship** if Play-start rate per Wanderer session: % of sessions with a play of at least 2 minutes (excludes accidental taps) improves by ≥ 2.9 at p<0.05 and Month 1 retention does not reach Lower bound of the 95% CI on the difference vs. control stays above −3 pts (assumed baseline 63-68%, from Sep-Nov all-user cohorts) after 4 weeks.
> We will **iterate** if direction is positive but lift is below the MDE.
> We will **kill** if the primary metric shows no improvement or moves negatively.
> The read date is fixed at the end of 4 weeks, no results reviewed before this date.
