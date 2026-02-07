# BOOK FACTORY - DATA PIPELINE ARCHITECTURE
## Complete Technical Design

**Document Version:** 3.0
**Last Updated:** 2026-02-07
**Status:** Active
**Reading Order:** 03 of 08 — See [00_INDEX.md](00_INDEX.md) for full document map

> **This is the SINGLE canonical source for pipeline stages, data model, and QA framework.** All other documents reference here for technical architecture.

---

# 1. PIPELINE OVERVIEW

## 1.1 High-Level Flow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           BOOK FACTORY DATA PIPELINE                         │
└─────────────────────────────────────────────────────────────────────────────┘

    ┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
    │  INPUT   │────▶│ PROCESS  │────▶│  OUTPUT  │────▶│ FEEDBACK │
    │  LAYER   │     │  LAYER   │     │  LAYER   │     │   LOOP   │
    └──────────┘     └──────────┘     └──────────┘     └──────────┘
         │                │                │                │
         ▼                ▼                ▼                ▼
    ┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
    │ • Market │     │ • AI     │     │ • Format │     │ • Sales  │
    │   Data   │     │   Agents │     │ • Package│     │   Data   │
    │ • Niche  │     │ • Quality│     │ • Publish│     │ • Reviews│
    │   Input  │     │   Gates  │     │ • Deliver│     │ • Metrics│
    │ • Cost   │     │ • Human  │     │ • Market │     │ • ROI    │
    │   Budget │     │   Review │     │          │     │   Track  │
    └──────────┘     └──────────┘     └──────────┘     └──────────┘
```

## 1.2 Pipeline Stages

```
  STAGE 1        STAGE 2        STAGE 3        STAGE 4        STAGE 5
  ────────       ────────       ────────       ────────       ────────

  ┌───────┐     ┌───────┐     ┌───────┐     ┌───────┐     ┌───────┐
  │MARKET │     │IDEATE │     │PRODUCE│     │QUALITY│     │PUBLISH│
  │INTEL  │────▶│       │────▶│       │────▶│       │────▶│       │
  └───────┘     └───────┘     └───────┘     └───────┘     └───────┘
      │             │             │             │             │
      ▼             ▼             ▼             ▼             ▼
  ┌───────┐     ┌───────┐     ┌───────┐     ┌───────┐     ┌───────┐
  │Niche  │     │Concepts│    │Chapters│    │Quality │    │Published│
  │Data   │     │Outlines│    │Content │    │Report  │    │Book     │
  │$Cost  │     │        │    │$Cost   │    │        │    │$Total   │
  └───────┘     └───────┘     └───────┘     └───────┘     └───────┘

                                     │
                                     ▼
                              ┌─────────────┐
                              │  STAGE 6    │
                              │  FEEDBACK   │
                              │  LOOP       │
                              └─────────────┘
```

---

# 2. DETAILED PIPELINE STAGES

## 2.1 STAGE 1: Market Intelligence

### Purpose
Thu thập và phân tích dữ liệu thị trường để xác định cơ hội profitable

### Data Flow
```
  External Sources                    Processing                Output
  ─────────────────                  ──────────                ──────

  ┌─────────────┐
  │ Keyword     │──┐
  │ Research    │  │
  └─────────────┘  │     ┌─────────────────┐     ┌─────────────┐
                   │     │                 │     │             │
  ┌─────────────┐  │     │  Niche Analyzer │     │  Niche DB   │
  │ Amazon      │──┼────▶│     Agent       │────▶│  (scored)   │
  │ Category    │  │     │                 │     │             │
  └─────────────┘  │     └─────────────────┘     └─────────────┘
                   │              │
  ┌─────────────┐  │              ▼
  │ Competitor  │──┤     ┌─────────────────┐
  │ Analysis    │  │     │ Scoring Engine  │
  └─────────────┘  │     │ ─────────────── │
                   │     │ • Demand Score  │
  ┌─────────────┐  │     │ • Competition   │
  │ Google      │──┘     │ • Profit Score  │
  │ Trends      │        │ • Risk Score    │
  └─────────────┘        └─────────────────┘
```

### Data Schema
```typescript
interface MarketIntelligence {
  source_data: {
    keywords: string[];
    competitor_books: CompetitorBook[];
    category_data: CategoryData;
    trend_data?: TrendData;
  };
  analysis: {
    niche_opportunities: NicheOpportunity[];
    market_gaps: string[];
    risk_factors: string[];
  };
  recommendations: {
    top_niches: ScoredNiche[];
    avoid_niches: string[];
    action_items: string[];
  };
  cost: {
    agent_cost: number;
    data_source_cost: number;
    total: number;
  };
}

interface ScoredNiche {
  id: string;
  name: string;
  category: string;
  scores: {
    demand: number;        // 0-10
    competition: number;   // 0-10 (higher = less competition = better)
    profit: number;        // 0-10
    feasibility: number;   // 0-10
    overall: number;       // weighted average
  };
  metrics: {
    avg_bsr: number;
    avg_price: number;
    avg_reviews: number;
    estimated_monthly_sales: number;
  };
  recommendation: 'proceed' | 'review' | 'skip';
  reasoning: string;
}
```

---

## 2.2 STAGE 2: Ideation & Planning

### Purpose
Tạo concepts và outlines từ niche data, với human approval gates

### Data Flow
```
   Input                    Agents                      Output
   ─────                    ──────                      ──────

┌──────────┐          ┌─────────────────┐
│  Niche   │          │    Concept      │         ┌─────────────┐
│  Data    │─────────▶│    Generator    │────────▶│  Concepts   │
│          │          │    ($0.30)      │         │  (10-20)    │
└──────────┘          └─────────────────┘         └──────┬──────┘
                                                         │
                                                         ▼
                                                  ┌─────────────┐
                                                  │   HUMAN     │
                                                  │   GATE 1    │
                                                  │   (Select)  │
                                                  └──────┬──────┘
                                                         │
                                                         ▼
┌──────────┐          ┌─────────────────┐         ┌─────────────┐
│ Selected │          │    Outline      │         │   Book      │
│ Concept  │─────────▶│    Architect    │────────▶│   Outline   │
│          │          │    ($0.50)      │         │             │
└──────────┘          └─────────────────┘         └──────┬──────┘
                                                         │
                                                         ▼
                                                  ┌─────────────┐
                                                  │   HUMAN     │
                                                  │   GATE 2    │
                                                  │   (Approve) │
                                                  └─────────────┘
```

### Data Schema
```typescript
interface BookConcept {
  id: string;
  niche_id: string;
  content: {
    title: string;
    subtitle: string;
    hook: string;
    target_reader: string;
    key_promise: string;
    differentiation: string;
  };
  scoring: {
    confidence: number;
    market_fit: number;
    uniqueness: number;
  };
  status: 'generated' | 'selected' | 'rejected';
}

interface BookOutline {
  id: string;
  concept_id: string;
  metadata: {
    title: string;
    subtitle: string;
    total_chapters: number;
    estimated_words: number;
    structure_type: string;
  };
  chapters: ChapterOutline[];
  front_matter: { has_introduction: boolean; intro_purpose: string };
  back_matter: { has_conclusion: boolean; has_resources: boolean; has_about_author: boolean };
  status: 'draft' | 'approved' | 'rejected';
}
```

---

## 2.3 STAGE 3: Content Production

### Purpose
Tạo nội dung sách từ outline đã approved, chapter-by-chapter

### Data Flow
```
   Input              Production Loop                    Output
   ─────              ───────────────                    ──────

┌──────────┐     ┌─────────────────────────────┐     ┌──────────┐
│ Approved │     │                             │     │  Draft   │
│ Outline  │────▶│  For each chapter:          │────▶│  Book    │
└──────────┘     │                             │     └──────────┘
                 │  ┌─────────────────────┐    │
                 │  │ 1. Writing Agent    │    │
                 │  │    ($1.00/chapter)  │    │
                 │  └─────────┬───────────┘    │
                 │            │                │
                 │            ▼                │
                 │  ┌─────────────────────┐    │
                 │  │ 2. Chapter Summary  │    │
                 │  │    (for next ch     │    │
                 │  │     continuity)     │    │
                 │  └─────────┬───────────┘    │
                 │            │                │
                 │            ▼                │
                 │  ┌─────────────────────┐    │
                 │  │ 3. Store in DB      │    │
                 │  │    + Track Cost     │    │
                 │  └─────────────────────┘    │
                 │                             │
                 └─────────────────────────────┘
```

### Data Schema
```typescript
interface ChapterContent {
  id: string;
  book_id: string;
  chapter_num: number;
  content: {
    title: string;
    body: string;        // markdown
    word_count: number;
  };
  metadata: {
    key_takeaways: string[];
    concepts_introduced: string[];
  };
  production: {
    cost: number;
    tokens_used: number;
    provider: string;
    duration_ms: number;
  };
  status: 'generating' | 'drafted' | 'reviewed' | 'approved';
  version: number;
}
```

---

## 2.4 STAGE 4: Quality Assurance

### Purpose
Đảm bảo chất lượng trước khi publish - mandatory human gate

### Data Flow
```
   Input                  QA Pipeline                    Output
   ─────                  ───────────                    ──────

┌──────────┐     ┌──────────────────────────────┐     ┌──────────┐
│  Draft   │     │                              │     │ Approved │
│  Book    │────▶│  1. Plagiarism Check (AI)    │────▶│   Book   │
└──────────┘     │  2. Fact Check (AI)          │     └──────────┘
                 │  3. Consistency Check (AI)    │
                 │  4. Legal/Compliance (AI)     │         │ FAIL
                 │  5. HUMAN REVIEW (Gate 3)     │         │
                 │                              │         ▼
                 └──────────────────────────────┘    ┌──────────┐
                                                     │ Revision │
                                                     │  Queue   │
                                                     │ (max 2x) │
                                                     └──────────┘
```

### Data Schema
```typescript
interface QualityAssurance {
  book_id: string;
  checks: {
    plagiarism: PlagiarismCheck;
    factual: FactualCheck;
    consistency: ConsistencyCheck;
    legal: LegalCheck;
    human_review: HumanReview;
  };
  assessment: {
    overall_score: number;       // 0-10
    publish_ready: boolean;
    required_fixes: Fix[];
    warnings: Warning[];
  };
  decision: 'approved' | 'revision_needed' | 'rejected';
  revision_count: number;  // max 2 before archive
  cost: {
    qa_agent_cost: number;
    human_review_time_minutes: number;
  };
}

interface PlagiarismCheck {
  score: number;              // 0-100 (lower = better)
  risk_level: 'low' | 'medium' | 'high';
  flagged_passages: {
    text: string;
    chapter: number;
    similarity: number;
    action: 'rewrite' | 'review' | 'ok';
  }[];
}

interface FactualCheck {
  claims_found: number;
  concerning_claims: {
    claim: string;
    chapter: number;
    concern: string;
    severity: 'low' | 'medium' | 'high';
    suggestion: string;
  }[];
}

interface ConsistencyCheck {
  score: number;
  issues: {
    type: 'terminology' | 'logic' | 'tone' | 'repetition';
    description: string;
    chapters: number[];
    suggestion: string;
  }[];
}

interface LegalCheck {
  risk_level: 'low' | 'medium' | 'high';
  issues: {
    type: 'medical_advice' | 'legal_advice' | 'financial_advice' | 'copyright' | 'defamation';
    text: string;
    chapter: number;
    recommendation: string;
  }[];
  required_disclaimers: string[];
}
```

---

## 2.5 STAGE 5: Packaging & Publishing

### Purpose
Format, đóng gói, và xuất bản sách lên marketplaces

### Data Flow
```
   Input              Packaging Pipeline              Distribution
   ─────              ──────────────────              ────────────

┌──────────┐     ┌─────────────────────────┐     ┌───────────────┐
│ Approved │     │                         │     │               │
│ Content  │────▶│  1. Metadata Gen ($0.20)│     │  Amazon KDP   │
└──────────┘     │  2. Cover Gen ($2.00)   │     │  (Priority)   │
                 │  3. EPUB Format         │────▶│               │
                 │  4. Final QA            │     │  Kobo (P2)    │
                 │  5. HUMAN GATE 4        │     │               │
                 │                         │     │  Direct (P2)  │
                 └─────────────────────────┘     └───────────────┘
```

### Data Schema
```typescript
interface PackagingPipeline {
  book_id: string;
  metadata: {
    titles: TitleVariant[];
    selected_title: TitleVariant;
    description: BookDescription;
    keywords: string[];  // exactly 7 for Amazon
    categories: string[];  // 2 BISAC categories
    author: { pen_name: string; bio: string };
    ai_disclosure: string;
    disclaimers: string[];
  };
  cover: {
    variants: CoverVariant[];
    selected: CoverVariant;
    dimensions: { width: number; height: number };  // 1600x2560 min
  };
  formats: {
    epub: { url: string; validated: boolean; file_size: number };
    pdf?: { url: string; page_count: number };
  };
  publications: Publication[];
  cost: {
    metadata_agent: number;
    cover_generation: number;
    formatting: number;
    total: number;
  };
}
```

---

## 2.6 STAGE 6: Analytics & Feedback Loop

### Purpose
Thu thập performance data và trigger optimization actions

### Data Flow
```
   Data Sources            Analytics Engine           Actions
   ────────────            ────────────────           ───────

┌─────────────┐      ┌──────────────────────┐
│ Sales Data  │──┐   │                      │     ┌─────────────┐
│ (daily)     │  │   │  1. Aggregation      │     │ Rewrite     │
└─────────────┘  │   │  2. Performance      │────▶│ Trigger     │
                 │   │     Scoring          │     └─────────────┘
┌─────────────┐  │   │  3. Trend Analysis   │
│ Reviews     │──┼──▶│  4. ROI Calculation  │     ┌─────────────┐
│ (weekly)    │  │   │  5. Action           │────▶│ Cover A/B   │
└─────────────┘  │   │     Recommendations  │     │ Test        │
                 │   │                      │     └─────────────┘
┌─────────────┐  │   └──────────────────────┘
│ BSR Ranking │──┤                                ┌─────────────┐
│ (daily)     │  │                           ────▶│ Ad Campaign │
└─────────────┘  │                                │ Adjustment  │
                 │                                └─────────────┘
┌─────────────┐  │
│ Ad Campaign │──┘                                ┌─────────────┐
│ Data (daily)│                              ────▶│ Sequel /    │
└─────────────┘                                   │ Archive     │
                                                  └─────────────┘

   AUTOMATED TRIGGERS:
   ┌────────────────────────────────────────────────┐
   │  IF rating < 3.5 AND reviews > 5 → Rewrite    │
   │  IF ACOS > 70% for 14 days     → Pause Ads   │
   │  IF sales > 5/day for 7 days   → Increase Ads │
   │  IF sales > 100 total          → Plan Sequel  │
   │  IF sales < 5 after 60 days    → Archive      │
   │  IF refund rate > 5%           → Review Book  │
   └────────────────────────────────────────────────┘
```

### Data Schema
```typescript
interface BookMetrics {
  book_id: string;
  period: 'daily' | 'weekly' | 'monthly';
  sales: {
    total_units: number;
    total_revenue: number;
    total_royalty: number;
    ku_page_reads: number;
    ku_revenue: number;
    avg_daily_sales: number;
    trend: 'up' | 'down' | 'stable';
  };
  marketing: {
    ad_spend: number;
    ad_revenue: number;
    acos: number;
    impressions: number;
    clicks: number;
    ctr: number;
  };
  reviews: {
    total_count: number;
    avg_rating: number;
    recent_sentiment: 'positive' | 'mixed' | 'negative';
    common_complaints: string[];
  };
  roi: {
    production_cost: number;
    marketing_cost: number;
    total_investment: number;
    total_revenue: number;
    profit: number;
    roi_percentage: number;
    break_even_reached: boolean;
    days_to_break_even: number | null;
  };
  health: 'thriving' | 'healthy' | 'underperforming' | 'failing';
}
```

---

# 3. DATA MODEL (SQL SCHEMA)

## 3.1 Core Entities

```sql
-- Niche tracking
CREATE TABLE niches (
    id UUID PRIMARY KEY,
    name VARCHAR(255),
    category VARCHAR(100),
    demand_score DECIMAL(3,1),
    competition_score DECIMAL(3,1),
    profit_score DECIMAL(3,1),
    overall_score DECIMAL(3,1),
    status VARCHAR(20) CHECK (status IN ('active', 'saturated', 'archived')),
    created_at TIMESTAMP,
    updated_at TIMESTAMP
);

-- Book projects
CREATE TABLE books (
    id UUID PRIMARY KEY,
    niche_id UUID REFERENCES niches(id),
    title VARCHAR(255),
    subtitle VARCHAR(500),
    pen_name VARCHAR(100),
    status VARCHAR(20) CHECK (status IN ('concept', 'outline', 'drafting', 'editing', 'packaging', 'published', 'archived')),
    word_count INT,
    production_cost DECIMAL(10,2),
    marketing_spend DECIMAL(10,2) DEFAULT 0,
    created_at TIMESTAMP,
    published_at TIMESTAMP
);

-- Chapters
CREATE TABLE chapters (
    id UUID PRIMARY KEY,
    book_id UUID REFERENCES books(id),
    chapter_num INT,
    title VARCHAR(255),
    content TEXT,
    word_count INT,
    status VARCHAR(20) CHECK (status IN ('planned', 'drafted', 'reviewed', 'approved')),
    version INT DEFAULT 1
);

-- Publishing records
CREATE TABLE publications (
    id UUID PRIMARY KEY,
    book_id UUID REFERENCES books(id),
    platform VARCHAR(20) CHECK (platform IN ('amazon_kdp', 'kobo', 'google_play', 'direct', 'other')),
    asin VARCHAR(20),
    publish_date DATE,
    price DECIMAL(5,2),
    status VARCHAR(20) CHECK (status IN ('pending', 'live', 'suspended', 'removed'))
);

-- Sales & Analytics
CREATE TABLE sales (
    id UUID PRIMARY KEY,
    publication_id UUID REFERENCES publications(id),
    date DATE,
    units_sold INT,
    page_reads INT DEFAULT 0,
    revenue DECIMAL(10,2),
    royalty DECIMAL(10,2),
    bsr INT
);

-- Marketing campaigns
CREATE TABLE marketing_campaigns (
    id UUID PRIMARY KEY,
    book_id UUID REFERENCES books(id),
    platform VARCHAR(50),
    campaign_type VARCHAR(50),
    daily_budget DECIMAL(6,2),
    spend DECIMAL(10,2) DEFAULT 0,
    impressions INT DEFAULT 0,
    clicks INT DEFAULT 0,
    orders INT DEFAULT 0,
    acos DECIMAL(5,2),
    status VARCHAR(20),
    started_at TIMESTAMP,
    ended_at TIMESTAMP
);

-- Agent runs (for debugging & cost optimization)
CREATE TABLE agent_runs (
    id UUID PRIMARY KEY,
    book_id UUID REFERENCES books(id),
    agent_type VARCHAR(50),
    agent_version VARCHAR(10),
    input_data JSONB,
    output_data JSONB,
    tokens_used INT,
    cost DECIMAL(6,4),
    duration_ms INT,
    provider VARCHAR(50),
    success BOOLEAN DEFAULT true,
    error_message TEXT,
    created_at TIMESTAMP
);

-- Financial ledger
CREATE TABLE financial_transactions (
    id UUID PRIMARY KEY,
    book_id UUID REFERENCES books(id),
    type VARCHAR(20) CHECK (type IN ('production_cost', 'marketing_spend', 'revenue', 'refund', 'subscription', 'tool_cost')),
    amount DECIMAL(10,2),
    description TEXT,
    date DATE,
    created_at TIMESTAMP
);
```

## 3.2 Key Relationships

```
Niche (1) ──────── (N) Books
Book (1) ──────── (N) Chapters
Book (1) ──────── (N) Publications
Book (1) ──────── (N) Marketing_Campaigns
Publication (1) ─ (N) Sales
Book (1) ──────── (N) Agent_Runs
Book (1) ──────── (N) Financial_Transactions
```

---

# 4. STATE MACHINE

## 4.1 Book Lifecycle States

```
                         ┌─────────┐
                         │  NICHE  │
                         └────┬────┘
                              │ generate_concepts
                              ▼
                         ┌─────────┐
            ┌───────────▶│ CONCEPT │
            │            └────┬────┘
            │                 │ select + outline
            │                 ▼
            │            ┌─────────┐
            │            │ OUTLINE │ ← Human Gate 1+2
            │            └────┬────┘
            │                 │ approve
            │                 ▼
            │            ┌─────────┐
     reject │            │ WRITING │
            │            └────┬────┘
            │                 │ complete
            │                 ▼
            │            ┌─────────┐
            │            │   QA    │ ← Human Gate 3
            └────────────└────┬────┘
                              │ pass
                              ▼
                         ┌─────────┐
                         │PACKAGING│ ← Human Gate 4
                         └────┬────┘
                              │ publish
                              ▼
                         ┌─────────┐
                         │PUBLISHED│
                         └────┬────┘
                              │ performance data
                              ▼
                         ┌──────────┐
                    ┌───▶│MONITORING│◀──┐
                    │    └────┬─────┘   │
                    │         │         │
                    └─────────┘    ┌────┴─────┐
                     optimize      │ ARCHIVED │
                                   └──────────┘
```

## 4.2 State Transitions
```typescript
const transitions = [
  { from: 'niche', to: 'concept', trigger: 'generate', conditions: ['niche.score >= 6'] },
  { from: 'concept', to: 'outline', trigger: 'select', conditions: ['concept.selected', 'gate_1.passed'] },
  { from: 'outline', to: 'writing', trigger: 'approve', conditions: ['outline.approved', 'gate_2.passed'] },
  { from: 'writing', to: 'qa', trigger: 'complete', conditions: ['all_chapters.drafted'] },
  { from: 'qa', to: 'packaging', trigger: 'pass', conditions: ['qa.score >= 7', 'gate_3.passed'] },
  { from: 'qa', to: 'writing', trigger: 'revise', conditions: ['qa.score < 7', 'revision_count < 2'] },
  { from: 'qa', to: 'archived', trigger: 'reject', conditions: ['revision_count >= 2'] },
  { from: 'packaging', to: 'published', trigger: 'publish', conditions: ['gate_4.passed', 'epub.valid'] },
  { from: 'published', to: 'monitoring', trigger: 'live', conditions: ['platform.approved'] },
  { from: 'monitoring', to: 'archived', trigger: 'archive', conditions: ['performance.failing', 'age > 60d'] },
];
```

---

# 5. ERROR HANDLING & RECOVERY

## 5.1 Error Categories

```typescript
enum ErrorCategory {
  // Recoverable (auto-retry)
  RATE_LIMIT = 'rate_limit',           // Retry after delay
  NETWORK_ERROR = 'network_error',      // Retry 3x
  TIMEOUT = 'timeout',                  // Retry with longer timeout

  // Partially Recoverable (retry or fallback)
  AI_GENERATION_FAILED = 'ai_failed',   // Retry → fallback provider
  QUALITY_CHECK_FAILED = 'qa_failed',   // Queue for revision (max 2x)
  OUTPUT_PARSE_FAILED = 'parse_failed', // Retry with stricter prompt

  // Non-Recoverable (human intervention)
  INVALID_INPUT = 'invalid_input',      // Reject, notify operator
  BUDGET_EXCEEDED = 'budget_exceeded',  // Pause pipeline, alert owner
  PLATFORM_REJECTED = 'rejected',       // Manual review required
  PROVIDER_DOWN = 'provider_down',      // Switch provider or wait
}
```

## 5.2 Recovery Flow
```
Error Occurs
  │
  ├── Rate Limit? ──────▶ Exponential backoff (1s, 2s, 4s, 8s)
  │
  ├── Network Error? ───▶ Retry 3x with 5s delay
  │
  ├── AI Failed? ───────▶ Retry 2x → Fallback provider → Manual
  │
  ├── Parse Failed? ────▶ Retry with "output JSON only" instruction
  │
  ├── QA Failed? ───────▶ Queue revision (max 2x) → Archive
  │
  ├── Budget Exceeded? ─▶ STOP pipeline → Alert owner
  │
  └── Platform Rejected? ▶ Flag for manual review → Fix → Resubmit
```

## 5.3 Circuit Breaker Pattern
```typescript
// If an agent fails > 3 times in 10 minutes, open circuit
const circuitBreaker = {
  threshold: 3,           // failures before opening
  timeout_window: 600000, // 10 minutes
  reset_timeout: 300000,  // 5 minutes before trying again
  fallback: 'next_provider_in_priority'
};
```

---

# 6. SECURITY & DATA PROTECTION

## 6.1 API Key Management
```
Rules:
1. NEVER commit API keys to git (.env in .gitignore)
2. Use environment variables for all secrets
3. Rotate API keys quarterly
4. Use separate API keys for dev/production
5. Monitor API key usage for anomalies
6. Maintain vendor registry with all active API keys/accounts
   → See 06_SERVICES.md Section 3.3
```

## 6.2 Data Protection
```
Rules:
1. Database credentials: Supabase service role key only in backend
2. Anon key: For frontend (limited permissions via RLS)
3. No PII storage (pen names are not real names)
4. Financial data: Encrypted at rest (Supabase default)
5. Agent logs: Retain for 90 days, then archive
6. Content: Retained indefinitely (it's the product)
```

## 6.3 Access Control
| Role | Dashboard | Pipeline | Database | API Keys | Financial |
|------|-----------|----------|----------|----------|-----------|
| Owner | Full | Full | Full | Full | Full |
| Developer | Full | Full | Read/Write | Read | Read |
| Content Operator | Full | Execute | Read | None | Summary |
| QA Editor | Read (books) | Review gate | Read (content) | None | None |
| Marketing | Read (analytics) | None | Read (sales) | None | Summary |

## 6.4 Backup Strategy
```
Database (Supabase):
- Automatic daily backups (Supabase managed)
- Manual backup before schema migrations
- Point-in-time recovery available on Pro plan

Content files:
- EPUB files stored in Supabase Storage
- Cover images stored in Supabase Storage
- Redundant copy in local/cloud backup (monthly)

Configuration:
- .env backed up securely (not in git)
- Prompt versions stored in database (version controlled)
```

## 6.5 Service Failover

> Chi tiết failover design, vendor profiles, testing schedule: [06_SERVICES.md](06_SERVICES.md) Part 5

---

# 7. QUALITY ASSURANCE FRAMEWORK

## 7.1 Quality Gates

### Gate 1: Concept Approval (Human)
```
[ ] Unique angle (not duplicate of top 10 competitors)
[ ] Clear target audience defined
[ ] Realistic scope for word count
[ ] No legal/liability red flags
[ ] Niche score >= 7.0
[ ] Estimated ROI > 200% at 50 copies
```

### Gate 2: Outline Approval (Human)
```
[ ] Logical chapter flow
[ ] Each chapter has clear value proposition
[ ] Appropriate depth for target length
[ ] Opening hook is compelling
[ ] No chapters that require expert credentials
```

### Gate 3: Draft Quality (AI + Human)
```
[ ] Plagiarism score < 5%
[ ] No obvious factual errors
[ ] Consistent tone throughout
[ ] No repetitive content between chapters
[ ] Call-to-action present
[ ] Word count within 10% of target
[ ] Reading level appropriate for audience
```

### Gate 4: Pre-Publish (Human)
```
[ ] Title is compelling and keyword-rich
[ ] Description follows AIDA format (Attention, Interest, Desire, Action)
[ ] Cover is professional quality (passes "thumbnail test")
[ ] Formatting is clean (EPUB validated on Kindle Previewer)
[ ] All disclaimers included
[ ] AI disclosure statement included (Amazon requirement)
[ ] Price point validated against competitors
[ ] Categories selected (2 relevant categories)
[ ] 7 keywords selected and validated
```

## 7.2 Amazon KDP Compliance Checklist

```
CRITICAL (Instant Rejection):
[ ] No copyright infringement
[ ] No trademark violations in title/subtitle
[ ] No misleading health/financial/legal claims
[ ] AI content disclosed (Amazon requires since 2023)
[ ] Cover meets dimension requirements (1600x2560 min)
[ ] Content has substantial value (not thin/padded)

IMPORTANT (May cause issues):
[ ] Not duplicate/very similar to existing book in catalog
[ ] Author name is appropriate (no impersonation)
[ ] Categories are accurate (no category stuffing)
[ ] Keywords are relevant (no keyword stuffing)
[ ] Description is not misleading
[ ] No prohibited content
```

## 7.3 Quality Metrics Dashboard

| Metric | Target | Measurement | Alert Threshold |
|--------|--------|-------------|-----------------|
| Plagiarism Score | < 5% | Automated + Copyscape | > 8% |
| Flesch Reading Ease | 60-70 | Automated tool | < 50 or > 80 |
| Chapter Consistency | > 90% | Style checker agent | < 85% |
| Fact Accuracy | 100% critical | Manual + AI review | Any flagged |
| Amazon Acceptance | > 95% | Track submissions | Any rejection |
| Reader Rating | > 4.0 avg | Amazon reviews | < 3.5 |
| Refund Rate | < 3% | KDP dashboard | > 5% |
| Content Takedowns | 0 | Amazon notifications | Any |

---

# 8. MONITORING & OBSERVABILITY

## 8.1 Pipeline Metrics Dashboard

```typescript
interface PipelineMetrics {
  performance: {
    books_in_pipeline: number;
    books_completed_today: number;
    avg_time_per_book: number;
    pipeline_throughput: number;  // books/day
    success_rate: number;         // % completed without error
  };
  cost: {
    total_spend_today: number;
    total_spend_month: number;
    cost_per_book_avg: number;
    cost_per_word: number;
    by_provider: Record<string, number>;
    by_agent: Record<string, number>;
    budget_remaining: number;
  };
  quality: {
    avg_qa_score: number;
    first_pass_rate: number;
    revision_rate: number;
    rejection_rate: number;
    platform_acceptance_rate: number;
  };
  business: {
    total_revenue_month: number;
    total_profit_month: number;
    books_published_month: number;
    avg_roi_per_book: number;
    best_performing_niche: string;
  };
}
```

## 8.2 Alerting Rules
```typescript
const alertRules = [
  { name: 'High Error Rate', condition: 'agent_error_rate > 0.1 over 1 hour', severity: 'critical', action: 'pause_pipeline + notify_owner' },
  { name: 'Cost Spike', condition: 'daily_cost > budget * 1.2', severity: 'warning', action: 'notify_operator' },
  { name: 'Budget Exceeded', condition: 'monthly_cost > monthly_budget', severity: 'critical', action: 'pause_all_non_essential + notify_owner' },
  { name: 'Low Quality Trend', condition: 'avg_qa_score < 6 over 5 books', severity: 'warning', action: 'review_prompts + notify_content_operator' },
  { name: 'Platform Rejection', condition: 'any_book_rejected', severity: 'critical', action: 'pause_publishing + manual_review' },
  { name: 'Revenue Decline', condition: 'weekly_revenue < prev_week * 0.7', severity: 'warning', action: 'investigate + review_ads' },
  { name: 'Refund Spike', condition: 'refund_rate > 5% for any book', severity: 'warning', action: 'review_book_quality + pause_ads' }
];
```

---

# 9. SCALING ARCHITECTURE

## 9.1 Phase 1: MVP (Current)
```
Architecture: Monolithic (Next.js)
- Single Next.js app handles everything
- Supabase free tier, Vercel free tier
- Sequential pipeline execution
- Manual monitoring
Capacity: ~10-15 books/month
```

## 9.2 Phase 2: Growth
```
Architecture: Enhanced Monolith
- Add background job queue (BullMQ / Vercel cron)
- Supabase Pro, Vercel Pro
- Parallel chapter generation (3 chapters at once)
- Automated daily analytics cron job
Capacity: ~30-50 books/month
```

## 9.3 Phase 3: Scale
```
Architecture: Service-Oriented
- Separate worker service for pipeline execution
- Queue-based job processing
- Multiple AI provider load balancing
- Dedicated analytics service
Capacity: ~100+ books/month
```

## 9.4 Phase 4: Industrial (Year 2+)
```
Architecture: Microservices (if needed)
- Market Intelligence, Content Production, QA, Publishing, Analytics services
- Shared message queue (Redis/RabbitMQ)
- ML-based niche scoring model
- Multi-region deployment
Capacity: ~300+ books/month
```

## 9.5 Multi-Language Architecture (Future)
```
- Translation Agent (AI translation from English)
- Native Editor Review (human, per language)
- Language-specific metadata optimization
- Separate KDP accounts per language market
- Localized cover design
Priority: Translate proven winners (100+ sales in English)
```

---

# 10. APPENDIX

## A. Database Schema
See: `supabase/migrations/001_initial_schema.sql`

## B. API Contracts
See: `src/app/api/` routes

## C. Agent Specifications
See: [04_AGENTS.md](04_AGENTS.md)

## D. Financial Model
See: [02_ECONOMICS.md](02_ECONOMICS.md)

## E. Operating Procedures
See: [07_OPERATIONS.md](07_OPERATIONS.md)

---

*Document Version: 3.0*
*Previous Version: 2.0 (DATA_PIPELINE_ARCHITECTURE.md — now includes data model + QA framework)*
*Next Review: After MVP completion*
