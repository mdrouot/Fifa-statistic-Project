# Data dictionary — Men's FIFA World Cup 2022

Source: **StatsBomb Open Data**. Each row represents **one player who participated in one match**. Unique key: `match_id` + `player_id`. Unused substitutes are excluded. Goalkeepers are included.

The CSV contains 1,995 rows and 38 variables. File format: UTF-8, comma-separated fields, decimal points, and one header row. In Excel, use the Text/CSV import option if columns are not separated automatically. Names and categories retain their source labels.

## Identifiers and context

| Variable | Type | Definition |
|---|---|---|
| `competition_id` | Integer identifier | StatsBomb competition identifier; always 43. |
| `season_id` | Integer identifier | StatsBomb season identifier; always 106. |
| `match_id` | Integer identifier | Unique match identifier. |
| `match_date` | Date | Match date, in YYYY-MM-DD format. |
| `match_name` | Text | Designated home team - designated away team. |
| `stage` | Category | `Group Stage`, `Round of 16`, `Quarter-finals`, `Semi-finals`, `3rd Place Final`, or `Final`. |
| `player_id` | Integer identifier | StatsBomb player identifier. Use this rather than the player's name when grouping observations. |
| `player_name` | Text | Player's full name in the lineup file. |
| `team_id` | Integer identifier | Player's team in this match. |
| `team_name` | Text | Name of the player's team. |
| `opponent_id` | Integer identifier | Opposing team's identifier. |
| `opponent_name` | Text | Opposing team's name. |
| `is_home` | Binary, 0/1 | 1 if the team is designated as the home team in the source. This designation does not measure a geographical home advantage. |
| `position_id` | Integer identifier | StatsBomb code corresponding to `position`. |
| `position` | Category | Starting position for starters (`Starting XI`); first position recorded in a personal event after entering the match for substitutes. Lineup information is used for two players without an event recording their position. Subsequent position changes are not summarised. |
| `is_starter` | Binary, 0/1 | 1 if the player is in the starting eleven. |
| `minutes_played` | Decimal, minutes | Participation time from entry until substitution, dismissal, or the end of the match. Includes stoppage time and extra time; excludes intervals between periods and penalty shootouts. Temporary absences for treatment remain included. Calculated from event timestamps, rounded to 6 decimal places. |
| `match_duration_minutes` | Decimal, minutes | Sum of the durations of periods 1–2 and, where applicable, 3–4, including stoppage time. Identical for all players in the same match. |
| `extra_time` | Binary, 0/1 | 1 if the match went to extra time. |

## Actions and totals

All statistics below cover **periods 1–4**. Penalties taken during the match are included. Penalty shootouts (period 5) are entirely excluded.

| Variable | Type / unit | Operational definition |
|---|---|---|
| `passes` | Count | Number of `Pass` events, including all types and outcomes: open play, throw-ins, corners, free kicks, goalkeeper distributions, etc. |
| `completed_passes` | Count | Passes without a `pass.outcome` field, StatsBomb's convention for a completed pass. |
| `passes_unknown_outcome` | Count | Passes with `pass.outcome` equal to `Unknown` (id 77). Included in `passes`, excluded from `completed_passes`. |
| `carries` | Count | Number of `Carry` events: controlling the ball while moving or standing still. |
| `shots` | Count | Number of `Shot` events. |
| `shots_on_target` | Count | Shots with outcome `Goal`, `Saved`, or `Saved to Post` (ids 97, 100, 116). Excludes `Blocked`, `Post`, `Off T`, `Wayward`, and `Saved Off Target`. This convention is explicitly derived from StatsBomb's shot outcomes. |
| `goals` | Count | Shots with outcome `Goal`. Excludes own goals. |
| `own_goals` | Count | Player's `Own Goal Against` events, credited to the opposing team when reconstructing the score. Do not also add the corresponding `Own Goal For` events. |
| `assists` | Count | Passes with `pass.goal_assist = true`. This does not count every pass leading to a shot (`shot_assist`). |
| `dribbles` | Count | `Dribble` events: attempts to beat an opponent. Distinct from carries. |
| `completed_dribbles` | Count | Dribbles with outcome `Complete` (id 8). |
| `pressures` | Count | `Pressure` events attributed to the player. This is not the number of actions the player performs while under pressure. |
| `tackles` | Count | `Duel` events of type `Tackle` (id 11), regardless of outcome. Excludes aerial duels and `Dribbled Past` events. |
| `interceptions` | Count | `Interception` events, regardless of outcome, including unsuccessful interceptions. Passes of type `Interception` are not added to this count. |
| `fouls_committed` | Count | `Foul Committed` events, including those where advantage is played. Offside offences are excluded. |
| `fouls_won` | Count | `Foul Won` events, including those where advantage is played. Some committed fouls have no individual recipient, so the two foul totals can differ. |
| `total_pass_distance` | Decimal, yards according to StatsBomb | Sum of `pass.length` across all passes, completed or otherwise. Origin-to-destination length on the source's standardised pitch, not an aerial trajectory or distance run. Rounded to 6 decimal places. |
| `total_carry_distance` | Decimal, StatsBomb coordinate units | Sum of the Euclidean distances `sqrt((x_end-x_start)² + (y_end-y_start)²)` of carries. Standardised 120 × 80 pitch, using the same coordinate axes as pass lengths. A straight-line approximation, not a measurement of the actual trajectory or total distance run. Rounded to 6 decimal places. |
| `expected_goals` | Decimal, expected number of goals | Sum of `shot.statsbomb_xg` across the player's shots. A cumulative expectation, not a percentage; it can exceed 1. Zero if there are no shots. Rounded to 8 decimal places. |

## Zeros, unknown outcomes, and classroom calculations

There are no empty cells in the final file. A zero count means that no corresponding event was recorded for that player in that match. Required distances and xG values are present for every relevant action: no missing value was replaced with zero. The 394 explicitly unknown pass outcomes are retained in a separate variable.

Examples of variables students can calculate:

- Mean pass length: `total_pass_distance / passes`.
- Mean carry distance: `total_carry_distance / carries`.
- Proportion of passes recorded as completed: `completed_passes / passes`.
- Completion rate among passes with known outcomes: `completed_passes / (passes - passes_unknown_outcome)`. This version retains other outcomes, including `Injury Clearance`, in the denominator.
- Dribble completion rate: `completed_dribbles / dribbles`.
- Mean shot quality: `expected_goals / shots`.

When the denominator is zero, the result is **undefined**, not zero. No means, rates, or mean action coordinates (`mean_x`/`mean_y`) are supplied.

Rows are not statistically independent: the same player appears in multiple matches, and players in the same match share a context. Counts depend, among other things, on participation time and position.

## Sources

- [Official StatsBomb Open Data repository](https://github.com/statsbomb/open-data).
- [Official event specification, version 1.1](https://github.com/statsbomb/open-data/blob/4b73468fc5b0f1950f9f66fada70ad3a4f9327cb/doc/StatsBomb%20Open%20Data%20Specification%20v1.1.pdf), particularly pages 8–9, 17–18, 24–31, and the coordinate and position appendices.
- Repository version used: `4b73468fc5b0f1950f9f66fada70ad3a4f9327cb`. See `validation_report_en.md` for lineup discrepancies and the minutes calculation method.
