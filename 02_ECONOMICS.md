# AI BOOK PRODUCTION ENGINE
## ECONOMICS MODEL & FINANCIAL GOVERNANCE

**Document Version:** 3.0
**Last Updated:** 2026-02-07
**Status:** Active
**Reading Order:** 02 of 08 — See [00_INDEX.md](00_INDEX.md) for full document map

> **This is the SINGLE canonical source for all financial data.** All other documents reference here for costs, revenue, and financial projections.

---

# 1. MỤC TIÊU TÀI CHÍNH

## 1.1 Realistic Targets by Phase

| Phase | Timeline | Revenue Target | Profit Target |
|-------|----------|---------------|---------------|
| MVP Validation | Month 1-3 | $500-2,000 | Break-even |
| Growth | Month 4-6 | $2,000-5,000/mo | $1,000-3,000/mo |
| Scale | Month 7-12 | $5,000-15,000/mo | $3,000-10,000/mo |
| Industrial | Year 2 | $15,000-40,000/mo | $10,000-30,000/mo |
| Mature | Year 3+ | $40,000-100,000/mo | $25,000-70,000/mo |

> **Note:** Mục tiêu $1M/năm revenue là khả thi ở Year 3+ với 500+ books và multi-channel distribution. Không nên đặt mục tiêu này cho Year 1.

## 1.2 Financial Principles
1. **Unit economics first:** Mỗi sách phải profitable trước khi scale
2. **Cash flow positive:** Không bao giờ chi nhiều hơn revenue tháng trước
3. **Marketing is investment:** Tracking ROAS cho mỗi dollar marketing
4. **Kill losers fast:** 30-day evaluation window per book
5. **Reinvest winners:** Double down on proven niches

---

# 2. CHI PHÍ SẢN XUẤT (COST MODEL)

## 2.1 Per-Book Production Cost

### MVP Phase (Optimized Free Tiers)
| Component | Cost | Notes |
|-----------|------|-------|
| LLM API (writing) | $3-8 | Groq free + Gemini free + OpenAI mini |
| LLM API (analysis) | $0.50-1.50 | Quality check, metadata |
| Image generation | $1-2 | DALL-E 3 (5 cover variants) |
| Plagiarism check | $0.50-1.00 | Copyscape or similar |
| Hosting (amortized) | $0.50 | Vercel + Supabase free tiers |
| Human editing | $8-12 | 1-1.5 hours at $8-10/hr |
| **Total Production** | **$14-25** | **Target: $20 average** |

### Scale Phase (Paid Tiers)
| Component | Cost | Notes |
|-----------|------|-------|
| LLM API (writing) | $2-5 | Volume pricing, prompt optimization |
| LLM API (analysis) | $0.30-1.00 | Cached prompts |
| Image generation | $1-2 | Bulk pricing |
| Plagiarism check | $0.30 | Subscription plan |
| Hosting (amortized) | $0.30 | Amortized over higher volume |
| Human editing | $5-8 | More efficient with templates |
| **Total Production** | **$9-17** | **Target: $15 average** |

> Chi tiết service-level cost breakdown: [06_SERVICES.md](06_SERVICES.md) Part 1.3 & Part 4.4

## 2.2 Per-Book Marketing Cost

| Expense | MVP Phase | Scale Phase | Notes |
|---------|-----------|-------------|-------|
| Amazon Ads (Month 1) | $30-50 | $20-40 | Discovery + optimization |
| Amazon Ads (Month 2+) | $0-30/mo | $10-50/mo | Only profitable books |
| Price promotions | $5-10 | $3-5 | Launch discount revenue loss |
| **Total Marketing (60 days)** | **$35-90** | **$33-95** | **Target: $50 avg** |

## 2.3 Fixed Monthly Costs

| Expense | MVP | Scale | Industrial |
|---------|-----|-------|-----------|
| Hosting (Vercel/Supabase) | $0-20 | $25-60 | $100-300 |
| Tools & subscriptions | $30-50 | $100-200 | $300-500 |
| Freelance QA editor | $200-400 | $800-2,000 | $3,000-6,000 |
| Marketing tools | $0-20 | $50-100 | $200-500 |
| Legal/accounting | $0 | $100-200 | $500-1,000 |
| Insurance | $0 | $30-50 | $50-100 |
| **Total Fixed/Month** | **$230-490** | **$1,105-2,610** | **$4,150-8,400** |

---

# 2.5 TEAM COSTS BY PHASE

## Level 1 – MVP (Month 1-3)

### Team (1-2 people)
| Vai trò | Trách nhiệm | Full/Part-time |
|---------|-------------|----------------|
| Founder/Builder | Architecture + development + operations | Full-time |
| Content QA | Review + edit AI output | Part-time (10h/week) |

### Monthly team cost: $200-400 (QA only; founder is unpaid equity)

## Level 2 – Scale Engine (Month 4-12)

### Team (3-5 people)
| Vai trò | Trách nhiệm |
|---------|-------------|
| Founder/CEO | Strategy + finance + partnerships |
| Full-stack dev | Platform development + maintenance |
| Content operator | Pipeline management + QA oversight |
| Marketing ops | Amazon Ads + SEO + analytics |
| QA editor (PT) | Content review + quality gate |

### Monthly team cost: $3,000-8,000

## Level 3 – Industrial Factory (Year 2+)

### Team (~10-15 people)
| Vai trò | Trách nhiệm |
|---------|-------------|
| CEO | Strategic direction |
| CTO | Technology + infrastructure |
| Head of Content | Content quality + editorial standards |
| Data analyst | Market intelligence + optimization |
| 2-3 Content operators | Pipeline management |
| Marketing manager | Multi-channel marketing |
| 2-3 QA editors | Quality assurance |
| Dev team (2-3) | Platform + automation |

### Monthly team cost: $15,000-40,000

---

# 3. REVENUE MODEL

## 3.1 Revenue Streams

### Primary: Amazon KDP Ebook
```
Price: $4.99 (standard) / $7.99 (premium niche)
Royalty: 70% (books priced $2.99-$9.99)
Net per sale: $3.49 / $5.59

Delivery cost: ~$0.06-0.15 (deducted from royalty)
Effective net: $3.34 - $5.44
```

### Secondary: Kindle Unlimited (KENP)
```
Payment: ~$0.005 per page read
Average book: 200 KENP pages
Revenue per full read: ~$1.00
Note: KENP revenue is ADDITIONAL to sales for KDP Select books
```

### Tertiary: Paperback (KDP Print)
```
Price: $12.99 - $16.99
Royalty: 60% minus printing cost
Net per sale: $2-5 depending on page count
Note: Higher perceived value, different audience
```

### Future: Audiobook (ACX/Findaway)
```
Price: $14.99 - $24.99
Royalty: 40% (exclusive) / 25% (non-exclusive)
Net per sale: $3.75 - $10
Production cost: $50-200 per book (AI voice)
```

## 3.2 Revenue Per Book Scenarios (12-Month Lifetime)

### Ebook Only
| Metric | Pessimistic | Conservative | Optimistic | Best Case |
|--------|------------|-------------|-----------|-----------|
| Copies sold | 15 | 50 | 150 | 500 |
| KU page reads | 5 full reads | 20 | 50 | 200 |
| Ebook revenue | $50 | $167 | $501 | $1,670 |
| KU revenue | $5 | $20 | $50 | $200 |
| **Total revenue** | **$55** | **$187** | **$551** | **$1,870** |
| Production cost | $20 | $20 | $20 | $20 |
| Marketing cost | $30 | $50 | $70 | $100 |
| **Profit** | **$5** | **$117** | **$461** | **$1,750** |
| **ROI** | **10%** | **167%** | **512%** | **1458%** |

### Profitability by Volume (at $4.99 price, $70 total investment)
| Copies Sold | Revenue | Total Investment | Profit | ROI |
|-------------|---------|-----------------|--------|-----|
| 20 | $69.80 | $70 | -$0.20 | Break-even |
| 50 | $174.50 | $70 | $104.50 | 149% |
| 100 | $349.00 | $70 | $279.00 | 399% |
| 200 | $698.00 | $80 | $618.00 | 773% |
| 500 | $1,745.00 | $100 | $1,645.00 | 1645% |

---

# 4. BREAK-EVEN ANALYSIS

## 4.1 Per-Book Break-Even

```
Total investment per book: $70 (production $20 + marketing $50)

Break-even at $4.99 price (70% royalty = $3.49 net):
Break-even copies: 20 copies

Break-even timeline:
- Fast movers: 1-2 months
- Average: 3-6 months
- Slow movers: 6-12 months
- Losers (kill after 60 days if < 5 copies): Cut losses
```

## 4.2 Portfolio Break-Even

```
Assuming 100 books published Year 1:
Total investment: $7,000 (production) + $5,000 (marketing) + $5,000 (fixed) = $17,000

Break-even requires: $17,000 / $3.49 = 4,871 total copies
Per book average needed: 49 copies

This is achievable if top 20% of books perform well (power law distribution)
```

## 4.3 Year 1 Portfolio Projections

### Conservative
```
Books Published: 100
Distribution of Performance:
├── Top 10% (10 books):     avg 200 copies each = 2,000 copies
├── Top 25% (15 books):     avg 100 copies each = 1,500 copies
├── Middle 50% (50 books):  avg 30 copies each = 1,500 copies
├── Bottom 25% (25 books):  avg 10 copies each = 250 copies
└── Total Sales:            5,250 copies

Revenue: 5,250 × $3.49 = $18,323
Production Costs: 100 × $20 = $2,000
Marketing Costs: 100 × $50 avg = $5,000
Tools & Subscriptions: $1,200
Total Costs: $8,200

NET PROFIT YEAR 1: ~$10,000

Note: Top 10% of books drive 38% of revenue (power law)
```

### Optimistic (If model proven by Month 3)
```
Books Published: 200
Total Sales: 15,000 copies
Revenue: $52,350
Total Costs: $20,000
NET PROFIT: ~$32,000
```

---

# 5. CASH FLOW PROJECTION

## 5.1 Month-by-Month Cash Flow (Year 1 - Conservative)

| Month | Books Published | Cumulative | Revenue | Costs | Cash Flow | Cumulative CF |
|-------|----------------|------------|---------|-------|-----------|---------------|
| M1 | 10 | 10 | $50 | $700 | -$650 | -$650 |
| M2 | 10 | 20 | $200 | $850 | -$650 | -$1,300 |
| M3 | 10 | 30 | $500 | $900 | -$400 | -$1,700 |
| M4 | 10 | 40 | $900 | $1,100 | -$200 | -$1,900 |
| M5 | 10 | 50 | $1,400 | $1,200 | $200 | -$1,700 |
| M6 | 10 | 60 | $2,000 | $1,300 | $700 | -$1,000 |
| M7 | 10 | 70 | $2,500 | $1,400 | $1,100 | $100 |
| M8 | 10 | 80 | $3,000 | $1,500 | $1,500 | $1,600 |
| M9 | 10 | 90 | $3,500 | $1,600 | $1,900 | $3,500 |
| M10 | 10 | 100 | $3,800 | $1,700 | $2,100 | $5,600 |
| M11 | 0 (optimize) | 100 | $3,500 | $1,200 | $2,300 | $7,900 |
| M12 | 0 (optimize) | 100 | $3,200 | $1,100 | $2,100 | $10,000 |

**Key Insight:** Maximum negative cash flow = ~$1,900 (Month 4). Need $2,500 initial capital as buffer.

## 5.2 Working Capital Requirements

```
Minimum startup capital needed: $2,500
├── Production costs (first 2 months): $400
├── Marketing budget (first 2 months): $500
├── Fixed costs (first 2 months): $600
├── Tools & subscriptions setup: $200
├── Legal entity setup: $200
├── Buffer (30% contingency): $600
```

```
Recommended startup capital: $5,000
├── Covers 6 months of negative cash flow
├── Allows for marketing experimentation
├── Handles unexpected costs
├── Provides psychological safety margin
```

---

# 6. KPI TÀI CHÍNH & DASHBOARDS

## 6.1 Daily Monitoring
| Metric | Formula | Target |
|--------|---------|--------|
| Daily revenue | Sum of all platform revenue | Trend up |
| Daily ad spend | Sum of all ad campaigns | < daily revenue |
| ACOS | Ad spend / Ad-attributed revenue | < 50% |

## 6.2 Weekly Review
| Metric | Formula | Target |
|--------|---------|--------|
| Revenue per book | Total revenue / active books | > $15/week for winners |
| Cost per book | Total costs / books produced | < $25 |
| Pipeline throughput | Books completed / books started | > 85% |
| Quality pass rate | Books passing QA / total | > 90% |

## 6.3 Monthly Review
| Metric | Formula | Target |
|--------|---------|--------|
| Monthly profit | Revenue - All costs | Positive by M5 |
| Portfolio ROI | Total profit / Total investment | > 100% |
| Customer Acquisition Cost | Marketing spend / new readers | < $1 |
| Lifetime Value | Avg revenue per book (12 months) | > $150 |
| Book survival rate | Books still earning after 6 months | > 60% |
| Return rate | Refunds / sales | < 3% |

## 6.4 Financial Controls

### Spending Approval Matrix
| Amount | Approver | Documentation |
|--------|----------|---------------|
| < $100 | Auto-approved (within budget) | Logged in system |
| $100-500 | Operator approval | Email confirmation |
| $500-2000 | Owner approval | Written approval + justification |
| > $2000 | Owner + financial review | Formal proposal |

### Budget Alerts
| Trigger | Action |
|---------|--------|
| Daily spend > 120% of budget | Email alert to owner |
| Monthly spend > 90% of budget | Review all active campaigns |
| Per-book cost > $30 | Pause and investigate |
| ACOS > 70% for 7 days | Pause ad campaign |
| Revenue decline > 20% week-over-week | Investigation required |

---

# 7. TAX & ACCOUNTING

## 7.1 Tax Strategy

### For US LLC
- **Estimated tax payments:** Quarterly (April 15, June 15, Sept 15, Jan 15)
- **Self-employment tax:** 15.3% on net profit
- **Income tax:** Based on tax bracket
- **Deductible expenses:**
  - All LLM API costs
  - Hosting and tools
  - Freelancer payments (need W-9 from US, W-8BEN from non-US)
  - Home office (if applicable)
  - Internet (business portion)
  - Equipment (computer, monitor)
  - Professional development / books
  - Software subscriptions
  - Marketing and advertising costs

### For Non-US Entity
- **Amazon withholding:** 30% default, reducible with tax treaty
- **File W-8BEN-E** for LLC or W-8BEN for individual
- **Local tax obligations:** Consult local accountant

## 7.2 Bookkeeping System
- **Tool:** Wave (free) or QuickBooks Self-Employed ($15/mo)
- **Frequency:** Weekly reconciliation
- **Categories:**
  - Revenue (by platform)
  - Production costs (by book)
  - Marketing costs (by campaign)
  - Fixed costs (hosting, tools, subscriptions)
  - Freelancer payments
  - Tax payments

## 7.3 Record Keeping
- Keep ALL receipts (digital) for 7 years
- Monthly P&L statement
- Quarterly tax estimate calculation
- Annual summary for tax filing
- Per-book cost tracking in database

---

# 8. RỦI RO TÀI CHÍNH

## 8.1 Risk Scenarios

| Scenario | Impact | Probability | Mitigation |
|----------|--------|-------------|------------|
| All books fail (< 10 copies each) | Loss of $2,000-5,000 | Low | Kill fast, pivot niches |
| Amazon account banned | Revenue drops to 0 | Medium | Platform diversification, savings buffer |
| AI costs triple | Production cost increases 50% | Medium | Multi-provider, open-source fallback |
| Market saturation | Revenue per book drops 50% | High | Quality differentiation, new niches |
| Refund rate spike | Revenue loss + account risk | Low | Quality gates, accurate descriptions |
| Ad costs spike | ACOS becomes unprofitable | Medium | Organic optimization, pause ads |

## 8.2 Financial Safety Rules
1. **Never invest more than you can afford to lose** in the first 3 months
2. **Maintain 2-month expense buffer** in business account at all times
3. **Stop scaling** if per-book profit turns negative for 3 consecutive books
4. **Maximum monthly ad budget** = 30% of previous month's revenue
5. **Reinvestment cap:** 50% of profits reinvested, 50% taken as income (after Month 6)

---

# 9. ĐƯỜNG ĐẾN SCALE

## 9.1 Phase 1: Validation (Month 1-3)
```
Books: 30
Revenue: $750-2,000
Costs: $2,100-3,500
Profit: -$1,500 to -$500 (expected negative, validation phase)

Success Criteria:
- At least 5 books break even
- Average production cost < $25
- At least 1 book sells 50+ copies
- Amazon acceptance rate > 90%
- Total learnings documented
```

## 9.2 Phase 2: Growth (Month 4-6)
```
Books: 30 more (60 total)
Revenue: $5,000-10,000
Costs: $5,000-8,000
Profit: $0-$2,000

Success Criteria:
- Portfolio ROI turns positive
- Top 20% books identified and replicated
- Series started for 2-3 winning niches
- ACOS < 50% on profitable campaigns
- Second revenue channel started (Kobo or paperback)
```

## 9.3 Phase 3: Scale (Month 7-12)
```
Books: 40-60 more (100-120 total)
Revenue: $15,000-40,000
Costs: $10,000-20,000
Profit: $5,000-20,000

Success Criteria:
- Monthly revenue > $5,000
- Positive cash flow every month
- Team of 3+ working on production
- Direct sales channel generating 10%+ of revenue
- Audiobook testing started
```

## 9.4 Phase 4: Industrial (Year 2)
```
Books: 200+ more (300+ total)
Revenue: $100,000-300,000/year
Costs: $50,000-120,000/year
Profit: $50,000-180,000/year

Success Criteria:
- Monthly revenue > $15,000
- 3+ pen name brands with loyal followings
- Multi-platform distribution
- Team of 8-10
- Automated pipeline > 85%
```

---

# 10. CHIẾN LƯỢC TỐI ƯU CHI PHÍ

## 10.1 Production Cost Optimization
- **Prompt engineering:** Reduce tokens 20-30% through better prompts
- **Caching:** Cache niche analysis, common structures
- **Batch processing:** Run similar books in batches (shared niche research)
- **Template system:** Reusable outlines for common book types
- **Free tier maximization:** Rotate across free API tiers

> Chi tiết service cost optimization: [06_SERVICES.md](06_SERVICES.md) Part 4

## 10.2 Marketing Cost Optimization
- **Organic SEO first:** Perfect metadata before spending on ads
- **Keyword harvesting:** Use auto campaigns to discover cheap keywords
- **Dayparting:** Run ads during peak purchase hours only
- **Bid optimization:** Weekly bid adjustments based on ACOS
- **Cross-promotion:** Use back-of-book to sell other books (free marketing)

## 10.3 Revenue Optimization
- **Price testing:** A/B test $4.99 vs $6.99 vs $9.99
- **KDP Select strategy:** First 90 days exclusive for KU revenue boost
- **Series pricing:** Book 1 at $0.99, subsequent books at $4.99-6.99
- **Seasonal promotions:** Holiday sales, back-to-school, New Year resolution
- **Backlist optimization:** Refresh covers and descriptions for older books quarterly

---

*Document Version: 3.0*
*Previous Version: 2.0 (ECONOMICS_MODEL.md — now includes team costs and profitability scenarios)*
*Next Review: Monthly (with actual data)*
