---
title: "DIY Family Budget Tracker: Google AppSheet as Interface Meets Google Sheets as Database"
author: ivan
date: 2024-07-25 12:22:21 +0100
categories: [Programming]
tags: [Fundamentals]
toc: true
mermaid: true
math: true
---

## Introduction, Context and Feature scoping

### Introduction

My fiancée and I had been happily using [Splitwise](https://www.splitwise.com/) to manage our family budget for years. But the latest update has made it nearly unusable: only three entries per day are allowed, and each entry is followed by a 10-second unskippable ad. To bypass these restrictions, you now have to subscribe to the Pro version for $40 a year.

How to solve this?

1. Pay $40
2. Look for other free alternatives
3. Self-host any of the already existing open-source projects
4. ... Literally anything else ...
5. Build your own version of Splitwise, with blackjack and hookers, since it would only take you a weekend anyway

If you picked option 5, you're in the right place.

### Context

Our family doesn't adhere to a conventional shared budget. Instead, we utilize a "`matching expenditure budget`". What does this mean? Instead of establishing a fixed budget and monitoring expenses against it, we track every household expenses and who covered each cost. The essential characteristic of this approach is that both partners aim to balance their spending contributions the total house expenditure, ensuring their net spending is close to zero.

If this sounds complex, it is not, here is graph:

```mermaid
sequenceDiagram
    participant A as Person A
    participant B as Person B
    participant Budget as Budget Tracker

    A->>Budget: Buys for $100
    Note over A,Budget: A spends $100, B owes $50 (half of $100)
    Budget-->>B: B owes $50
    B->>Budget: Buys for $25
    Note over B,Budget: B spends $25, now owes $25 (50 - 25)
    Budget-->>B: B owes $25
    A->>Budget: Buys for $20
    Note over A,Budget: A spends $20, B now owes $35 (25 + 20/2)
    Budget-->>B: B owes $35
    B->>Budget: Buys for $80
    Note over B,Budget: B spends $80, A now owes $5 (35 - 80/2)
    Budget-->>A: A owes $5
    A->>Budget: Continue Cycle?
    Budget-->>A: Yes
    A->>Budget: Buys for $100
    Note over A,Budget: A spends $100, B owes $47.50 ((100-5)/2)
    Budget-->>B: B owes $47.50
    B->>Budget: Buys for $25
    Note over B,Budget: B spends $25, now owes $22.50 (47.50 - 25)
    Budget-->>B: B owes $22.50
    A->>Budget: Continue Cycle?
    Budget-->>A: No
    A->>B: Settle All Debts
    Note over A,B: A and B settle the $22.50 debt
    B->>A: All debts settled (Money transfer)
    A->>Budget: Start from $0
    Budget-->>A: $0
    Budget-->>B: $0
```