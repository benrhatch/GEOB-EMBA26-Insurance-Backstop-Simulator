# GEOB-EMBA26-Insurance-Backstop-Simulator
Live market simulator for a Kalshi-style macro bet on Canadian catastrophe insurance legislation

# Canada's Insurance Time Bomb
### Live Market Simulator

---

## Overview

This is an interactive market simulator built for a macro investment pitch in the Ivey Executive MBA program (Class of 2026). It models how a Kalshi-style prediction market contract might evolve over 19 months, from June 2026 through December 31, 2027.

The contract:

> **"Will the Government of Canada enact federal legislation establishing an earthquake or catastrophic insurance backstop mechanism before December 31, 2027?"**
>
> *Resolution: YES if a bill receives Royal Assent on or before December 31, 2027. NO otherwise.*

The trade is **NO** — betting against Canadian legislative throughput on a file that has seen thirteen years of pre-legislative activity without enacted law.

## The Pricing

| | Probability of YES | Implied NO Price |
|---|---|---|
| Conventional wisdom (industry / media) | ~55% | 45¢ |
| Sophisticated market estimate | ~40% | 60¢ |
| Our analytical view | **20%** | **80¢** |

**Trade:** Buy NO at 60¢. Target settlement at $1.00. Expected value: **+20¢ per contract** (33% expected return on stake).

The 20% view is triangulated from three independent methods:

- **Canadian financial sector base rate** — historical frequency of "intention to consult" announcements becoming enacted legislation within 24 months: 15-20%
- **Decision tree compound probability** — four sequential legislative gates (response published × cabinet approval × bill tabled × Royal Assent), each below 70%, compounding to ~11%
- **International reference class** — preemptive catastrophe insurance backstops in developed countries: 8-15% within a 2-year window absent a triggering disaster

## Using the Simulator

The simulator has two modes:

**Manual mode:** Click event buttons on the right (positive shocks in green, negative shocks in red) to apply specific market-moving events. Use "Advance 1 month" or "Advance 3 months" to push time forward.

**Auto-simulation:** Click "▶▶ Run full simulation" to play a randomly-selected scenario across the full 19-month window. The simulation takes 90-105 seconds, with narrative captions appearing as events fire. The contract resolves at the end — NO settles at $1.00 in most scenarios; YES settles at $1.00 if Royal Assent fires.

For best results, press **F11** to enter browser fullscreen mode before running.

## The Math

Two forces drive price movements:

### Event Shocks (Odds-Ratio Multipliers)

Each event applies a multiplier to the current odds, keeping probabilities mathematically bounded between 0 and 1:

```
new_probability = (current_odds × multiplier) / (1 + current_odds × multiplier)
```

| Event | Multiplier |
|-------|-----------|
| Bill tabled in Parliament | 3.0× |
| Royal Assent received | 50.0× (effectively resolves) |
| Bill passes House of Commons | 2.0× |
| Major BC earthquake (M7+) | 4.0× |
| CERPA-style response published | 1.8× |
| Federal election called | 0.4× |
| Government collapses | 0.35× |
| Cabinet rejects CERPA model | 0.5× |

### Time Decay

Between events, probability drifts exponentially toward a 5% floor with a 180-day half-life — modeling the closing legislative window. Within 90 days of a positive event, decay slows to a 540-day half-life (momentum effect). Small Gaussian noise scaled by √days adds realistic market wiggle.

### Scenario Distribution

The auto-simulator picks from six weighted scenarios reflecting the honest probability distribution behind the 20% view:

| Scenario | Weight | Terminal YES |
|----------|--------|--------------|
| Base case — slow grind | 35% | ~9% |
| Base case — election shock | 20% | ~20% |
| Base case — cabinet rejects | 15% | ~9% |
| Bull stall (close but no Royal Assent) | 15% | ~43% |
| Full YES path | 8% | ~74% |
| Seismic disaster | 7% | ~73% |

Monte Carlo across 5,000 simulated runs produces an average terminal YES probability of ~26%, closely matching the 20% analytical view. About 15% of runs end above 50% YES (i.e., the NO position loses) — reflecting that even a strong contrarian view requires acknowledging meaningful tail risk.

## Technical Notes

- Single-file HTML — no build step, no dependencies
- Vanilla JavaScript and inline SVG; no frameworks
- Fonts: IBM Plex Serif (display text) and Geist (numerical displays) via Google Fonts
- All numerical displays use `font-variant-numeric: tabular-nums` to prevent layout shift during live updates
- Tested in Chrome, Safari, and Firefox
- Internet connection required on first load (for fonts); works offline thereafter

## Context

This pitch was developed for the EMBA macroeconomics course taught by Professor Dippel at Ivey Business School. The course requires a 15-minute macro investment pitch, with students given the option to bet directly on a macroeconomic trend by constructing a hypothetical Kalshi-style prediction market.

The macro framing positions Canadian legislative inaction on catastrophe insurance as a knowable expression of constrained federal fiscal capacity — a multi-billion dollar contingent liability that Ottawa cannot credibly add to its balance sheet given current trajectories on housing, defense, healthcare, and climate adaptation commitments.

## Author

Ben Hatch
Ivey Executive MBA, Class of 2026

---

*Built with the assistance of Claude (Anthropic). All analytical conclusions and pricing decisions are the author's own.*
