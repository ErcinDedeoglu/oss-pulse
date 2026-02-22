# 📖 Methodology

> How OSS Pulse collects data and calculates metrics

---

## Data Collection

### What We Track

OSS Pulse tracks **GitHub's most influential repositories** — not a random sample, but the elite of open source. We focus on repositories with proven impact and community engagement, filtering out repos below 500 stars.

### Selection Criteria

Our daily collection pulls repositories matching these criteria:

| Query | Sort By | Purpose |
|-------|---------|---------|
| `stars:>1` | Stars | Most starred repositories |
| `forks:>500` | Forks | Highly forked projects (active development) |
| `help-wanted-issues:>0` | Help Wanted | Community-driven projects seeking contributors |
| `watchers:>100` | Default | Well-watched repositories |
| `topic:ai` | Default | Trending AI/ML projects |

### Freshness Filter

All repositories must have been **pushed within the last 2 years** to be included. This ensures we track active, maintained projects.

### Quality Filter

Repositories below **500 stars** are excluded from analytics. This removes low-quality repos and small personal projects that add noise without adding insight.

### Collection Process

- **Source:** GitHub Search API
- **Frequency:** Daily at 00:00 UTC
- **Deduplication:** Repositories appearing in multiple queries are counted once
- **History:** 297 days of daily snapshots for trend analysis

---

## Momentum Score (0-100)

The Momentum Score is a single number that answers: **"Is this project accelerating right now?"**

It combines four components, each scored 0-100 individually, then weighted:

### Components

| Component | Weight | What It Measures |
|-----------|--------|------------------|
| **Velocity** | 40% | Absolute 7-day star gain, percentile-scaled against all tracked repos |
| **Relative Growth** | 25% | 7-day gain as % of existing stars — rewards small repos growing fast |
| **Acceleration** | 25% | This week's gain minus last week's gain — speeding up or slowing down? |
| **Consistency** | 10% | Positive growth across 7-day, 14-day, and 30-day windows |

### Velocity Thresholds (Percentile-Based)

| Weekly Gain | Score Range | Approx. Percentile |
|-------------|-------------|---------------------|
| >= 2,000 | 100 | >P97 |
| 800 - 2,000 | 80 - 100 | P95 - P97 |
| 500 - 800 | 70 - 80 | P90 - P95 |
| 175 - 500 | 55 - 70 | P75 - P90 |
| 60 - 175 | 40 - 55 | P50 - P75 |
| 10 - 60 | 20 - 40 | P25 - P50 |
| 0 - 10 | 0 - 20 | <P25 |

### Viral Bonus

If a repo has a genuine viral spike (>5x its own 14-day baseline), it receives a **+10 bonus** (capped at 100).

### Design Principles

- Thresholds are calibrated against the **actual data distribution** of tracked repos, not arbitrary round numbers
- The formula uses **percentile-based scoring** to ensure the full 0-100 range is utilized
- Relative growth (gain/stars%) prevents mega-repos from dominating purely on absolute numbers

---

## Quadrant View

Repos are classified into a **2×2 matrix** based on size and momentum:

|  | Momentum >= 55 | Momentum < 55 |
|--|----------------|---------------|
| **>= 50k stars** | 🔥 Hot Giants | 🏔️ Coasting Giants |
| **< 50k stars** | 🚀 Rising Stars | 🌱 Emerging |

- **50k stars** divides "big" from "small" — roughly the median of tracked repos
- **55 momentum** divides "hot" from "not" — slightly above the mathematical midpoint, tuned to separate the genuinely accelerating repos

---

## Sparklines

Each repo gets a 7-character sparkline showing its star count trajectory over the last 7 days:

```
▁▂▃▄▅▆▇
```

Characters map to the repo's own min-max range over those 7 days, so the sparkline shows **shape** (accelerating, steady, spiking) rather than absolute magnitude. A flat sparkline `▁▁▁▁▁▁▁` means no change; a rising `▁▂▃▄▅▆▇` means steady acceleration.

---

## Health Score Calculation

Our health score is inspired by the [CHAOSS Project](https://chaoss.community/) (Community Health Analytics for Open Source Software), a Linux Foundation initiative.

### Dimensions

| Dimension | Weight | Calculation |
|-----------|--------|-------------|
| **Activity** | 30% | Based on days since last push. Daily push = 100, >1 year = 5 |
| **Popularity** | 20% | Stars per day since creation, logarithmically scaled |
| **Engagement** | 20% | Fork-to-star ratio. >30% = 100, <2% = 10 |
| **Issues** | 15% | Open issues per 1,000 stars. <1 = 100, >50 = 10 |
| **Quality** | 15% | License (+40), description (+25), topics (+20), homepage (+15) |

### Penalties

- **Archived:** -70% (score × 0.3)
- **Disabled:** -90% (score × 0.1)
- **No License:** -15% (score × 0.85)

### Grade Scale

| Grade | Score Range |
|-------|-------------|
| A+ | 95-100 |
| A | 90-94 |
| A- | 85-89 |
| B+ | 80-84 |
| B | 75-79 |
| B- | 70-74 |
| C+ | 65-69 |
| C | 60-64 |
| C- | 55-59 |
| D+ | 50-54 |
| D | 45-49 |
| D- | 40-44 |
| F | 0-39 |

---

## Viral Spike Detection

A viral spike is detected when:
1. Daily star gain exceeds **5× the 14-day rolling average**
2. The spike occurs within the last 7 days

High absolute weekly gain alone does **not** classify a repo as viral. Viral classification requires a genuine spike relative to the repo's own baseline. This prevents large repos with consistently high gains from being mislabeled as "viral."

---

## Organization Rankings

Organizations are ranked by three views:
1. **Total Stars:** Sum of all repository stars (default sort)
2. **Average Momentum:** Mean momentum score across all repos — which orgs are accelerating right now?
3. **Average Health:** Mean health score across all repos — long-term project health

Only organizations with 2+ tracked repos appear in the momentum and health views.

---

## Language Analysis

Languages are determined by GitHub's primary language detection for each repository.

---

## Limitations

- Data reflects public repositories only
- Star counts may include bot/fake stars (we don't filter these)
- Health scores are heuristic, not definitive quality measures
- Historical data limited to our collection period
- Momentum Score requires at least 7 days of history; new repos may show as unranked

---

## References

- [CHAOSS Project](https://chaoss.community/)
- [CHAOSS Metrics](https://chaoss.community/kb-metrics-and-metrics-models/)
- [GitHub API Documentation](https://docs.github.com/en/rest)

---

[← Back to Dashboard](../README.md) | [Momentum Rankings](momentum.md) | [Health Scores](health-scores.md) | [Organizations](organizations.md)
