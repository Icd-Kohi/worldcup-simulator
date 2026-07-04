# World Cup Simulator

Entry for FIFA World Cup simulator challenge. The app loads 32 teams, randomly draws groups, simulates the group stage and knockout bracket, resolves knockout draws by penalties, and submits the champion result in a JSON format.

## Stack

- HTML
- CSS
- JavaScript

## Architecture

```
index.html
src/
  main.js
  api/
    worldCupApi.js
  core/
    groups.js
    knockout.js
    simulator.js
    standings.js
    teams.js
    utils.js
  data/
    teams.js
  images/
    logo.png
  ui/
    renderBracket.js
    renderChampion.js
    renderGroups.js
    renderMatches.js
    renderStandings.js
    state.js
    html.js
    teamName.js
  styles/
    style.css
```

Business rules in `src/core`. Rendering modules receive already computed tournament state and only display it.

## Business Rules

- 32 teams.
- Teams are shuffled with Fisher-Yates shuffle and assigned to groups A through H.
- Each group has 4 teams.
- Each group plays 6 matches across 3 rounds:
  - Round 1: 1v2, 3v4
  - Round 2: 1v3, 2v4
  - Round 3: 1v4, 2v3
- Group matches can end in wins or draws.
- Standings track points, goals for, goals against, and goal difference.
- Tiebreakers are points, goal difference, then random draw.
- The top 2 teams from each group advance.
- Round of 16 pairings:
  - 1A vs 2B
  - 1C vs 2D
  - 1E vs 2F
  - 1G vs 2H
  - 1B vs 2A
  - 1D vs 2C
  - 1F vs 2E
  - 1H vs 2G
- Knockout rounds preserve winner flow from adjacent matches.
- Knockout draws are resolved by penalties, always producing one winner.

## Final Result Payload

The final result payload sends:

- `EquipeA`: champion team token
- `EquipeB`: runner-up team token
- Champion and runner-up names as readable context
- Final score
- Penalty score = `0 - 0` when penalties were not needed

## Data 
#### Team 
```js 
{
  id: "team-id",
  name: "Brazil",
  group: "A"
}
```
#### Match 
```js 
{
  homeTeam: {...},
  visitingTeam: {...},
  homeGoals: 2,
  visitingGoals: 1,
  stage: "group", // group, round16, quarter, semifinal, final
  group: "A",
  round: 1,
  penalties: {
    home: 0,
    visiting: 0
  },
  winner: {...}
}
```
#### Standing
```js 
{
  team: {...},
  points: 0,
  goalsFor: 0,
  goalsAgainst: 0,
  goalDifference: 0
}
```
#### Tournament global state
```js 
{
  teams: [],
  groups: {},
  groupMatches: {},
  standings: {},
  round16: [],
  quarterFinals: [],
  semiFinals: [],
  final: null,
  champion: null
}
```
