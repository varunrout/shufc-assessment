# Behavioural Analysis — Interpretation & Findings

## Overview

This document interprets the outputs of `02_behavioral_analysis.ipynb`, translating the data into commercially relevant insights for Sheffield United FC's hospitality operation.

**Dataset scope:** 43,289 tickets across 54 events, involving 256 unique purchasers and 276 unique owners.

---

## Important: What Does "Account" Mean in This Data?

Before interpreting the findings, it is essential to clarify what `customer_type = 'Account'` represents. An initial reading might suggest these are reseller/distributor accounts (like Ticketmaster or BookMyShow) that buy blocks and redistribute. **The data tells a different story.**

### Evidence from the Data

| Signal | Finding | Interpretation |
|--------|---------|----------------|
| **client_type** | 83.1% of Account tickets are "Season Ticket Holder" | These are season-long commitments, not one-off resale blocks |
| **Venue areas** | Accounts sit in The Pavilion (58%), Tony Currie Suite (22%), 1889 Restaurant (20%) | These are premium hospitality areas inside Bramall Lane — not general admission |
| **Seats per event** | Top accounts buy 16–113 seats per match, across 39–48 events | Fixed multi-seat allocations per match, consistent across the season |
| **Demographics** | All 75 Accounts have `gender = Unspecified` and `age = null` | Business entities, not individuals |
| **Owner pattern** | Every Account has exactly 1 unique owner (itself), 100% self-purchase | The CRM registers the corporate entity as the owner of all its seats |

### Conclusion: Accounts Are Corporate Hospitality Clients

**Accounts are businesses that hold season-long hospitality packages** — typically a fixed number of premium seats (a table, a box, a block) in hospitality areas at every home match. They are:

- **NOT resellers** (Ticketmaster-type intermediaries) — resellers would not hold "Season Ticket Holder" status in hospitality suites
- **NOT individual fans using a business account** — they have no age, no gender, and buy at a scale no individual would (16–113 seats per match)
- **Corporate clients** — businesses purchasing hospitality for staff entertainment, client hosting, or corporate relationship-building

### Why This Matters for Every Finding Below

The fact that `owner_id` always equals `purchaser_id` for Accounts does **not** mean these seats are unused or self-consumed. It means the data model does not capture who physically sits in the seat. When Account #1 buys 113 seats for a match, those seats are almost certainly occupied by a mix of staff, clients, and guests — but the CRM records the **corporate entity** as the owner, not the individuals. This has a direct impact on how we interpret gifting/hosting behaviour (Section 4).

---

## 1. Revenue Is Extremely Concentrated

### The Numbers

| Segment | Purchasers | Tickets | % of All Tickets |
|---------|-----------|---------|-----------------|
| Top 5 purchasers | 5 | 12,570 | 29.0% |
| Top 10 purchasers | 10 | 18,193 | 42.0% |
| Top 20 purchasers | 20 | 25,315 | 58.5% |
| Top 5% of purchasers | 12 | 19,705 | 45.5% |
| Top 10% of purchasers | 25 | 27,794 | 64.2% |
| Top 50% of purchasers | 128 | 41,376 | 95.6% |

**Gini coefficient: 0.761** — this indicates high concentration, comparable to wealth distribution in developing economies.

### What This Means

- **Five buyers control nearly a third of all hospitality revenue.** This is an extreme dependency. If even one of the top 5 accounts churned, the club would face a material revenue shortfall.
- **The bottom 50% of purchasers contribute just 4.4% of tickets.** The long tail of small buyers is economically marginal in isolation, but represents the growth opportunity.
- **The mean tickets per purchaser (169) is 3.7x the median (46).** This skew confirms that a small number of super-buyers dramatically inflate the average, while most purchasers buy far less.

### Top 10 Purchasers — All Corporate Accounts

| Rank | Type | Tickets | % of Total | Cumulative % |
|------|------|---------|-----------|-------------|
| #1 | Account | 4,402 | 10.2% | 10.2% |
| #2 | Account | 3,008 | 6.9% | 17.1% |
| #3 | Account | 1,740 | 4.0% | 21.1% |
| #4 | Account | 1,728 | 4.0% | 25.1% |
| #5 | Account | 1,692 | 3.9% | 29.0% |
| #6 | Account | 1,404 | 3.2% | 32.2% |
| #7 | Account | 1,220 | 2.8% | 35.0% |
| #8 | Account | 1,175 | 2.7% | 37.7% |
| #9 | Account | 960 | 2.2% | 39.9% |
| #10 | Account | 864 | 2.0% | 41.9% |

Every single top-10 purchaser is a corporate Account. The largest buyer alone controls 10.2% of all hospitality tickets — approximately 113 seats per match across 39 events.

Notably, many of these top accounts buy **exactly 16 or 36 seats per event**, suggesting standardised hospitality packages (e.g., a table of 16 in The Pavilion, two tables of 18 in the Tony Currie Suite). This consistency implies contractual, season-long commitments rather than ad-hoc purchasing.

### Commercial Implication

This is a **key account management problem**. The club's hospitality revenue is structurally dependent on a handful of corporate relationships. A formal key account retention programme with dedicated relationship managers for the top 20 buyers is not optional — it is essential infrastructure.

---

## 2. This Is a Corporate Hospitality Product

### The Numbers

| Purchaser Type | Purchasers | Tickets | % of Tickets | Avg Tickets/Purchaser |
|---------------|-----------|---------|-------------|----------------------|
| **Account** | 75 | 33,594 | 77.6% | 447.9 |
| **Customer** | 181 | 9,695 | 22.4% | 53.6 |

| client_type breakdown | Account | Customer |
|-----------------------|---------|----------|
| Season Ticket Holder | 83.1% | 50.6% |
| Match by Match Ticket Holder | 16.9% | 49.4% |

### What This Means

- **75 corporate Accounts generate 77.6% of all hospitality tickets.** The hospitality product is fundamentally B2B.
- **83% of Account tickets are season-long commitments.** These are not ad-hoc corporate bookings — they are contractual, season-long hospitality packages with fixed seat allocations per match.
- **Accounts buy 8.4x more tickets per purchaser** than individual Customers (448 vs 54). This reflects the structural difference: an Account holds a table/box with multiple seats at every match, while a Customer buys individual seats match-by-match or for the season.
- **Individuals are split 50/50** between season and match-by-match, suggesting a mix of committed season hospitality fans and occasional premium-experience buyers.
- **Individuals represent the majority of unique purchasers (181 vs 75)** but contribute only 22.4% of volume. They are numerically dominant but commercially secondary.

### Commercial Implication

The sales and marketing approach should reflect this reality:

- **Primary focus**: Corporate account acquisition and retention (where 78% of volume sits). These are B2B sales relationships, not consumer marketing targets.
- **Secondary focus**: Growing the individual customer base to reduce corporate dependency. The 50/50 season-vs-match split among individuals suggests an opportunity to convert match-by-match buyers into season holders.
- **Product development**: Packages and pricing are structured around corporate table/box allocations. Individual options are complementary but could be expanded.

---

## 3. Repeat Behaviour Is Exceptionally Strong

### The Numbers

| Metric | Value |
|--------|-------|
| Repeat purchasers (>1 transaction) | 241 (94.1%) |
| One-time purchasers | 15 (5.9%) |
| Tickets from repeat purchasers | 43,289 (100.0%) |
| Tickets from one-time purchasers | 0 (0.0%) |

| By Type | Total | Repeat | Repeat Rate | Avg Transactions | Avg Events |
|---------|-------|--------|-------------|-----------------|------------|
| **Account** | 75 | 73 | 97.3% | 447.9 | 30.5 |
| **Customer** | 181 | 168 | 92.8% | 53.6 | 20.5 |

### What This Means

- **94.1% of all purchasers are repeat buyers** — this is an extraordinarily high loyalty signal. The hospitality product creates recurring, relationship-driven revenue.
- **100% of ticket volume comes from repeat purchasers.** The 15 one-time purchasers collectively bought zero additional tickets — their transactions are likely very recent single-event purchases or trial buyers who haven't yet returned.
- **Accounts attend an average of 30.5 events** out of 54 total — over half of all available matches. These are season-long corporate partners, not occasional users.
- **Individual Customers attend 20.5 events on average** — still substantial. Even the individual fan base shows strong repeat engagement.

### Commercial Implication

This is a **loyalty-anchored revenue model**. The primary revenue risk is not acquisition failure — it is churn from key accounts. Retention strategy should include:

- Season-long packages and early renewal incentives
- Dedicated hospitality relationship management
- Post-event feedback loops to maintain satisfaction
- Recognition programmes for long-standing accounts

---

## 4. Gifting/Hosting — Data Limitation, Not Absence

### The Numbers (As Recorded)

| Metric | Count | % |
|--------|-------|---|
| Self-purchase (purchaser = owner) | 42,142 | 97.4% |
| Gifted/hosted (purchaser ≠ owner) | 1,147 | 2.6% |

| Purchaser Type | Total Tickets | Gifted | Gifted Rate |
|---------------|--------------|--------|-------------|
| **Account** | 33,594 | 0 | 0.0% |
| **Customer** | 9,695 | 1,147 | 11.8% |

**When tickets are gifted (Customer → Owner):**

| Flow | Tickets | % of Gifted |
|------|---------|-------------|
| Customer → Customer | 779 | 67.9% |
| Customer → Account | 368 | 32.1% |

### What This Actually Means

**The 97.4% "self-purchase" figure is misleading.** It does not mean 97.4% of hospitality attendees bought their own ticket.

The data shows that every single Account has exactly 1 unique owner — itself. When a corporate Account buys 113 seats for a match, all 113 are recorded with `owner_id = purchaser_id` (the Account). The actual humans occupying those seats — clients, guests, staff, partners — are invisible in this data model.

This means:

- **For Accounts (77.6% of tickets)**: We genuinely cannot determine from this data whether seats are used for client hosting, staff perks, or personal attendance by company directors. The CRM does not track end-users at the seat level for corporate hospitality packages.
- **For individual Customers (22.4%)**: The gifting signal is real. 11.8% of individual tickets have a different owner, split between other individuals (68%) and business accounts (32%). This represents genuine ticket-sharing — likely friends, family, or personal networks.

### What We Can Infer (But Not Prove)

Given that Accounts:
- Hold **season-long hospitality packages** with 16–113 seats per match
- Sit in **premium hospitality areas** (The Pavilion, Tony Currie Suite, 1889 Restaurant)
- Are **business entities** with no age, no gender, and "Unspecified" demographics

It is commercially reasonable to assume a significant proportion of Account seats are used for **client entertainment and corporate hosting** — this is the standard use case for premium hospitality suites in professional football. However, **this cannot be confirmed from the ticket data alone**.

### Recommendation

This is the single most important data gap in the analysis:

1. **Investigate with the CRM/ticketing team** whether `owner_id` can be extended to track individual guests for Account seats
2. **Post-event surveys or check-in data** would reveal the true guest-vs-staff split
3. **If corporate hosting is confirmed at scale**, the gifting rate is not 2.6% — it could be 50%+ of all tickets, fundamentally changing the strategic picture

---

## 5. Season-over-Season Trends

### The Numbers

| Metric | 2024/25 | 2025/26 |
|--------|---------|---------|
| Tickets | 23,355 | 19,711 |
| Unique Purchasers | 204 | 155 |
| Avg Tickets/Purchaser | 114.5 | 127.2 |
| Account % | 80.0% | 75.6% |
| Repeat Rate | 95.1% | 96.1% |
| Gifted Rate | 1.8% | 3.7% |
| Top 10 Concentration | 43.3% | 44.4% |

*(2023/24 excluded from comparison — only 223 tickets from a single purchaser, likely pre-season or residual data.)*

### What This Means

- **Ticket volume declined by 15.6%** (23,355 → 19,711) from 2024/25 to 2025/26. However, the 2025/26 season may be incomplete at the time of analysis, so this may not reflect a true decline.
- **Fewer purchasers but higher spend per buyer.** Average tickets per purchaser rose from 114.5 to 127.2, suggesting the remaining buyers are deepening their commitment.
- **Account share decreased slightly** from 80.0% to 75.6%. Individual Customer participation is growing marginally, which is a healthy diversification signal.
- **Repeat rate is stable and high** (95.1% → 96.1%). Retention is not declining.
- **Gifting rate doubled** from 1.8% to 3.7%. Still small in absolute terms but trending upward — could indicate growing social/referral behaviour.
- **Concentration is stable** (43.3% → 44.4%). The top 10 share is not worsening but remains high.

### Commercial Implication

The hospitality product shows a **stable, loyal core** with early signs of healthy diversification. The priority should be:

1. Reversing the purchaser count decline — fewer buyers is a warning signal even if per-buyer spend is rising
2. Accelerating the small shift toward individual Customer growth
3. Monitoring whether the gifting trend continues — this could be an untapped referral channel

---

## 6. Behavioural Segmentation (KMeans Clustering)

### Approach

To formalise the purchasing patterns observed above, a KMeans clustering was applied to all 256 purchasers (Accounts and Customers combined). Four behavioural features were used — all purely transactional, requiring no demographic data:

1. **log_tickets** — log-transformed total tickets purchased
2. **transaction_count** — number of distinct transactions
3. **unique_areas** — diversity of hospitality areas used
4. **unique_events** — number of distinct events attended

Features were standardised (z-scored) before clustering. An elbow plot (k = 2–8) confirmed k = 4 as the optimal choice, with diminishing inertia reductions beyond that point.

### The Four Segments

| Segment | Purchasers | Total Tickets | Avg Tickets | Avg Events | % Accounts | Ticket Share |
|---------|-----------|---------------|-------------|------------|------------|-------------|
| **Anchor Buyers** (high-volume repeat) | 8 | 16,369 | 2,046 | 46 | 100% | 37.8% |
| **Core Regulars** (mid-volume steady) | 154 | 24,407 | 159 | 33 | 32.5% | 56.4% |
| **Occasional Buyers** (low-volume repeat) | 14 | 2,041 | 146 | 26 | 42.9% | 4.7% |
| **Light Touch** (minimal engagement) | 80 | 472 | 6 | 2 | 13.8% | 1.1% |

### What This Means

- **Anchor Buyers (8 purchasers, 37.8% of tickets):** All 8 are corporate Accounts. These are the super-buyers identified in Section 1 — season-long hospitality package holders purchasing thousands of tickets each across 46+ events on average. They are the revenue anchor. Losing even one would create a material shortfall.
- **Core Regulars (154 purchasers, 56.4% of tickets):** The largest segment by headcount and the biggest by ticket share. A mix of Accounts (32.5%) and individual Customers (67.5%), attending 33 events on average. This is the commercial backbone — reliable, mid-volume, cross-type. The blended Account/Customer composition means retention strategies need to work for both corporate renewals and individual loyalty.
- **Occasional Buyers (14 purchasers, 4.7% of tickets):** A small group buying a similar per-person volume to Core Regulars (~146 avg tickets) but attending fewer events (26 vs 33). They may be seasonal hospitality buyers, corporate clients who activate for specific fixtures, or individuals with inconsistent commitment. The 43% Account share suggests a mix of both.
- **Light Touch (80 purchasers, 1.1% of tickets):** 80 purchasers contributing just 472 tickets — an average of 6 tickets and 1.5 events each. Overwhelmingly individual Customers (86%). These are trial buyers, one-off experience seekers, or lapsed purchasers. Individually marginal, but collectively they represent the largest conversion opportunity: moving even a fraction into Core Regulars would meaningfully diversify the base.

### Commercial Implication

The clustering quantifies what the earlier sections hinted at:

1. **The "Anchor → Core" pipeline is the revenue engine.** Together these two segments account for 94.2% of all tickets from just 162 purchasers (63% of the base).
2. **The Light Touch segment is the growth pool.** 80 purchasers buying an average of 6 tickets each is a conversion challenge, not a lost cause. Multi-match taster packages, group hospitality offers, and targeted post-first-visit follow-ups could shift some of these into Core Regulars.
3. **Retention priority is clear**: Anchor Buyers need key account management; Core Regulars need loyalty rewards and renewal incentives; Light Touch need onboarding journeys.
4. **Occasional Buyers are a monitoring segment**: their mid-volume but variable engagement may respond to fixture-specific marketing or flexible mini-season packages.

---

## 7. Strategic Summary

### Revenue Health Assessment

| Dimension | Status | Risk Level |
|-----------|--------|-----------|
| Revenue concentration | Top 10 = 42% of tickets | **High** |
| Corporate dependency | Accounts = 77.6% of volume | **High** |
| Repeat loyalty | 94.1% repeat rate | **Strong** (low risk) |
| Gifting/hosting | 2.6% recorded — but Account end-users are untracked | **Unknown** (data gap) |
| Season trend | Stable concentration, slight purchaser decline | **Medium** |
| Segmentation | 4 clear behavioural clusters confirmed by KMeans | **Actionable** |

### The Core Story

Sheffield United FC's hospitality operation is a **high-concentration, corporate-led, loyalty-driven product** built on season-long corporate hospitality packages:

1. **Strength**: Exceptional repeat purchase rates (94%) and season-long commitments (83% of Account tickets are season packages) indicate deep, contractual relationships — not transactional purchasing.
2. **Vulnerability**: Revenue is dangerously concentrated among a small number of corporate accounts. Five buyers control 29% of all tickets. These are not resellers — they are direct corporate clients, which means the club has a direct relationship to protect.
3. **Opportunity**: The individual Customer segment (22.4% of volume) is under-developed. With 181 individual purchasers split 50/50 between season and match-by-match purchases, there is clear headroom to convert casual buyers into committed season holders.
4. **Segmentation validates strategy**: KMeans clustering (k=4) formalised four distinct commercial segments — Anchor Buyers (8 corporate super-accounts, 37.8% of tickets), Core Regulars (154 mixed-type steady buyers, 56.4%), Occasional Buyers (14, 4.7%), and Light Touch (80 minimal-engagement buyers, 1.1%). Each segment demands a different retention/growth approach.
5. **Critical data gap**: The CRM does not track who actually occupies Account seats. If corporate hosting is the dominant use case (as is typical for hospitality suites), the club is missing insight into potentially thousands of end-users who experience the product but are invisible in the data. These "hidden guests" are an untapped marketing and conversion opportunity.

### Recommended Actions

| Priority | Action | Expected Impact |
|----------|--------|----------------|
| **Critical** | Implement key account management for top 20 buyers | Protect 58.5% of revenue |
| **Critical** | Investigate `owner_id` data capture for Accounts | Unlock insight into thousands of hidden end-users |
| **High** | Develop individual Customer growth strategy | Reduce concentration risk |
| **High** | Design segment-specific retention playbooks | Anchor Buyers → key account mgmt; Core Regulars → loyalty rewards; Light Touch → onboarding journeys |
| **Medium** | Create multi-match taster packages for Light Touch segment | Convert 80 low-engagement buyers into Core Regulars |
| **Medium** | Create multi-match loyalty packages | Deepen individual repeat behaviour |
| **Medium** | Introduce referral incentives | Capitalise on growing gifting trend |
| **Low** | Monitor 2025/26 season completion data | Validate apparent volume decline |

---

