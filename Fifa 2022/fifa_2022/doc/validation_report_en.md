# Validation report — StatsBomb World Cup 2022 dataset

Dataset produced on 27 September 2026. Source: **StatsBomb Open Data**, competition 43, season 106. Repository version: `4b73468fc5b0f1950f9f66fada70ad3a4f9327cb`.

## Dataset summary

- **1,995 player × match rows**, **680 players**, **32 teams**, **64 matches**, **38 variables**.
- 48 group matches, 8 round-of-16 matches, 4 quarter-finals, 2 semi-finals, 1 third-place match, and 1 final.
- 1,408 starts and 587 substitute appearances. The 1,249 team-sheet entries without participation are excluded.
- Players who entered without recording a personal action are retained: inclusion does not depend on recording a pass or shot.
- No duplicate player–match keys and no missing cells in the final CSV.

## Sources inspected

One `matches/43/106.json` file, **64 lineup files**, and **64 event files** were downloaded at the same repository version. Official specifications were consulted for fields, units, action outcomes, and periods. Raw sources were retained locally with SHA-256 hashes during preparation.

- [Match data at the version used](https://github.com/statsbomb/open-data/blob/4b73468fc5b0f1950f9f66fada70ad3a4f9327cb/data/matches/43/106.json)
- [Official events](https://github.com/statsbomb/open-data/tree/4b73468fc5b0f1950f9f66fada70ad3a4f9327cb/data/events)
- [Official lineups](https://github.com/statsbomb/open-data/tree/4b73468fc5b0f1950f9f66fada70ad3a4f9327cb/data/lineups)
- [Official specifications](https://github.com/statsbomb/open-data/tree/4b73468fc5b0f1950f9f66fada70ad3a4f9327cb/doc)
- [Repository attribution requirements and terms](https://github.com/statsbomb/open-data/tree/4b73468fc5b0f1950f9f66fada70ad3a4f9327cb) and [source licence](https://github.com/statsbomb/open-data/blob/4b73468fc5b0f1950f9f66fada70ad3a4f9327cb/LICENSE.pdf). Shared analyses must credit StatsBomb and comply with its attribution requirements, including the logo specified in the repository.

## Checks performed

| Check | Result |
|---|---|
| File coverage | All 64 matches present in matches, events, and lineups. |
| Events | 234,637 source events, with unique identifiers within and across matches. |
| Scope | 234,531 events in periods 1–4 retained. |
| Penalty shootouts | 106 events excluded, including 41 shots and 26 successful kicks, across 5 matches. |
| Extra time | 5 matches include periods 3 and 4; these are included in statistics and minutes. |
| Team sheets | Every included player belongs to their team's lineup. The set of participating players matches the set of players with at least one position interval in the lineups. |
| Entries and exits | 11 starters per team; 587 substitutions with the departing player already participating and the incoming player not previously participating; no negative intervals. |
| Dismissals | 3 dismissals before the end of play accounted for. Dumfries's card in period 5 does not affect his minutes. |
| Period endings | Both Half End events for each period have the same timestamp; no event in that period occurs later. Each period ending is counted only once when calculating duration. |
| Team minutes | For all 128 team × match combinations: sum of player minutes = 11 × match duration − minutes lost through dismissals. Agreement before rounding is within 0.0000001 minute. |
| Player participation | No action included in the statistics occurs outside the player's entry–exit interval, allowing a tolerance of 0.05 seconds. |
| Scores | For all 128 team × match combinations: goals by shooters + opponents' own goals = score in the matches file. |
| Assists | All 110 passes marked goal_assist refer to a goal-scoring shot for the same team. |
| Required data | Length available for every pass; start/end coordinates available for every carry; xG available and between 0 and 1 for every shot. |
| Numerical relationships | Completed passes ≤ passes; completed dribbles ≤ dribbles; goals ≤ shots on target ≤ shots; 0 ≤ cumulative xG ≤ shots. |
| Second aggregation | All 19 action measures were recalculated separately using event labels and compared for each of the 1,995 rows, within 0.000001 for rounded values. |
| Export | The saved CSV was read back: dimensions, accented names, identifiers, context, and values were compared with the prepared rows. |

## Calculating minutes played

Event `timestamp` values are relative to each period. A continuous timeline is constructed by adding the actual durations of preceding periods, each ending at `Half End`. Starters begin at zero. A substitution closes the departing player's interval and opens the incoming player's interval at the same instant. A red card or second yellow card ends the player's participation. Remaining players finish at the last `Half End` in period 2 or 4.

A substitution during an interval therefore includes the previous period's stoppage time without including the interval itself. Simply subtracting the nominal entry minute from the nominal exit minute would be incorrect because the nominal clock restarts at 45, 90, and 105 minutes.

The variable measures **participation time, including stoppages**, rather than ball-in-play time. The 74 temporary `Player Off` events, accompanied by 74 `Player On` events, are not subtracted. None of these departures is marked permanent. Minutes depend on when StatsBomb timestamps the substitution or penalised action; they are not a continuous physical tracking measurement.

Observed minimum: **1.286833 min**. Maximum: **141.410167 min**, for players who played the entire final. This maximum includes stoppage time in all four periods.

Final example: 52:34.569 + 53:37.921 + 16:04.648 + 19:07.472 = **141.410167 minutes**. Mbappé has 3 goals and Messi 2 in the CSV; their successful shootout kicks are not added. Axel Disasi has 3.527433 minutes and zero action counts: his row is retained.

## Lineup discrepancies and how they were handled

**34 participation-boundary discrepancies exceeding 1.01 seconds** were identified by comparing the first/last lineup intervals with events. Some intervals are incomplete or inconsistently ordered, particularly around breaks and extra time. For example, Lucas Hernández has a lineup interval extending to the final whistle despite a substitution event; Abdessamad Ezzalzouli's lineup begins very late despite his earlier entry.

These intervals were therefore not used to calculate minutes. Entry, exit, and dismissal events determine participation boundaries for all players, subject to the team-size checks above. No time was imputed from an average or a theoretical 90/120-minute duration.

For substitutes' positions, the first personal event recording a position after entry is preferred to the first lineup interval. Two players have no such event: **Kristijan Jakić (3869684)** and **Axel Disasi (3869685)**. Their positions come from the lineup. Starters' positions come from `Starting XI`.

The differences below are calculated as “lineup boundary − event boundary”, in seconds. They are recorded for traceability; the CSV uses events.

| Match | Player | Boundary | Difference (s) |
|---|---|---|---:|
| 3857279 | Lucas Hernández Pi | Exit | 5485.363 |
| 3857273 | Wayne Hennessey | Exit | 1105.250 |
| 3857278 | Sardar Azmoun | Exit | 3293.502 |
| 3857278 | Ali Karimi | Entry | 303.484 |
| 3857280 | Vincent Paté Aboubakar | Exit | 431.883 |
| 3869219 | Hidemasa Morita | Exit | 960.131 |
| 3869219 | Ivan Perišić | Exit | 960.131 |
| 3869219 | Ante Budimir | Exit | 959.977 |
| 3869220 | Azzedine Ounahi | Exit | 213.105 |
| 3869220 | Abdessamad Ezzalzouli | Entry | 3665.476 |
| 3869220 | Nicholas Williams Arthuer | Exit | 309.459 |
| 3869220 | Walid Cheddira | Entry | 2722.519 |
| 3869220 | Abdelhamid Sabiri | Entry | 2720.940 |
| 3869220 | Yahia Attiyat allah | Entry | 2647.263 |
| 3869220 | Jawad El Yamiq | Entry | 2577.698 |
| 3869321 | Cody Mathès Gakpo | Exit | 501.825 |
| 3869321 | Lisandro Martínez | Exit | 602.500 |
| 3869321 | Nahuel Molina Lucero | Exit | 963.592 |
| 3869321 | Leandro Daniel Paredes | Entry | 3586.494 |
| 3869321 | Nicolás Alejandro Tagliafico | Entry | 2904.441 |
| 3869321 | Germán Alejandro Pezzella | Entry | 2885.192 |
| 3869321 | Lautaro Javier Martínez | Entry | 2678.084 |
| 3869420 | Éder Gabriel Militão | Exit | 1022.077 |
| 3869420 | Lucas Tolentino Coelho de Lima | Exit | 1023.311 |
| 3869420 | Antony Matheus dos Santos | Entry | 3450.476 |
| 3869420 | Rodrygo Silva de Goes | Entry | 2939.846 |
| 3869420 | Nikola Vlašić | Entry | 2671.214 |
| 3869420 | Bruno Petković | Entry | 2667.384 |
| 3869420 | Pedro Guilherme Abreu dos Santos | Entry | 1763.194 |
| 3869486 | Walid Cheddira | Exit | 352.779 |
| 3869685 | Jules Koundé | Exit | 211.646 |
| 3869685 | Raphaël Varane | Exit | 699.372 |
| 3869685 | Marcos Javier Acuña | Entry | 2821.991 |
| 3869685 | Gonzalo Ariel Montiel | Entry | 736.000 |

## Control totals

| Variable | Total |
|---|---:|
| `passes` | 68515 |
| `completed_passes` | 56346 |
| `passes_unknown_outcome` | 394 |
| `total_pass_distance` | 1441024.892257 |
| `assists` | 110 |
| `carries` | 53764 |
| `total_carry_distance` | 303491.228682 |
| `pressures` | 16554 |
| `tackles` | 2185 |
| `fouls_committed` | 1775 |
| `fouls_won` | 1693 |
| `interceptions` | 1371 |
| `dribbles` | 1793 |
| `completed_dribbles` | 937 |
| `shots` | 1453 |
| `shots_on_target` | 508 |
| `goals` | 169 |
| `expected_goals` | 155.898542 |
| `own_goals` | 3 |

The 169 goals from shots and 3 own goals reconstruct **172 goals**. Own goals have no Shot event and therefore receive no xG in this aggregation. The 394 passes with unknown outcomes are not reclassified as definite completions or failures.

Definitions follow the data dictionary. In particular, committed and won foul totals need not match; interceptions include all outcomes; carry distances are straight-line segments in normalised coordinates. Fully populated cells do not establish that the source captures every real action in the match.

## Final file fingerprint

CSV: `statsbomb_world_cup_2022_player_match.csv`

SHA-256: `fae38ddb417e041f3b14bc85bd5f07b5fc132e4184039d8db9ef26553014392a`

The CSV's variable names and category labels are already in English. Player names retain their original spelling. The same CSV accompanies both language versions of the documentation; its data are unchanged.
