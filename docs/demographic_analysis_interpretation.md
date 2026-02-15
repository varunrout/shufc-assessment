# Demographic Analysis  - Interpretation (Notebook 03)

## The short version

The hospitality product is corporate-led (we knew that from NB02), but the individual segment is where diversification lives. Demographically, individuals skew older (mean 57.2, median 58.5  - nearly half are 60+ among those with age data), heavily male (71.8%), and overwhelmingly England-based (98.2% of tickets). Local customers within 25km drive the highest volumes. The strategic question is how to grow the individual base without undermining the corporate stability engine.

## Individuals: who they are

### Age

Not a mass-market fan base. Age data is available for 168 of 202 individuals.

The distribution: 47% are 60+, 20.8% are 50-59, 17.3% are 40-49, and only 14.8% are under 40. Mean age 57.2, median 58.5. This is a mature, likely affluent demographic  - consistent with what you'd expect for premium hospitality pricing.

If growth is the goal, the club probably needs either (a) products that convert younger fans (entry-level hospitality, group experiences) or (b) better targeting of the "local professional aged 35-50" cohort who might not know the product exists.

### Gender

71.8% male, 9.4% female, 18.8% unspecified. The unspecified chunk makes it hard to draw firm conclusions. There might be an underserved women's audience, but it's hard to say without knowing what "unspecified" actually represents in the CRM.

## What makes a high-value individual?

Defined as top quartile by ticket volume (> 50 tickets). 45 high-value vs 136 lower-value out of 181 individuals with purchase history.

High-value individuals are slightly older on average (59.6 vs 56.2) and buy dramatically more: 154.2 avg tickets vs 20.2. The gap is about repeat commitment, not demographics  - which means the opportunity is converting lower-value individuals into repeat buyers through onboarding, renewal nudges, and bundle products, not just acquiring new customers.

### Distance matters

For England purchasers, proximity correlates strongly with volume:
- 0-10 km: 218.4 avg tickets, 96.5% repeat rate
- 10-25 km: 189.0 avg tickets, 95.0% repeat rate
- 25-50 km: 70.1 avg tickets (small base though)
- 50+ km: 66.0 avg tickets, 83.3% repeat rate

Local and near-local customers are the highest-volume, highest-repeat audience. Long-distance fans exist and can be valuable, but they're lower volume and less reliably repeat.

## Geography

England dominates: 98.2% of tickets from 231 purchasers (avg 183.9 each). Non-England is currently tiny  - 25 purchasers, avg 31.9 tickets, 1.8% share. Both segments have high repeat rates (94.4% vs 92.0%).

Non-England growth is plausible but niche. Better to treat it as a targeted premium traveller segment rather than expecting it to move the needle on volume in the short term.

## Individual segmentation (KMeans clustering)

Applied KMeans to 181 individuals using age (median-imputed for 34 missing values), distance (non-England imputed with max + 50km = 338km), log ticket volume, and transaction count. Elbow plot confirmed k=3.

| Segment | n | Avg age | Avg distance | Avg tickets | % England | % Male | Ticket share |
|---------|---|---------|-------------|------------|----------|--------|-------------|
| High-Value Local Regulars | 80 | 64.9 | 16 km | 102.0 | 100% | 81.2% | 84.2% |
| Mid-Engagement Steady Fans | 32 | 55.9 | 300 km | 19.2 | 34.4% | 56.2% | 6.3% |
| Low-Volume Occasionals | 69 | 49.3 | 18 km | 13.3 | 100% | 75.4% | 9.5% |

**High-Value Local Regulars** are the individual equivalent of NB02's Anchor Buyers. Older (avg 65), all local, all England, heavily male. ~102 tickets each  - roughly 2 per match across a season. They're the individual revenue backbone. The age profile also flags a medium-term succession risk: the club should be thinking about cultivating the next generation.

**Mid-Engagement Steady Fans** are the interesting outlier. Geographically distant (300km avg), most gender-balanced segment (56.2% male), only 34.4% England-based. These are the travel-willing fans from the geography analysis. 19 tickets each isn't casual  - they engage meaningfully, just less often. Travel-inclusive packages or flexible fixture-picking plans could work here.

**Low-Volume Occasionals** are the conversion opportunity. Youngest segment (avg 49.3), all local, but only 13 tickets each. They have geographic proximity and product awareness  - the barrier is frequency. Same playbook as NB02's Light Touch: multi-match tasters, post-first-visit nudges, hospitality intro products.

The NB02 and NB03 segmentations tell a consistent story. NB02's Core Regulars overlap heavily with NB03's High-Value Regulars + Occasionals. NB02's Light Touch maps to NB03's Low-Volume Occasionals. They reinforce each other.

## What to do about it

1. Protect the corporate base  - Accounts aren't interpretable via age/gender, manage through retention and account relationships
2. Scale the "local affluent repeat" individual segment within 0-25km  - that's the cleanest conversion funnel
3. Build entry-level products for 18-39s as a long-run growth bet. The Low-Volume Occasionals segment (avg 49.3) already skews younger than the high-value base (64.9), so younger fans are present but under-engaged
4. Target mid-engagement distance fans with tailored travel/premium offers  - small segment but potentially high-margin
5. Fix corporate guest data capture  - guest-level attendance attribution would unlock the true demographic reach of hospitality

## Caveats

Age is missing for 34 individuals (clustering imputes with median 59). Gender has a big "Unspecified" chunk  - don't over-interpret gender gaps. Distance/district are England-specific; non-England customers get imputed distance for clustering. KMeans is sensitive to feature scaling and imputation  - treat the segments as indicative and validate against commercial judgment.
