# Demographic Analysis — Interpretation (Notebook 03)

## Executive Summary (Commercial Lens)
Hospitality ticketing is structurally corporate-led (Accounts are the stability engine), but the **individual segment is where diversification and growth lives**.

From the demographic view:
- Individuals skew **older** (mean **57.2**, median **58.5**; **47.0%** are **60+** among non-missing ages).
- Individuals are **male-skewed** (**71.8%** Male; **9.4%** Female; **18.8%** Unspecified).
- The customer base and ticket volume are overwhelmingly **England-based** (England accounts for **98.2%** of tickets; non‑England **1.8%**).
- Within England, distance matters materially for volume: **0–10 km** and **10–25 km** purchasers drive the highest average volumes.

This aligns with the behavioural story from Notebook 02: protect corporate season-holding revenue, but **reduce concentration risk** by deliberately growing repeat and high-value individuals.

---

## 1) Individuals (Growth Segment): Who They Are
### Age profile is strongly older
The individual hospitality customer base is not “mass fan” — it skews towards a mature demographic.

Key points:
- Age coverage: **168 / 202** individuals have non-missing ages.
- Central tendency: mean **57.2**, median **58.5**.
- Age buckets (non-missing):
  - **60+**: **47.0%**
  - **50–59**: **20.8%**
  - **40–49**: **17.3%**
  - **30–39**: **8.3%**
  - **18–29**: **6.5%**

Commercial interpretation:
- Hospitality, at least for individuals, currently over-indexes to an older/affluent base.
- If growth is needed, the club likely needs either (a) products that convert younger fans, or (b) targeted acquisition that expands the “local affluent professional” pool.

### Gender is skewed (and needs cautious handling)
Individuals are predominantly male, with a significant “Unspecified” segment.

- Gender split: **71.8% Male**, **9.4% Female**, **18.8% Unspecified**.

Commercial interpretation:
- There may be an under-served opportunity in women’s hospitality audiences, but conclusions should be moderated by how gender is captured and what “Unspecified” represents.

---

## 2) Individuals: What Drives High Value
### High-value individuals are defined by behaviour, not labels
Notebook 03 defines high-value individuals as the **top quartile by ticket volume**:
- Individuals with volume records: **181**
- High-value cutoff: **> 50** tickets
- Segment sizes: **45 high-value** vs **136 lower-value**

### High-value profile (headline)
High-value individuals are:
- Slightly older on average (mean age **59.6** vs **56.2** lower-value).
- Much higher volume:
  - High-value avg tickets: **154.2** (median **138.0**)
  - Lower-value avg tickets: **20.2** (median **16.0**)

Commercial interpretation:
- The real opportunity is moving lower-value individuals into repeat/high-value behaviour (onboarding, renewal nudges, bundles, and match-plan products), not only recruiting net-new.

### Distance is a practical acquisition lever
For England purchasers:
- Avg tickets per purchaser by distance bucket:
  - **0–10 km**: **218.4** (repeat rate **96.5%**)
  - **10–25 km**: **189.0** (repeat rate **95.0%**)
  - **25–50 km**: **70.1** (repeat rate **100.0%**, but small base)
  - **50+ km**: **66.0** (repeat rate **83.3%**)

Commercial interpretation:
- **Local and near-local England customers are the highest-volume, highest-repeat audience** for hospitality.
- Long-distance England customers exist and can be valuable, but are lower volume on average and appear less reliably repeat.

---

## 3) Geography: What It Means for Growth
### England dominates volume (non‑England is currently small)
Ticket share and purchasers:
- England: **98.2%** of tickets; **231** purchasers; avg **183.9** tickets per purchaser.
- Non‑England: **1.8%** of tickets; **25** purchasers; avg **31.9** tickets per purchaser.

Repeat remains high in both:
- Non‑England repeat rate: **92.0%**
- England repeat rate: **94.4%**

Commercial interpretation:
- International/non‑England growth is plausible but is currently a niche segment.
- A pragmatic strategy is to treat non‑England as a targeted premium traveller segment (low base, potentially high willingness-to-pay), rather than expecting it to solve volume goals in the near term.

---

## 4) Strategic Implications (Recommended Actions)
1) **Protect the corporate base** (Accounts are the stability engine)
- This notebook reinforces the limitation: Accounts are not demographically interpretable via age/gender; they should be managed through retention, seat allocation, and account relationship strategy.

2) **Scale the “local affluent repeat” individual segment**
- The highest-volume individuals cluster close to Bramall Lane. Targeting within **0–25 km** is likely the cleanest conversion funnel.
- Clustering confirms this: the **High-Value Local Regulars** segment (80 individuals, avg 16 km distance) accounts for **84.2%** of all individual tickets.

3) **Build products for younger individuals (18–39) as a long-run growth bet**
- Younger representation is low. If the club wants future-proofing, it needs an intentional pathway (entry hospitality, flexible bundles, group experiences).
- The **Low-Volume Occasionals** segment (avg age 49.3) skews younger than the high-value base (avg age 64.9), suggesting younger individuals are present but under-engaged.

4) **Target the Mid-Engagement distance segment with tailored offers**
- The clustering identified 32 **Mid-Engagement Steady Fans** with an avg distance of 300 km, buying 19 tickets on average. Only 34% are England-based.
- These are likely travel-willing or international fans who engage meaningfully but at lower volume. Premium travel-inclusive matchday packages or guaranteed-seat loyalty plans could deepen their commitment.

5) **Fix data capture for corporate guest usage**
- To understand true demographic reach (and unlock corporate-to-consumer cross-sell), the club needs guest-level attendance attribution for Accounts (or at minimum, recipient-level capture).

---

## 5) Individual Segmentation (Demographic × Behaviour Clustering)

### Approach

To move from descriptive profiling to actionable segmentation, a KMeans clustering was applied to the **181 individual customers** using a blend of demographic and behavioural features:

1. **age** — numeric (34 missing values imputed with median = 59)
2. **distance_to_bramall_lane_km** — distance from postcode to Bramall Lane (non-England customers imputed with max observed + 50 km = 338 km)
3. **log_tickets** — log-transformed total ticket volume
4. **transaction_count** — number of distinct transactions

Features were standardised before clustering. An elbow plot (k = 2–7) was used to validate the choice of k = 3, balancing interpretability with meaningful separation.

### The Three Individual Segments

| Segment | Individuals | Avg Age | Avg Distance (km) | Avg Tickets | % England | % Male | Ticket Share |
|---------|------------|---------|-------------------|-------------|-----------|--------|-------------|
| **High-Value Local Regulars** | 80 | 64.9 | 16 | 102.0 | 100% | 81.2% | 84.2% |
| **Mid-Engagement Steady Fans** | 32 | 55.9 | 300 | 19.2 | 34.4% | 56.2% | 6.3% |
| **Low-Volume Occasionals** | 69 | 49.3 | 18 | 13.3 | 100% | 75.4% | 9.5% |

### What This Means

- **High-Value Local Regulars (80 individuals, 84.2% of individual tickets):** The heartbeat of the individual base. These are older (avg 64.9), overwhelmingly local (avg 16 km), 100% England-based, and heavily male (81.2%). They buy an average of 102 tickets each — close to 2 per match across a season. This is the “local affluent season hospitality fan” archetype. Retention is paramount: losing even a few would materially dent the individual revenue line.

- **Mid-Engagement Steady Fans (32 individuals, 6.3% of tickets):** A distinctive segment: moderately aged (avg 55.9), geographically distant (avg 300 km), and the most gender-balanced (56.2% male). Only 34.4% are England-based — these are the non-local and international fans identified in the geography analysis. At 19 tickets each they are not casual — they attend meaningfully, just less frequently. This is the segment most likely to respond to travel-inclusive packages, flexible fixture-picking plans, or guaranteed-seat loyalty schemes.

- **Low-Volume Occasionals (69 individuals, 9.5% of tickets):** The youngest segment (avg 49.3), overwhelmingly local (avg 18 km, 100% England), buying just 13 tickets each. These are trial buyers, occasional treat seekers, or lapsed fans. They share geographic proximity with the High-Value segment but radically lower engagement — which makes them the clearest conversion opportunity. Multi-match taster packages, first-visit-to-second-visit nudges, and hospitality “intro” products could shift some into regular attendance.

### Commercial Implication

1. **The High-Value Local Regulars are the individual equivalent of Anchor Buyers in NB02.** They need the same key-account-style attention — priority renewals, personalised service, recognition. Their age profile (avg 65) also signals a medium-term succession risk: the club should be actively cultivating the next generation of high-value individuals.

2. **The Low-Volume Occasionals are the primary growth lever.** At 69 individuals with 100% local/England presence, they already have geographic proximity and product awareness — the barrier is frequency, not access. This maps directly onto the “Light Touch” segment in NB02 and should share the same onboarding playbook.

3. **The Mid-Engagement distance fans are a niche premium opportunity.** Small (32 people) but distinctive — the most gender-balanced, the most geographically diverse, and the only segment with significant non-England representation. They should not be growth-targeted for volume, but could be high-margin with the right premium travel/experience packaging.

4. **Cross-referencing with NB02 segments:** The individual clustering in NB03 and the all-purchaser clustering in NB02 tell a consistent story. NB02’s Core Regulars (67.5% Customers) overlap heavily with NB03’s High-Value Local Regulars + Low-Volume Occasionals. NB02’s Light Touch (86% Customers) maps to NB03’s Low-Volume Occasionals. The two segmentations reinforce rather than contradict each other.

---

## 6) Caveats
- Age is missing for **34** individuals; interpret bucket shares as "of non-missing." Clustering imputes missing age with the median (59).
- Gender includes a large "Unspecified" category; avoid over-interpreting gender gaps.
- Distance/district are England-specific; non-England customers are retained using `is_england_customer` and imputed with max distance + 50 km (338 km) for clustering.
- KMeans clustering is sensitive to feature scaling and imputation choices. The segments are indicative, not definitive — they should be validated against commercial intuition and updated as more data accumulates.
