# AI BOOK PRODUCTION ENGINE
## SERVICE ECOSYSTEM & VENDOR MANAGEMENT

**Document Version:** 3.0
**Last Updated:** 2026-02-07
**Status:** Active - Living Document
**Reading Order:** 06 of 08 — See [00_INDEX.md](00_INDEX.md) for full document map
**Connected Documents:** [01_VISION.md](01_VISION.md), [03_PIPELINE.md](03_PIPELINE.md), [04_AGENTS.md](04_AGENTS.md), [02_ECONOMICS.md](02_ECONOMICS.md)

https://docs.google.com/spreadsheets/d/1voB2-a4xG8xriNdAIPAAU1smlAmt3rfDe-QYEQpD6c8/edit?usp=sharing
---

# EXECUTIVE SUMMARY

## Tam nhin

Book Factory la mot **nha may san xuat so** — phu thuoc hoan toan vao he sinh thai dich vu SaaS/API ben ngoai de van hanh. Quan tri service ecosystem khong phai la "nice to have" — no la **yeu to song con** cua nha may.

**Tai sao document nay quan trong:**
1. **Chi phi san xuat = chi phi dich vu:** ~80% chi phi per book den tu API calls (AI, image, plagiarism check)
2. **Single point of failure:** Neu Groq down → pipeline dung. Neu Amazon thay doi policy → doanh thu = 0
3. **Cost volatility:** AI providers thay doi pricing lien tuc. Khong quan ly → chi phi co the tang 3-5x overnight
4. **Competitive advantage:** Ai quan ly vendor tot hon → chi phi thap hon → margin cao hon → scale nhanh hon

**Nha may hien tai su dung ~15-20 dich vu ben ngoai xuyen suot 10 buoc pipeline.**

---

# PART 1: SERVICE DEPENDENCY MAP

## 1.1 Pipeline → Service Mapping

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    PRODUCTION PIPELINE & SERVICE DEPENDENCIES            │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  STEP 1: Market Intelligence                                            │
│  ├── AI Provider (Groq/Gemini) ──── Niche analysis, scoring            │
│  ├── Amazon Data ──── BSR, categories, competitor data                  │
│  ├── Google Trends API ──── Keyword trend data                          │
│  └── Publisher Rocket ──── Keyword research data (manual import)        │
│                                                                         │
│  STEP 2: Concept Generation                                             │
│  ├── AI Provider (Groq/Gemini) ──── Generate 10-20 concepts            │
│  └── Supabase ──── Store concepts + scores                              │
│                                                                         │
│  STEP 3: Outline Architecture                                           │
│  ├── AI Provider (Groq/Gemini) ──── Generate detailed outline           │
│  └── Supabase ──── Store outline + metadata                             │
│                                                                         │
│  ★ HUMAN REVIEW GATE 1: Concept + Outline Approval                     │
│                                                                         │
│  STEP 4: Content Writing (HIGHEST COST STEP)                            │
│  ├── AI Provider (Groq → Gemini → OpenAI → Anthropic) ──── Chapters   │
│  ├── AI Provider ──── Continuity summaries between chapters             │
│  └── Supabase ──── Store chapters + progress                            │
│                                                                         │
│  ★ HUMAN REVIEW GATE 2: Draft Quality Review                           │
│                                                                         │
│  STEP 5: Quality Assurance                                              │
│  ├── AI Provider ──── Quality scoring, consistency check                │
│  ├── Copyscape API ──── Plagiarism detection                            │
│  ├── AI Provider ──── Hallucination detection                           │
│  └── AI Provider ──── Legal/compliance review                           │
│                                                                         │
│  STEP 6: Metadata Generation                                            │
│  ├── AI Provider ──── Titles, descriptions, keywords                    │
│  └── Publisher Rocket data ──── Keyword validation                      │
│                                                                         │
│  STEP 7: Cover Generation                                               │
│  ├── DALL-E 3 / Ideogram ──── Generate cover images                     │
│  ├── Image processing lib ──── Text overlay, sizing                     │
│  └── Supabase Storage ──── Store cover files                            │
│                                                                         │
│  STEP 8: EPUB Formatting                                                │
│  ├── Calibre (local) ──── EPUB generation                               │
│  └── Kindle Previewer (local) ──── Validation                           │
│                                                                         │
│  ★ HUMAN REVIEW GATE 3: Pre-publish Checklist                          │
│                                                                         │
│  STEP 9: Publishing                                                     │
│  ├── Amazon KDP ──── Upload, metadata, pricing                          │
│  ├── Kobo Writing Life (Phase 2) ──── Multi-platform                    │
│  └── Draft2Digital (Phase 2) ──── Aggregator distribution               │
│                                                                         │
│  STEP 10: Analytics & Optimization                                      │
│  ├── Amazon KDP Dashboard ──── Sales, royalties, BSR                    │
│  ├── Amazon Ads ──── Campaign performance                               │
│  └── Supabase ──── Aggregate analytics data                             │
│                                                                         │
│  ★ HUMAN REVIEW GATE 4: Performance Review & Optimization               │
│                                                                         │
│  INFRASTRUCTURE (xuyen suot tat ca steps):                              │
│  ├── Supabase ──── PostgreSQL database + storage + (future) auth        │
│  ├── Vercel ──── Hosting, serverless functions, cron                     │
│  ├── GitHub ──── Source code, version control                            │
│  └── Domain/DNS ──── Web presence                                       │
└─────────────────────────────────────────────────────────────────────────┘
```

## 1.2 Service Criticality Matrix

| Service | Pipeline Steps | Criticality | Co Alternative? | Downtime Impact |
|---------|---------------|-------------|-----------------|-----------------|
| **Groq API** | 1,2,3,4,5,6 | CRITICAL | Co (Gemini, OpenAI) | Pipeline dung neu ko failover |
| **Google Gemini** | 1,2,3,4,5,6 | HIGH | Co (Groq, OpenAI) | Backup provider |
| **OpenAI** | 4 (premium), 7 (DALL-E) | MEDIUM | Co (Anthropic, Ideogram) | Giam quality options |
| **Anthropic** | 4 (premium backup) | LOW | Co (OpenAI, Gemini) | Mat 1 provider option |
| **Supabase** | ALL | CRITICAL | PostgreSQL self-host | Toan bo system down |
| **Vercel** | ALL (hosting) | CRITICAL | Netlify, Railway | Website + API down |
| **Amazon KDP** | 9, 10 | CRITICAL | Kobo, D2D (partial) | Khong publish duoc |
| **DALL-E 3** | 7 | HIGH | Ideogram, Midjourney API | Khong tao cover |
| **Copyscape** | 5 | MEDIUM | Manual check, alternatives | Skip plagiarism check |
| **Publisher Rocket** | 1, 6 | LOW | Manual keyword research | Cham hon, it data |
| **Calibre** | 8 | HIGH | epubjs, pandoc | Khong format duoc |
| **GitHub** | Infrastructure | MEDIUM | GitLab, Bitbucket | Code access disrupted |

## 1.3 Cost Distribution per Book (Service Breakdown)

> **Canonical per-book cost breakdown:** [02_ECONOMICS.md](02_ECONOMICS.md) Section 2.1
>
> This document focuses on **service-level** costs (monthly budgets, per-provider pricing). For the consolidated per-book production cost, see Economics.

### Monthly Service Budget Summary

See Part 4.4 below for detailed monthly service budgets by phase.

---

# PART 2: VENDOR PROFILES & ALTERNATIVES

## 2.1 AI Language Model Providers

### A. Groq (Primary - MVP)
| Attribute | Details |
|-----------|---------|
| **Role** | Primary LLM cho tat ca text generation |
| **Models** | Llama 3.1 70B, Mixtral 8x7B |
| **Free Tier** | ~14,400 requests/day (rat generous) |
| **Paid Pricing** | $0.05-0.10/1M tokens (cuc re) |
| **Strengths** | Toc do cuc nhanh (inference), free tier lon, Llama models quality tot |
| **Weaknesses** | Khong co fine-tuning, model selection han che, startup moi |
| **Lock-in Risk** | THAP — dung standard API format, models la open-source |
| **Switching Cost** | RAT THAP — chi thay endpoint URL + API key |

### B. Google Gemini (Secondary)
| Attribute | Details |
|-----------|---------|
| **Role** | Secondary/fallback LLM |
| **Models** | Gemini 1.5 Pro, Gemini 1.5 Flash |
| **Free Tier** | 60 requests/minute (generous) |
| **Paid Pricing** | $0.075-0.30/1M tokens |
| **Strengths** | Google backing, long context (1M tokens), multimodal |
| **Weaknesses** | API thay doi thuong xuyen, rate limits phuc tap |
| **Lock-in Risk** | THAP — standard REST API |
| **Switching Cost** | THAP — adapter pattern trong code |

### C. OpenAI (Premium)
| Attribute | Details |
|-----------|---------|
| **Role** | Premium quality option + DALL-E covers |
| **Models** | GPT-4o-mini, GPT-4o |
| **Free Tier** | Khong co (tra tien tu dau) |
| **Paid Pricing** | $0.15-2.50/1M tokens (GPT-4o-mini re, GPT-4o dat) |
| **Strengths** | Highest quality output, best instruction following |
| **Weaknesses** | Dat nhat, rate limits, dependency lon nhat |
| **Lock-in Risk** | TRUNG BINH — DALL-E khong de thay the |
| **Switching Cost** | TRUNG BINH — can test quality parity khi switch |

### D. Anthropic Claude (Premium Backup)
| Attribute | Details |
|-----------|---------|
| **Role** | Premium backup, best cho long-form content |
| **Models** | Claude Sonnet 4.5, Claude Haiku 4.5 |
| **Free Tier** | Khong co |
| **Paid Pricing** | $0.25-3.00/1M tokens |
| **Strengths** | Excellent long-form, nuanced writing, safety |
| **Weaknesses** | Dat, API access khong phai luc nao cung available |
| **Lock-in Risk** | THAP |
| **Switching Cost** | THAP |

### E. Open-Source Self-Hosted (Contingency)
| Attribute | Details |
|-----------|---------|
| **Role** | Emergency fallback neu tat ca providers tang gia hoac down |
| **Models** | Llama 3.1 (70B/8B), Mistral Large, Qwen |
| **Cost** | $50-200/mo cho GPU rental (RunPod, Vast.ai) |
| **When to use** | Chi khi: (a) costs tang > 3x, (b) all providers down, (c) scale > 100 books/mo |
| **Lock-in Risk** | KHONG — own everything |

### Provider Fallback Chain (Code Reference: `src/lib/ai/provider.ts`)
```
Request → Groq (free tier)
          ↓ fail/rate-limit
          → Gemini (free tier)
            ↓ fail/rate-limit
            → OpenAI GPT-4o-mini (paid, cheapest)
              ↓ fail/rate-limit
              → Anthropic Haiku (paid, backup)
                ↓ fail
                → Queue for retry (30 min delay)
```

## 2.2 AI Image Generation Providers

| Provider | Use Case | Pricing | Quality | Alternative |
|----------|----------|---------|---------|-------------|
| **DALL-E 3** (OpenAI) | Book covers | $0.04-0.08/image | Cao | Ideogram |
| **Ideogram** | Book covers (backup) | $0.02-0.05/image | Cao | DALL-E 3 |
| **Midjourney** | Premium covers (future) | $10-60/mo subscription | Rat cao | Manual design |
| **Canva API** | Text overlay, templates | Free → $12/mo | N/A | ImageMagick (free) |

**Recommendation:** DALL-E 3 primary, Ideogram backup. Midjourney chi khi can premium quality.

## 2.3 Infrastructure Services

| Service | Provider | Free Tier | Paid Tier | Alternative | Switch Difficulty |
|---------|----------|-----------|-----------|-------------|-------------------|
| **Database** | Supabase | 500MB, 50K rows | $25/mo (8GB) | PlanetScale, Neon | MEDIUM (migration) |
| **Hosting** | Vercel | 100GB bandwidth | $20/mo | Netlify, Railway | LOW |
| **Storage** | Supabase Storage | 1GB | Included in Pro | Cloudflare R2, S3 | LOW |
| **CDN** | Vercel Edge | Included | Included | Cloudflare | LOW |
| **Email** | Resend | 3,000/mo | $20/mo | SendGrid, Postmark | LOW |
| **Monitoring** | Vercel Analytics | Basic (free) | $10/mo | Sentry ($26/mo) | LOW |
| **DNS** | Cloudflare | Free | Free | Namecheap | LOW |
| **Code** | GitHub | Free (public/private) | $4/mo (team) | GitLab | MEDIUM |

## 2.4 Quality Assurance Tools

| Tool | Purpose | Pricing | Critical? | Alternative |
|------|---------|---------|-----------|-------------|
| **Copyscape** | Plagiarism detection | $0.03/search | HIGH | Grammarly, Turnitin |
| **Kindle Previewer** | EPUB validation | Free | HIGH | Calibre viewer |
| **Calibre** | EPUB formatting | Free (open source) | HIGH | Pandoc, Sigil |
| **LanguageTool** | Grammar check | Free API (limited) | MEDIUM | Grammarly API ($) |

## 2.5 Distribution & Revenue Platforms

| Platform | Role | Fee | Revenue Share | Lock-in Risk | Alternative |
|----------|------|-----|---------------|-------------|-------------|
| **Amazon KDP** | Primary distribution | Free | 30% (ebook) | CAO — 70%+ of ebook market | Kobo, D2D |
| **Kindle Unlimited** | Page read revenue | Free | Variable (~$0.005/page) | CAO — exclusive option | Wide distribution |
| **Amazon Ads** | Book marketing | CPC ($0.15-0.50) | N/A | MEDIUM | BookBub, social ads |
| **Kobo** | Secondary distribution | Free | 30% | THAP | D2D aggregator |
| **Draft2Digital** | Aggregator | Free | 10% + platform fees | THAP | Smashwords |
| **IngramSpark** | Print distribution | $49 setup | Variable | THAP | KDP Print |

## 2.6 Business & Operations Tools

| Tool | Purpose | Cost | Phase |
|------|---------|------|-------|
| **Publisher Rocket** | Keyword research | $97 one-time | MVP |
| **Wave Accounting** | Bookkeeping | Free | MVP |
| **Google Workspace** | Email, docs, calendar | $6/mo | MVP |
| **Notion/Linear** | Project management | Free | MVP |
| **Stripe** | Payment processing (future) | 2.9% + $0.30 | Scale |

---

# PART 3: VENDOR MANAGEMENT FRAMEWORK

## 3.1 Vendor Selection Scorecard

Su dung scorecard nay khi evaluate vendor moi hoac so sanh alternatives:

| Criteria | Weight | Score (1-5) | Notes |
|----------|--------|-------------|-------|
| **Cost Efficiency** | 25% | | Free tier? Per-unit cost? Volume discounts? |
| **Reliability/Uptime** | 20% | | SLA guarantee? Historical uptime? |
| **Quality of Output** | 20% | | Dap ung quality bar cho production? |
| **Switching Cost** | 15% | | Standard API? Data portable? Migration effort? |
| **Scalability** | 10% | | Rate limits? Pricing at 10x/100x volume? |
| **Support & Documentation** | 5% | | Docs quality? Response time? Community? |
| **Company Stability** | 5% | | Funding? Revenue? Risk of shutdown? |

**Threshold:** Score >= 3.5 weighted average → approve. Score < 3.0 → reject. 3.0-3.5 → conditional (monitor closely).

## 3.2 Vendor Onboarding Checklist

```
Khi them vendor moi:
[ ] Doc Terms of Service (dac biet: data usage, content ownership, rate limits)
[ ] Test API voi production-like workload
[ ] Measure latency, reliability, output quality
[ ] Implement trong code voi adapter pattern (de switch)
[ ] Set up monitoring & alerting
[ ] Document API key management (secrets, rotation)
[ ] Set cost alerts/limits (hard cap per day/month)
[ ] Add to vendor registry (bang theo doi ben duoi)
[ ] Test failover scenario (simulate vendor down)
[ ] Review privacy/compliance implications
```

## 3.3 Vendor Registry Template

Maintain bang nay cho moi vendor dang active:

```
┌─────────────────────────────────────────────────────────────┐
│ VENDOR REGISTRY ENTRY                                        │
├─────────────────────────────────────────────────────────────┤
│ Vendor Name:        _________________                        │
│ Category:           _________________                        │
│ Contract Type:      Free Tier / Pay-as-you-go / Subscription│
│ Monthly Cost:       $____ (actual last month)                │
│ Monthly Budget Cap: $____                                    │
│ API Key Location:   .env.local → ___KEY___                  │
│ Account Owner:      _________________                        │
│ Primary Contact:    _________________                        │
│ SLA/Uptime:         ____%                                    │
│ Last Reviewed:      ____-__-__                               │
│ Next Review Date:   ____-__-__                               │
│ Risk Level:         Low / Medium / High / Critical           │
│ Backup Vendor:      _________________                        │
│ Switch Playbook:    Yes / No (link: ___)                     │
│ Notes:              _________________                        │
└─────────────────────────────────────────────────────────────┘
```

## 3.4 Monitoring & Review Cadence

| Frequency | Action | Responsible |
|-----------|--------|-------------|
| **Real-time** | API error rates, latency → auto-failover | System (automated) |
| **Daily** | Cost tracking per provider → compare vs budget | Operator (dashboard) |
| **Weekly** | Usage trends, cost per book, provider performance | Operator (review) |
| **Monthly** | Full vendor review: cost optimization, new alternatives | Founder/CEO |
| **Quarterly** | Strategic review: contract negotiations, provider landscape | Founder/CEO |

## 3.5 Vendor Exit / Switching Playbook

Khi can switch vendor (gia tang, chat luong giam, vendor shutdown):

```
Phase 1: EVALUATE (1-2 ngay)
├── Xac nhan ly do switch
├── Evaluate alternatives (dung Scorecard 3.1)
├── Test alternative voi sample workload
└── So sanh quality output (A/B test 5-10 books)

Phase 2: PREPARE (1-3 ngay)
├── Implement adapter cho new provider (code change)
├── Update environment variables
├── Test end-to-end pipeline voi new provider
└── Prepare rollback plan

Phase 3: MIGRATE (1 ngay)
├── Switch traffic gradually (10% → 50% → 100%)
├── Monitor error rates & quality
├── Verify cost tracking accurate
└── Update vendor registry

Phase 4: CLEANUP (1 ngay)
├── Deactivate old API keys
├── Update documentation
├── Archive old provider config
└── Post-mortem: what did we learn?
```

**Critical Rule:** Luon giu it nhat **2 providers active** cho AI language models. Khong bao gio depend 100% vao 1 provider.

---

# PART 4: COST OPTIMIZATION STRATEGY

## 4.1 Free Tier Maximization (MVP Phase)

```
SERVICE                  FREE TIER CAPACITY          MONTHLY VALUE SAVED
═══════════════════════════════════════════════════════════════════════
Groq                     14,400 req/day              ~$200-500/mo saved
Google Gemini            60 req/min                  ~$100-300/mo saved
Supabase                 500MB + 50K rows            ~$25/mo saved
Vercel                   100GB bandwidth             ~$20/mo saved
GitHub                   Unlimited private repos     ~$4/mo saved
Cloudflare DNS           Unlimited                   ~$10/mo saved
Wave Accounting          Full features               ~$30/mo saved
Calibre                  Full features               ~$0 (always free)
Kindle Previewer         Full features               ~$0 (always free)
═══════════════════════════════════════════════════════════════════════
TOTAL FREE TIER VALUE:                               ~$389-889/mo
```

**Strategy:** Maximize free tiers cho MVP (Month 1-3). Chi upgrade khi hit limits hoac can features cu the.

## 4.2 Token Optimization (Biggest Cost Lever)

AI API costs chiem 60-70% chi phi per book. Optimization tactics:

| Tactic | Savings | Effort | Priority |
|--------|---------|--------|----------|
| **Prompt compression** — giam system prompt length | 15-25% | Medium | P0 |
| **Tiered model selection** — dung cheap model cho simple tasks, premium cho complex | 30-50% | Medium | P0 |
| **Response caching** — cache niche analysis, common patterns | 10-20% | Low | P1 |
| **Batch processing** — group API calls, reduce overhead | 5-10% | Low | P1 |
| **Output length control** — set max_tokens chinh xac | 10-15% | Low | P0 |
| **Progressive generation** — generate outline truoc, chi write chapters approved | 20-30% | Medium | P0 |

### Model Tiering Strategy
```
TASK                          MODEL TIER          WHY
──────────────────────────────────────────────────────────────
Niche scoring                 FREE (Groq/Gemini)   Simple analysis, template output
Concept generation            FREE (Groq/Gemini)   Creative but structured
Outline creation              FREE (Groq/Gemini)   Structured, template-driven
Chapter writing               FREE → PAID          Quality-critical, fallback to paid if free tier quality insufficient
Continuity summaries          FREE (Groq/Gemini)   Short, structured output
Quality scoring               FREE (Groq/Gemini)   Analytical, scoring
Plagiarism detection          PAID (Copyscape)     No free alternative at quality
Metadata generation           FREE (Groq/Gemini)   Short, structured
Cover generation              PAID (DALL-E 3)      No adequate free option
```

## 4.3 Volume Pricing Roadmap

| Timeline | Action | Expected Savings |
|----------|--------|-----------------|
| **Month 1-3** | Use free tiers exclusively | 100% savings vs paid |
| **Month 4-6** | Apply for startup credits (Groq, Google, OpenAI) | $1K-10K in credits |
| **Month 7-9** | Negotiate volume pricing neu usage > $200/mo/provider | 10-20% discount |
| **Month 10-12** | Annual commitment deals (neu predictable usage) | 20-30% discount |
| **Year 2+** | Multi-year deals, become case study partner | 30-50% discount |

### Startup Credits Programs
| Provider | Program | Typical Value | How to Apply |
|----------|---------|---------------|-------------|
| **OpenAI** | Startup program | $1,000-$10,000 credits | Through accelerator or direct |
| **Google Cloud** | Startup credits | $1,000-$100,000 | Google for Startups program |
| **Anthropic** | Early access programs | Varies | Direct outreach |
| **Vercel** | Startup plan | $300-$600 credits | Vercel for Startups |
| **Supabase** | Startup program | Pro plan free 1 year | Launch Week applications |

## 4.4 Cost Tracking & Budgeting

### Per-Book Cost Budget
```
COST CATEGORY           MVP BUDGET    SCALE BUDGET   INDUSTRIAL
═══════════════════════════════════════════════════════════════
AI Text Generation      $8-15         $5-10          $3-7
AI Image (Covers)       $2-4          $1-3           $0.50-2
Plagiarism Check        $0.50-1       $0.30-0.50     $0.20-0.30
Infrastructure (÷ books) $1-2         $0.50-1        $0.20-0.50
Human QA (÷ books)      $5-8          $3-5           $2-3
Tools (÷ books)          $1-2         $0.50-1        $0.20-0.50
═══════════════════════════════════════════════════════════════
TOTAL PER BOOK          $18-32        $10-20         $6-13
TARGET                  < $25         < $18          < $12
```

### Monthly Service Budget (by phase)
```
SERVICE                 MVP (Mo 1-3)  GROWTH (Mo 4-6) SCALE (Mo 7-12)
═══════════════════════════════════════════════════════════════════════
AI APIs (all providers)  $50-200      $200-500        $500-1,500
Image Generation         $20-50       $50-150         $150-400
Copyscape               $10-30       $30-80          $80-200
Supabase                $0           $0-25           $25-50
Vercel                  $0           $0-20           $20-50
Publisher Rocket        $0 (one-time) $0              $0
Email/Monitoring        $0           $0-20           $20-50
Human QA                $200-400     $400-1,000      $1,000-3,000
Amazon Ads              $150-300     $300-1,000      $1,000-3,000
═══════════════════════════════════════════════════════════════════════
TOTAL MONTHLY           $430-980     $980-2,795      $2,795-8,250
BOOKS PRODUCED          10-15        20-40           50-100+
```

### Cost Alert Rules
```
ALERT LEVEL     TRIGGER                             ACTION
─────────────────────────────────────────────────────────────
INFO            Daily cost > $20                    Log, review at EOD
WARNING         Daily cost > $50                    Notify operator
CRITICAL        Daily cost > $100                   Pause pipeline, investigate
EMERGENCY       Monthly cost > budget × 1.5         Stop all non-essential, escalate to owner

Per-Provider:
WARNING         Provider monthly > $500             Review usage, optimize
CRITICAL        Provider monthly > $1,000           Negotiate or switch
```

---

# PART 5: REDUNDANCY & FAILOVER DESIGN

## 5.1 Service Tier Classification

```
TIER 1 — CRITICAL (Pipeline stops without this)
├── AI Language Model (at least 1 provider)
├── Database (Supabase/PostgreSQL)
├── Hosting (Vercel)
└── Distribution (Amazon KDP)
    → MUST have backup for Tier 1 services
    → Max acceptable downtime: 4 hours

TIER 2 — HIGH (Quality degrades without this)
├── Image Generation (DALL-E/Ideogram)
├── Plagiarism Check (Copyscape)
├── EPUB Formatter (Calibre)
└── Secondary AI providers
    → SHOULD have backup
    → Max acceptable downtime: 24 hours

TIER 3 — MEDIUM (Can work around temporarily)
├── Keyword Research tools
├── Analytics tools
├── Email service
└── Monitoring
    → Nice to have backup
    → Max acceptable downtime: 1 week
```

## 5.2 Failover Architecture

```
┌─────────────────────────────────────────────────────────────┐
│ AI LANGUAGE MODELS — Multi-Provider Failover                 │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Request ──→ Groq (Primary, Free)                           │
│              │ ✓ Success → Return                            │
│              │ ✗ Fail / Rate Limited                         │
│              ↓                                               │
│              Gemini (Secondary, Free)                        │
│              │ ✓ Success → Return                            │
│              │ ✗ Fail / Rate Limited                         │
│              ↓                                               │
│              OpenAI GPT-4o-mini (Tertiary, Paid)            │
│              │ ✓ Success → Return                            │
│              │ ✗ Fail                                        │
│              ↓                                               │
│              Anthropic Haiku (Quaternary, Paid)              │
│              │ ✓ Success → Return                            │
│              │ ✗ Fail                                        │
│              ↓                                               │
│              Queue + Retry (30 min cooldown)                 │
│              + Alert operator                                │
│                                                              │
│  Code: src/lib/ai/provider.ts (330 lines)                   │
│  Pattern: Adapter + Circuit Breaker                          │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ IMAGE GENERATION — Failover                                  │
├─────────────────────────────────────────────────────────────┤
│  DALL-E 3 (Primary) ──→ Ideogram (Backup)                  │
│                          ──→ Stock templates (Emergency)     │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ DATABASE — Backup Strategy                                   │
├─────────────────────────────────────────────────────────────┤
│  Supabase (Primary)                                          │
│  ├── Daily automated backups (Supabase built-in)            │
│  ├── Weekly manual export to local                          │
│  └── Emergency: Restore from backup → local PostgreSQL      │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ DISTRIBUTION — Platform Diversification                      │
├─────────────────────────────────────────────────────────────┤
│  Amazon KDP (70-80% of revenue)                              │
│  ├── Risk: Account suspension, policy change                │
│  ├── Mitigation: Kobo + Draft2Digital from Month 4          │
│  └── Emergency: Wide distribution within 48 hours           │
└─────────────────────────────────────────────────────────────┘
```

## 5.3 Failover Testing Schedule

| Test | Frequency | How | Responsible |
|------|-----------|-----|-------------|
| AI provider failover | Monthly | Disable primary, verify auto-switch | Developer |
| Database restore | Quarterly | Restore backup to test environment | Developer |
| Pipeline with backup providers only | Monthly | Force non-primary providers | Operator |
| Full disaster recovery | Semi-annually | Simulate all Tier 1 services down | Founder + Dev |

---

# PART 6: PARTNERSHIP & NEGOTIATION STRATEGY

## 6.1 Partnership Approach per Vendor Category

### AI Providers — "Volume for Discounts"
```
Timeline:
Month 1-6:    Use free tiers, build usage history, document results
Month 4-6:    Apply for startup credits programs
Month 6-9:    Contact sales with usage data + growth projections
Month 9-12:   Negotiate volume pricing deal
Year 2+:      Become case study / reference customer for mutual marketing
```

**Negotiation leverage:**
- Usage data (X requests/month, growing Y% MoM)
- Multi-provider strategy (we can shift volume between providers)
- Case study value (interesting AI use case for their marketing)
- Early adopter of new models (beta testing value)

### Distribution Platforms — "Diversify for Independence"
```
Phase 0 (MVP):     Amazon KDP exclusive (maximize Kindle Unlimited revenue)
Phase 1 (Month 4): Add Kobo (reduce Amazon dependency to 70%)
Phase 2 (Month 6): Add Draft2Digital (reduce Amazon to 50-60%)
Phase 3 (Year 2):  Direct sales channel (reduce platform dependency further)

Goal: No single platform > 60% of revenue by Year 2
```

### Infrastructure — "Free-to-Pro Transition"
```
Start free → prove ROI → upgrade only when:
1. Free tier limit hit AND
2. Alternative free option exhausted AND
3. ROI of upgrade is positive (specific feature needed or scaling required)
```

## 6.2 Outreach Templates

### Startup Credits Request (AI Providers)
```
Subject: Book Production AI Platform - Startup Credits Request

Hi [Provider] Team,

We're building an AI-powered book production pipeline that uses [Provider]
as a [primary/secondary] AI provider. Currently producing [X] books/month
with [Y] API calls/month on your platform.

We'd like to explore your startup credits program:
- Current usage: [X] calls/month, [Y] tokens/month
- 6-month projected usage: [X] calls/month (growing [Z]% MoM)
- Use case: Automated content production pipeline (niche analysis →
  writing → quality assurance → metadata generation)

We're also happy to share our experience as a case study.

Best regards,
[Name]
```

### Volume Pricing Inquiry
```
Subject: Volume Pricing Discussion - [X] API calls/month

Hi [Provider] Sales Team,

We've been using [Provider] for [X] months with consistent usage of
[Y] calls/month and growing. We'd like to discuss volume pricing options.

Current monthly spend: $[X]
Projected 12-month spend: $[Y]
We're evaluating multi-provider strategy and would like to understand
what volume commitments could reduce our per-unit cost.

Available for a call this week?

Best regards,
[Name]
```

## 6.3 Content Partner Network (Human QA)

| Phase | QA Editor Pool | Source | Pricing |
|-------|---------------|--------|---------|
| **MVP** | 1-2 freelancers | Upwork, Reedsy | $8-15/hour |
| **Growth** | 3-5 managed editors | Referral + Upwork | $10-12/hour (volume rate) |
| **Scale** | 5-10 editors + manager | Dedicated pool | $8-10/hour + bonus |
| **Industrial** | QA team with lead | Full hire | Salary + performance |

**Editor Selection Criteria:**
- Experience with ebook/self-published content
- Familiarity with Amazon KDP requirements
- Fast turnaround (24-48 hours per book)
- Comfortable with AI-generated content
- Native English speaker (for English market)

---

# PART 7: RISK MANAGEMENT

## 7.1 Vendor Risk Assessment Matrix

| Risk | Probability | Impact | Affected Steps | Mitigation |
|------|-----------|--------|----------------|------------|
| **AI provider price increase 2-3x** | Cao | Cao | 1-6 | Multi-provider, switch to cheaper, token optimization |
| **Amazon account suspension** | Trung binh | Rat cao | 9, 10 | Multi-platform, comply strictly, separate pen names |
| **Groq shutdown/pivot** | Thap | Cao | 1-6 | Already have 3 backup providers |
| **Supabase outage** | Thap | Rat cao | ALL | Daily backups, restore playbook |
| **Free tier removal** | Trung binh | Trung binh | 1-6 | Budget for paid, apply for credits |
| **API rate limit reduction** | Trung binh | Trung binh | 1-6 | Multi-provider distributes load |
| **DALL-E policy restricts book covers** | Thap | Trung binh | 7 | Ideogram backup, stock photos |
| **Copyscape accuracy decline** | Thap | Trung binh | 5 | Manual spot-checks, alternative tools |
| **ToS change — AI content restrictions** | Trung binh | Cao | ALL | Monitor policies monthly, human editorial layer |

## 7.2 Concentration Risk Dashboard

```
CURRENT VENDOR CONCENTRATION:
═══════════════════════════════════════════════════

AI Language:    4 providers ████████████████████  HEALTHY
                (Groq, Gemini, OpenAI, Anthropic)

AI Image:       2 providers ██████████            OK
                (DALL-E 3, Ideogram)

Database:       1 provider  █████                 AT RISK ⚠️
                (Supabase only)

Hosting:        1 provider  █████                 AT RISK ⚠️
                (Vercel only)

Distribution:   1 platform  █████                 CRITICAL ⚠️
                (Amazon KDP only at MVP)

QA Tools:       2 tools     ██████████            OK
                (Copyscape + AI-based)

═══════════════════════════════════════════════════
ACTION ITEMS:
[ ] Month 4: Add Kobo to reduce Amazon concentration
[ ] Month 6: Test Netlify/Railway as Vercel backup
[ ] Ongoing: Daily database backups to secondary location
```

## 7.3 Scenario Planning

### Scenario A: AI Provider Costs Increase 3x
```
Impact: Cost per book increases from $25 → $45-60
Response:
1. Immediate: Switch to cheapest available provider
2. Short-term: Aggressive token optimization (reduce 30%)
3. Medium-term: Test self-hosted open-source models
4. Last resort: Reduce output volume, focus on highest-ROI books
Recovery time: 1-2 weeks
```

### Scenario B: Amazon Account Suspended
```
Impact: 70-80% revenue stops immediately
Response:
1. Immediate: File appeal with Amazon
2. Day 1: Upload all books to Kobo + Draft2Digital
3. Day 2: Setup direct sales (Gumroad)
4. Week 1: Evaluate new Amazon account options
5. Ongoing: Continue publishing on alternative platforms
Recovery time: 2-4 weeks to partial revenue, 2-3 months to full
```

### Scenario C: Supabase Major Outage (> 24 hours)
```
Impact: Pipeline completely stopped
Response:
1. Restore from latest backup to local PostgreSQL
2. Update connection strings to local DB
3. Continue production in degraded mode
4. Monitor Supabase status for recovery
Recovery time: 4-8 hours (with backup tested)
```

## 7.4 Vendor ToS Monitoring Checklist

Review monthly:
```
[ ] Amazon KDP Terms of Service — changes to AI content policy?
[ ] OpenAI Usage Policy — changes to content generation restrictions?
[ ] Groq Terms — changes to free tier limits or acceptable use?
[ ] Google AI Terms — changes to Gemini API terms?
[ ] Supabase Terms — changes to data retention, pricing?
[ ] DALL-E Content Policy — new image generation restrictions?

Date of last review: ________
Reviewed by: ________
Changes found: Yes / No
Action required: ________
```

---

# PART 8: CONNECTION TO SYSTEM DESIGN

## 8.1 Architecture Patterns for Multi-Vendor

### Adapter Pattern (Current: `src/lib/ai/provider.ts`)
```
Moi AI provider duoc wrap trong adapter chuan:

interface AIProvider {
  generateText(prompt: string, options: GenerateOptions): Promise<string>
  generateImage(prompt: string, options: ImageOptions): Promise<Buffer>
}

→ Switch provider = swap adapter, khong thay doi business logic
→ Code reference: [03_PIPELINE.md](03_PIPELINE.md) Section 3
```

### Circuit Breaker Pattern
```
Moi external service call duoc bao ve boi circuit breaker:

States: CLOSED (normal) → OPEN (service down, skip) → HALF_OPEN (test recovery)

Config per provider:
  failure_threshold: 3 consecutive failures → OPEN
  recovery_timeout: 30 seconds → try HALF_OPEN
  success_threshold: 2 consecutive successes → CLOSED

→ Code reference: [03_PIPELINE.md](03_PIPELINE.md) Section 6
```

### Configuration Management
```
Vendor config should be externalized:

.env.local:
  GROQ_API_KEY=xxx
  GEMINI_API_KEY=xxx
  OPENAI_API_KEY=xxx
  ANTHROPIC_API_KEY=xxx
  SUPABASE_URL=xxx
  SUPABASE_ANON_KEY=xxx

Runtime config (database or config file):
  provider_priority: [groq, gemini, openai, anthropic]
  provider_enabled: {groq: true, gemini: true, ...}
  cost_limits: {daily: 50, monthly: 500, per_book: 25}
  model_mapping: {
    niche_analysis: "groq/llama-3.1-70b",
    writing: "groq/llama-3.1-70b",
    quality_check: "gemini/gemini-1.5-flash",
    cover: "openai/dall-e-3"
  }
```

## 8.2 Database Schema for Vendor Tracking

```sql
-- Track all external service calls (da co trong agent_runs, mo rong)
-- Reference: supabase/migrations/001_initial_schema.sql

-- Mo rong agent_runs table:
-- provider: string (groq, gemini, openai, anthropic, dalle, copyscape)
-- provider_model: string (llama-3.1-70b, gemini-1.5-flash, etc.)
-- tokens_input: int
-- tokens_output: int
-- cost_usd: decimal
-- latency_ms: int
-- success: boolean
-- error_code: string (nullable)
-- fallback_used: boolean (co switch provider hay khong)

-- Aggregation views cho cost tracking:
-- daily_costs_by_provider: SUM(cost_usd) GROUP BY provider, date
-- monthly_costs_by_provider: SUM(cost_usd) GROUP BY provider, month
-- cost_per_book: SUM(cost_usd) GROUP BY book_id
-- provider_reliability: COUNT(success) / COUNT(*) GROUP BY provider
```

## 8.3 Monitoring & Alerting Integration

```
Dashboard Metrics (connect to [07_OPERATIONS.md](07_OPERATIONS.md)):

PER PROVIDER:
├── Requests/minute (real-time)
├── Error rate % (5-minute window)
├── Average latency (ms)
├── Cost today / this month
├── Remaining free tier quota
└── Circuit breaker status

PER BOOK:
├── Total cost breakdown by service
├── Provider usage distribution
├── Time per pipeline step
└── Retry/failover count

SYSTEM-WIDE:
├── Total daily/monthly service costs
├── Cost per book (rolling average)
├── Provider health status (all green?)
├── Free tier utilization %
└── Days until free tier renewal
```

## 8.4 Feature Flags for Provider Control

```
Cho phep switch provider behavior ma khong can deploy:

feature_flags:
  use_groq: true
  use_gemini: true
  use_openai: false  ← tam tat neu qua dat
  use_anthropic: false
  use_dalle: true
  use_ideogram: false

  prefer_free_tier: true  ← uu tien free providers
  max_paid_fallbacks: 2   ← toi da 2 lan fallback sang paid

  enable_caching: true    ← cache niche analysis results
  cache_ttl_hours: 24     ← cache valid for 24h
```

---

# APPENDIX

## A. Service Ecosystem Evolution Timeline

```
MONTH 1-3 (MVP):
├── AI: Groq (free) + Gemini (free) + OpenAI (minimal paid)
├── Infra: Supabase (free) + Vercel (free) + GitHub (free)
├── Tools: Calibre (free) + Copyscape (pay-per-use)
├── Distribution: Amazon KDP only
├── Human: 1-2 freelance editors
└── Total service cost: $430-980/month

MONTH 4-6 (GROWTH):
├── AI: + Startup credits applied, begin volume tracking
├── Infra: Supabase Pro ($25) if needed
├── Tools: + LanguageTool for grammar
├── Distribution: + Kobo, + Draft2Digital
├── Human: 3-5 editors (managed pool)
└── Total service cost: $980-2,795/month

MONTH 7-12 (SCALE):
├── AI: Volume pricing negotiations, optimize model selection
├── Infra: Vercel Pro ($20), monitoring tools
├── Tools: Full QA suite
├── Distribution: 3+ platforms
├── Human: 5-10 editors with QA lead
└── Total service cost: $2,795-8,250/month

YEAR 2+ (INDUSTRIAL):
├── AI: Volume deals (30-50% discount), possible self-hosted backup
├── Infra: Full production stack
├── Tools: Custom integrations
├── Distribution: All major platforms + direct sales
├── Human: Full QA team
└── Total service cost: $8,000-25,000/month
```

## B. Vendor Comparison Quick Reference

### AI Text Generation (Cost per 1M tokens)
```
PROVIDER        INPUT      OUTPUT     SPEED      FREE TIER
════════════════════════════════════════════════════════════
Groq            $0.05      $0.10      ★★★★★      ★★★★★
Gemini Flash    $0.075     $0.30      ★★★★       ★★★★
GPT-4o-mini     $0.15      $0.60      ★★★        ☆☆☆☆☆
Claude Haiku    $0.25      $1.25      ★★★        ☆☆☆☆☆
GPT-4o          $2.50      $10.00     ★★★        ☆☆☆☆☆
Claude Sonnet   $3.00      $15.00     ★★★        ☆☆☆☆☆
════════════════════════════════════════════════════════════
Note: Prices as of early 2026. Check current pricing before decisions.
```

### AI Image Generation (Cost per image)
```
PROVIDER        COST/IMAGE   QUALITY    SPEED     BOOK COVER SUITABLE
════════════════════════════════════════════════════════════════════
DALL-E 3        $0.04-0.08   ★★★★      ★★★★      ★★★★
Ideogram        $0.02-0.05   ★★★★      ★★★       ★★★★
Midjourney      ~$0.03*      ★★★★★     ★★★       ★★★★★
(* subscription-based, effective per-image cost varies)
```

## C. Connected Documents Reference

| Document | Relevant Sections |
|----------|-------------------|
| [01_VISION.md](01_VISION.md) | Section 1.1 (service dependency definition), Section 5 (architecture), Section 7 (tech stack), Section 10 (roadmap) |
| [03_PIPELINE.md](03_PIPELINE.md) | Section 3 (provider abstraction), Section 5 (security), Section 6 (error handling) |
| [04_AGENTS.md](04_AGENTS.md) | Section 4 (cost governance per agent), provider mapping per agent |
| [02_ECONOMICS.md](02_ECONOMICS.md) | Section 4-5 (cost breakdown), per-book economics |
| [07_OPERATIONS.md](07_OPERATIONS.md) | SOP #3 (pipeline run), monitoring procedures |
| [08_MVP_CHECKLIST.md](08_MVP_CHECKLIST.md) | Day 0 (technical setup — API keys, accounts) |

---

*Document Version: 3.0*
*Previous Version: 2.0 (PARTNERSHIP_SAAS_STRATEGY.md — per-book cost breakdown consolidated to 02_ECONOMICS.md)*
*Focus: Service ecosystem & vendor management for the production factory*
*Next Review: Monthly (vendor costs + performance)*
*Owner: Founder/CEO*
