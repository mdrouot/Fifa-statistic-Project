**StatsBomb is a professional provider of football data and analytics**, now part of Hudl. Its products support team analysis and player recruitment, among other uses. **StatsBomb Open Data** is the portion of its data made freely available for research and learning. [Official overview](https://www.hudl.com/en_gb/products/statsbomb), [Open Data repository](https://github.com/statsbomb/open-data).

It is a credible source for this course for three reasons:

- **The data come directly from the provider**, including detailed match events.
- **Definitions are documented**: we can establish exactly what counts as a pass, shot, or interception.
- **Calculations can be checked and reproduced**: the source files and their version history are publicly accessible. [Documentation and file structure](https://github.com/statsbomb/open-data).

This does not mean the data are perfect. We identified inconsistencies in some participation intervals in the lineup files. We documented them and used event data to calculate minutes played. The scores of **all 64 matches** and the aggregated statistics were then checked.

These are **the exact sources used**, linked to the repository version downloaded for this dataset:

- [List of all 64 matches — `43/106.json`](https://github.com/statsbomb/open-data/blob/4b73468fc5b0f1950f9f66fada70ad3a4f9327cb/data/matches/43/106.json)
- [Match events folder — `events`](https://github.com/statsbomb/open-data/tree/4b73468fc5b0f1950f9f66fada70ad3a4f9327cb/data/events)
- [Team lineups folder — `lineups`](https://github.com/statsbomb/open-data/tree/4b73468fc5b0f1950f9f66fada70ad3a4f9327cb/data/lineups)
- [Official data definitions — PDF](https://github.com/statsbomb/open-data/blob/4b73468fc5b0f1950f9f66fada70ad3a4f9327cb/doc/StatsBomb%20Open%20Data%20Specification%20v1.1.pdf)

**Our CSV is a teaching dataset compiled from these source files**, rather than a file published in this form by StatsBomb.