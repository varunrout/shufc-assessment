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

3) **Build products for younger individuals (18–39) as a long-run growth bet**
- Younger representation is low. If the club wants future-proofing, it needs an intentional pathway (entry hospitality, flexible bundles, group experiences).

4) **Fix data capture for corporate guest usage**
- To understand true demographic reach (and unlock corporate-to-consumer cross-sell), the club needs guest-level attendance attribution for Accounts (or at minimum, recipient-level capture).

---

## 5) Caveats
- Age is missing for **34** individuals; interpret bucket shares as “of non-missing.”
- Gender includes a large “Unspecified” category; avoid over-interpreting gender gaps.
- Distance/district are England-specific; non‑England customers are retained using `is_england_customer` and should be analysed with appropriate methods.
