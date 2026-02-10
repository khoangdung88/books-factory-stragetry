# BOOK FACTORY - DOCUMENT INDEX & READING GUIDE

**Version:** 1.0
**Created:** 2026-02-07
**Status:** Active

---

## EXECUTIVE SUMMARY

The AI Book Production Engine is an automated, data-driven publishing business that:
- Identifies profitable niches via market intelligence
- Produces quality non-fiction ebooks using a multi-agent AI pipeline with human review gates
- Publishes on Amazon KDP (primary) with planned expansion to Kobo, Google Play, and direct sales
- Optimizes continuously through sales data, reviews, and A/B testing

**Business model:** Produce ebooks at ~$20/book, sell at $4.99 ($3.49 net royalty), break even at 20 copies. Portfolio strategy: 100 books Year 1, power-law distribution drives profitability.

**Tech stack:** Next.js 14 + Supabase + Multi-AI provider (Groq/Gemini/OpenAI/Anthropic) + DALL-E 3 for covers.

**Capital required:** $2,500 minimum, $5,000 recommended. Cash-flow positive by Month 5-7.

---

## DOCUMENT MAP

| # | Document | Domain | Description |
|---|----------|--------|-------------|
| 01 | [01_VISION.md](01_VISION.md) | Strategy | Vision, legal structure, business model, competition, architecture overview, niche selection, marketing strategy, KPIs, roadmap, risks, design principles |
| 02 | [02_ECONOMICS.md](02_ECONOMICS.md) | Finance | **Single source for ALL cost & revenue data.** Per-book costs, revenue model, break-even, cash flow, tax strategy, financial KPIs, team costs |
| 03 | [03_PIPELINE.md](03_PIPELINE.md) | Technical | Pipeline architecture, data flow per stage, data model (SQL schema), state machine, error handling, security, QA framework, scaling architecture |
| 04 | [04_AGENTS.md](04_AGENTS.md) | Technical | Agent specifications (11 agents), orchestration flow, human gates, versioning, SLAs, cost governance |
| 05 | [05_PROMPTS.md](05_PROMPTS.md) | Technical | Production-ready prompts for all agents, versioning strategy, A/B testing framework, prompt engineering best practices |
| 06 | [06_SERVICES.md](06_SERVICES.md) | Vendor | Service dependency map, vendor profiles & alternatives, vendor management framework, cost optimization, failover design, partnership strategy |
| 07 | [07_OPERATIONS.md](07_OPERATIONS.md) | Operations | Org structure, RACI matrix, SOPs (9 procedures), daily/weekly/monthly schedules, operational KPIs, incident response, reporting templates |
| 08 | [08_MVP_CHECKLIST.md](08_MVP_CHECKLIST.md) | Execution | Day-by-day 28-day implementation plan, pre-launch setup, weekly milestones, success criteria, post-MVP priorities |
| 09 | [09_Books.md](09_Books.md) | Content | Book types, content density, technical execution, Amazon rules, coder checklist |

---

## READING PATHS

### Founder / Business Owner
> Understand the full business before building.

1. **00_INDEX.md** (this file) - Overview
2. **01_VISION.md** - Strategy, market, legal, roadmap
3. **02_ECONOMICS.md** - Financial model, costs, revenue, cash flow
4. **08_MVP_CHECKLIST.md** - 28-day execution plan
5. **07_OPERATIONS.md** - How daily operations work

### Developer / Technical Builder
> Understand what to build and how it works.

1. **00_INDEX.md** (this file) - Overview
2. **03_PIPELINE.md** - Architecture, data model, error handling
3. **04_AGENTS.md** - Agent specs, orchestration, SLAs
4. **05_PROMPTS.md** - Prompt implementations
5. **06_SERVICES.md** - Vendor integration, failover design

### Content Operator / QA Editor
> Understand daily workflow and quality standards.

1. **00_INDEX.md** (this file) - Overview
2. **07_OPERATIONS.md** - SOPs, schedules, review checklists
3. **08_MVP_CHECKLIST.md** - What's being built and when
4. **01_VISION.md** - Section 5.5 (Niche Selection) for editorial context

---

## DOMAIN OWNERSHIP

Each topic has **one canonical document**. All other documents reference it instead of duplicating.

| Topic | Owner | Others Reference |
|-------|-------|-----------------|
| Vision, strategy, roadmap | 01_VISION | all |
| All financial data, costs, revenue | 02_ECONOMICS | 01, 06, 08 |
| Pipeline architecture, data model | 03_PIPELINE | 01, 04, 06 |
| Agent specifications | 04_AGENTS | 03, 05 |
| Prompt templates | 05_PROMPTS | 04 |
| Vendor/service ecosystem | 06_SERVICES | 01, 02, 03, 04 |
| SOPs, org structure, operations | 07_OPERATIONS | 01, 08 |
| Execution checklist | 08_MVP_CHECKLIST | 07 |
| Niche selection framework | 01_VISION | 03, 04 |
| Quality gates / QA framework | 03_PIPELINE | 07, 08 |
| Marketing strategy | 01_VISION | 02, 08 |
| Team costs | 02_ECONOMICS | 01, 07 |
| Risk management (vendor) | 06_SERVICES | 01, 03 |

---

## GLOSSARY

| Term | Definition |
|------|------------|
| ACOS | Advertising Cost of Sales — ad spend / ad-attributed revenue |
| Agent | Autonomous AI module with a specific task in the pipeline |
| ASIN | Amazon Standard Identification Number |
| BISAC | Book Industry Standards and Communications (category codes) |
| BSR | Best Seller Rank (Amazon) |
| KDP | Kindle Direct Publishing (Amazon's self-publishing platform) |
| KENP | Kindle Edition Normalized Pages (used for KU payouts) |
| KU | Kindle Unlimited (subscription reading program) |
| Niche Score | Composite metric (0-10) for market opportunity |
| Quality Gate | Checkpoint requiring human approval to proceed in pipeline |
| RACI | Responsible, Accountable, Consulted, Informed (role matrix) |
| SOP | Standard Operating Procedure |

---

## TOOL RECOMMENDATIONS

| Category | Tool | Purpose | Cost |
|----------|------|---------|------|
| Keyword Research | Publisher Rocket | Amazon keyword data | $97 one-time |
| Plagiarism Check | Copyscape | Detect copied content | $0.03/search |
| EPUB Formatting | Calibre | EPUB creation & validation | Free |
| EPUB Validation | Kindle Previewer | Amazon format testing | Free |
| Cover Design | DALL-E 3 / Ideogram | AI cover generation | $0.04-0.08/image |
| Cover Text Overlay | Canva / ImageMagick | Add title text to covers | Free-$13/mo |
| Sales Analytics | KDP Dashboard | Sales tracking, BSR, royalties | Free |
| Advertising | Amazon Ads Console | Sponsored Products campaigns | Free (pay per click) |
| Email Marketing | ConvertKit | Reader email list | Free < 1,000 subs |
| Bookkeeping | Wave Accounting | Financial tracking | Free |
| Grammar Check | LanguageTool | Grammar & style | Free API (limited) |

---

*Version: 1.0*
*This is the starting point for all readers. When in doubt about where something lives, check the Domain Ownership table above.*
