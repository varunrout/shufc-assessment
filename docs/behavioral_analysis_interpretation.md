# Behavioural Analysis  - Interpretation & Findings

## Overview

Interpretation of `02_behavioral_analysis.ipynb`. Working with 43,289 tickets across 54 events, 256 unique purchasers and 276 unique owners.

## First things first: what are "Accounts"?

This tripped me up initially. You might assume `customer_type = 'Account'` means reseller/distributor accounts (Ticketmaster-type intermediaries buying blocks). But the data says otherwise:

- 83.1% of Account tickets are season ticket holders  - these are season-long commitments, not resale blocks
- They sit in The Pavilion (58%), Tony Currie Suite (22%), 1889 Restaurant (20%)  - premium hospitality areas, not general admission
- Top accounts buy 16-113 seats per match across 39-48 events  - fixed multi-seat allocations, consistent all season
- All 75 Accounts have `gender = Unspecified` and `age = null`  - business entities
- Every Account has exactly 1 unique owner (itself), 100% self-purchase

So these are corporate hospitality clients  - businesses holding season-long packages with a fixed number of premium seats at every home match. They buy for staff entertainment, client hosting, corporate relationship-building. Not resellers, not individuals using a business account.

The `owner_id = purchaser_id` thing is important: it doesn't mean seats go unused. It means the CRM records the corporate entity as the owner, not the actual people sitting in those seats. When Account #1 buys 113 seats, those are almost certainly occupied by a mix of staff, clients, and guests  - but the data doesn't tell us who. This matters a lot for how we read the gifting numbers later.

## 1. Revenue concentration

The top 5 purchasers account for 29% of all tickets. Top 10 get you to 42%. Top 20 takes you to 58.5%. The Gini coefficient is 0.761, which is extremely concentrated.

The bottom 50% of purchasers? Just 4.4% of tickets. Mean tickets per purchaser (169) is 3.7x the median (46)  - classic right-skew from a few super-buyers.

Every single top-10 purchaser is a corporate Account. The #1 buyer alone controls 10.2% of all hospitality tickets  - roughly 113 seats per match across 39 events. Several of the top accounts buy exactly 16 or 36 seats per event, which looks like standardised packages (a table, two tables, etc.)  - contractual rather than ad-hoc.

| Rank | Type | Tickets | % of Total | Cumulative % |
|------|------|---------|-----------|-------------|
| 1 | Account | 4,402 | 10.2% | 10.2% |
| 2 | Account | 3,008 | 6.9% | 17.1% |
| 3 | Account | 1,740 | 4.0% | 21.1% |
| 4 | Account | 1,728 | 4.0% | 25.1% |
| 5 | Account | 1,692 | 3.9% | 29.0% |
| 6 | Account | 1,404 | 3.2% | 32.2% |
| 7 | Account | 1,220 | 2.8% | 35.0% |
| 8 | Account | 1,175 | 2.7% | 37.7% |
| 9 | Account | 960 | 2.2% | 39.9% |
| 10 | Account | 864 | 2.0% | 41.9% |

This is fundamentally a key account management problem. The hospitality revenue is structurally dependent on a handful of corporate relationships. If even one of the top 5 churned, it would be a material shortfall. Formal retention programme for the top 20 isn't optional.

## 2. Corporate-dominant product

75 corporate Accounts generate 77.6% of all tickets (avg 448 per account). 181 individual Customers produce the remaining 22.4% (avg 54 each). The hospitality product is B2B at its core.

83% of Account tickets are season-long commitments  - not ad-hoc bookings. Accounts buy 8.4x more per purchaser than individuals, which makes sense structurally: a corporate account holds a table or box with multiple seats at every match, while an individual buys single seats.

Individuals split about 50/50 between season and match-by-match purchasing. There are more individual purchasers by headcount (181 vs 75) but they're commercially secondary by volume.

What this means for sales strategy: the primary focus has to be corporate account acquisition and retention  - that's where 78% of volume sits, and these are B2B relationship sales, not consumer marketing. Growing the individual base is the secondary play, and the 50/50 season-vs-match split suggests there's room to convert match-by-match buyers into season holders.

## 3. Repeat behaviour

94.1% of purchasers are repeat buyers. 100% of ticket volume comes from repeat purchasers. The 15 one-time buyers collectively contributed zero additional tickets  - likely recent single-event purchases or trials.

Accounts attend an average of 30.5 out of 54 events. Individual Customers average 20.5 events. Both numbers are high  - this is a loyalty-anchored product. The main revenue risk isn't acquisition failure, it's churn from established accounts.

## 4. Gifting and hosting

On paper: 97.4% self-purchase (purchaser = owner), only 2.6% gifted. But this is misleading.

Every Account has exactly 1 unique owner  - itself. So when an Account buys 113 seats, all 113 show up as "self-purchase" even though the actual occupants are clients, guests, and staff. For the 77.6% of tickets held by Accounts, we genuinely can't determine from this data who actually sat in the seat.

For individual Customers it's more meaningful: 11.8% of their tickets have a different owner. Of those, 68% go to other individuals (friends, family) and 32% go to business accounts. That's real ticket-sharing.

Given that Accounts hold season-long packages in premium hospitality areas, it's commercially reasonable to assume a big chunk of those seats go toward client entertainment. That's what hospitality suites are for. But we can't confirm it from the data.

This is the biggest data gap in the entire analysis. If the CRM could track individual guests for Account seats (via `owner_id` extension, post-event surveys, or check-in data), the real gifting/hosting rate could be 50%+ rather than 2.6%. That would fundamentally change the strategic picture.

## 5. Season trends

Comparing 2024/25 to 2025/26 (excluding 2023/24 which had only 223 tickets from one purchaser  - probably pre-season residual):

| Metric | 2024/25 | 2025/26 |
|--------|---------|---------|
| Tickets | 23,355 | 19,711 |
| Unique purchasers | 204 | 155 |
| Avg tickets/purchaser | 114.5 | 127.2 |
| Account share | 80.0% | 75.6% |
| Repeat rate | 95.1% | 96.1% |
| Gifted rate | 1.8% | 3.7% |

Volume is down 15.6% but the 2025/26 season might be incomplete at time of analysis, so don't read too much into the absolute decline. More interesting: fewer purchasers but higher spend per buyer, suggesting the remaining base is deepening commitment. Account share dropped slightly (healthy diversification). Repeat rate is rock-solid. Gifting doubled  - still small but worth watching as a potential referral channel.

## 6. Behavioural segmentation (KMeans)

Applied KMeans clustering to all 256 purchasers using four transactional features: log-transformed ticket volume, transaction count, unique areas, and unique events. All standardised before clustering. Elbow plot pointed to k=4.

The four segments:

| Segment | n | Tickets | Avg tickets | Avg events | % Accounts | Ticket share |
|---------|---|---------|------------|-----------|-----------|-------------|
| Anchor Buyers | 8 | 16,369 | 2,046 | 46 | 100% | 37.8% |
| Core Regulars | 154 | 24,407 | 159 | 33 | 32.5% | 56.4% |
| Occasional Buyers | 14 | 2,041 | 146 | 26 | 42.9% | 4.7% |
| Light Touch | 80 | 472 | 6 | 2 | 13.8% | 1.1% |

Anchor Buyers are the 8 corporate super-accounts  - all the top purchasers from section 1. 37.8% of all tickets from just 8 entities. Losing one would hurt.

Core Regulars are the backbone: 154 purchasers (mix of 32.5% accounts and 67.5% individuals), 33 events average, 56.4% of ticket share. Reliable mid-volume buyers. Retention strategies here need to work for both corporate renewals and individual loyalty since the segment is mixed.

Occasional Buyers (14 purchasers) look similar to Core Regulars per-person (~146 avg tickets) but attend fewer events. Could be seasonal buyers or fixture-specific corporate activations.

Light Touch is the conversion opportunity: 80 purchasers (86% individuals) buying an average of 6 tickets each. Trial buyers, one-off experience seekers. Individually marginal but collectively the largest growth pool. Multi-match taster packages or post-first-visit follow-ups could shift some of these up.

The Anchor + Core pipeline accounts for 94.2% of tickets from 63% of the base. That's the revenue engine. Light Touch is where future growth comes from.

## 7. Pulling it together

The hospitality operation is high-concentration, corporate-led, loyalty-driven. Built on season-long corporate packages.

Strengths: 94% repeat rate, 83% of Account tickets on season commitments  - these are deep contractual relationships. The loyalty base is genuinely strong.

Vulnerability: revenue dangerously concentrated. Five buyers control 29% of tickets. They're direct corporate clients (not resellers), so the club has the relationship to protect  - but also the responsibility to.

Opportunity: individual Customers are under-developed at 22.4% of volume. 181 purchasers split 50/50 between season and match-by-match. There's headroom to convert casual buyers into committed season holders.

The biggest data gap: the CRM doesn't track who sits in Account seats. If corporate hosting is the dominant use case (standard for hospitality suites), the club is missing insight into thousands of end-users who experience the product but are invisible in the data. These hidden guests could be a major marketing and conversion opportunity.

### What to do about it

In rough priority order:

1. Key account management for the top 20 buyers  - protects ~58.5% of revenue
2. Investigate whether `owner_id` can be extended to capture individual guests for Accounts
3. Build an individual Customer growth strategy to reduce concentration risk
4. Design segment-specific approaches: key account management for Anchors, loyalty/renewal for Core Regulars, onboarding journeys for Light Touch
5. Multi-match taster packages for the Light Touch segment
6. Referral incentives to capitalise on the growing gifting trend
7. Keep an eye on 2025/26 completion data before concluding volume is actually declining

