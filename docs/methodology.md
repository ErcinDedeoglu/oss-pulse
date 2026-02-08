# 📖 Methodology

> How OSS Pulse collects data and calculates metrics

---

## Data Collection

### What We Track

OSS Pulse tracks **GitHub's most influential repositories** - not a random sample, but the elite of open source. We focus on repositories that have proven impact and community engagement.

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

### Collection Process

- **Source:** GitHub Search API
- **Frequency:** Daily at 00:00 UTC
- **Deduplication:** Repositories appearing in multiple queries are counted once
- **History:** 250+ days of daily snapshots for trend analysis

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

## Growth Pattern Classification

| Pattern | Criteria |
|---------|----------|
| 🚀 **Viral** | >500 stars/week OR >5x baseline daily average |
| ⚡ **Fast** | >100 stars/week sustained |
| 📈 **Moderate** | 20-100 stars/week |
| 🐢 **Slow** | 3-20 stars/week |
| ⏸️ **Stable** | <3 stars/week change |
| 📉 **Declining** | Negative weekly growth |

### Viral Spike Detection

A viral spike is detected when:
1. Daily star gain exceeds 5× the 14-day rolling average
2. The spike occurs within the last 7 days
3. Absolute gain is significant (>100 stars)

---

## Organization Rankings

Organizations are ranked by:
1. **Total Stars:** Sum of all repository stars
2. **Repo Count:** Number of repos in our dataset
3. **Average Health:** Mean health score across all repos

---

## Language Analysis

Languages are determined by GitHub's primary language detection for each repository.

---

## Limitations

- Data reflects public repositories only
- Star counts may include bot/fake stars (we don't filter these)
- Health scores are heuristic, not definitive quality measures
- Historical data limited to our collection period

---

## References

- [CHAOSS Project](https://chaoss.community/)
- [CHAOSS Metrics](https://chaoss.community/kb-metrics-and-metrics-models/)
- [GitHub API Documentation](https://docs.github.com/en/rest)

---

[← Back to Home](../README.md)
