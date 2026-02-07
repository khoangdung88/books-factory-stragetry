# AI BOOK PRODUCTION ENGINE
## AGENT SPECIFICATION & GOVERNANCE

**Document Version:** 3.0
**Last Updated:** 2026-02-07
**Status:** Active
**Reading Order:** 04 of 08 — See [00_INDEX.md](00_INDEX.md) for full document map

---

# 1. NGUYÊN TẮC THIẾT KẾ AGENT

## 1.1 Core Principles
| # | Principle | Description |
|---|-----------|-------------|
| 1 | Single Responsibility | Mỗi agent chỉ làm 1 việc rõ ràng |
| 2 | Stateless Design | Không giữ state giữa các call (state in DB) |
| 3 | Structured Output | JSON schema cố định, validate trước khi dùng |
| 4 | Fail Gracefully | Retry → Fallback provider → Manual escalation |
| 5 | Human Checkpoint | Điểm dừng bắt buộc tại gates |
| 6 | Cost Tracking | Log tokens, cost, duration cho mỗi run |
| 7 | Version Control | Mỗi prompt có version, tracked in DB |
| 8 | Idempotent | Chạy lại cùng input → cùng quality output |
| 9 | Observable | Mọi agent run đều có log đầy đủ |
| 10 | Budget Bounded | Mỗi agent có hard cost limit |

## 1.2 Agent Lifecycle
```
DESIGN → TEST → DEPLOY → MONITOR → OPTIMIZE → VERSION UP
  │                                      │
  └──────────────────────────────────────┘
```

---

# 2. DANH SÁCH AGENT (Production)

## 2.1 Niche Analyzer Agent
```yaml
name: niche_analyzer
version: 1.0
category: market_intelligence
priority: P0 (MVP)

Purpose: Phân tích và scoring niche từ market data

Input:
  - keywords: string[]
  - category_data: object (BSR, pricing, reviews)
  - competitor_books: Book[] (top 10)

Output:
  - niche_id: uuid
  - niche_name: string
  - scores:
      demand: float (0-10)
      competition: float (0-10)
      profit: float (0-10)
      overall: float (weighted)
  - top_keywords: Keyword[]
  - recommendation: "proceed" | "skip" | "review"
  - reasoning: string

Provider Priority: [groq, gemini, openai]
Cost Budget: $0.50/run
Hard Limit: $1.00/run
Max Retries: 3
Timeout: 60s
Fallback: If all providers fail → queue for manual analysis
```

## 2.2 Concept Generator Agent
```yaml
name: concept_generator
version: 1.0
category: ideation
priority: P0 (MVP)

Purpose: Tạo 10-20 concept sách từ niche data

Input:
  - niche_data: NicheAnalysis
  - audience_profile: string
  - num_concepts: int (default: 10)
  - style_preference: "practical" | "inspirational" | "comprehensive"

Output:
  - concepts: Concept[]
      - title: string
      - subtitle: string
      - hook: string
      - target_reader: string
      - key_promise: string
      - differentiation: string
      - confidence_score: float (0-1)
      - chapter_preview: string[]

Provider Priority: [groq, gemini, openai]
Cost Budget: $0.30/run
Hard Limit: $0.75/run
Max Retries: 3
Timeout: 90s
```

## 2.3 Outline Architect Agent
```yaml
name: outline_architect
version: 1.0
category: planning
priority: P0 (MVP)

Purpose: Tạo outline chi tiết từ concept đã chọn

Input:
  - concept: Concept
  - book_length_target: int (word count)
  - structure_type: "how-to" | "framework" | "narrative" | "workbook"

Output:
  - outline: Outline
      - book_title: string
      - chapters: Chapter[]
          - chapter_num: int
          - title: string
          - key_points: string[]
          - word_target: int
          - includes: { stories, exercises, checklists, examples }
      - total_chapters: int
      - estimated_word_count: int
      - front_matter: FrontMatter
      - back_matter: BackMatter

Provider Priority: [groq, gemini, openai]
Cost Budget: $0.50/run
Hard Limit: $1.00/run
Max Retries: 3
Timeout: 90s
```

## 2.4 Writing Agent
```yaml
name: writing_agent
version: 1.0
category: production
priority: P0 (MVP)

Purpose: Viết nội dung chapter-by-chapter

Input:
  - outline: Outline
  - chapter_num: int
  - style_guide: StyleGuide (tone, reading_level, audience)
  - previous_chapters_summary: string (context memory)
  - key_points: string[]

Output:
  - chapter: ChapterDraft
      - chapter_num: int
      - title: string
      - content: string (markdown)
      - word_count: int
      - key_takeaways: string[]
      - concepts_introduced: string[]

Provider Priority: [gemini, openai, anthropic]
Cost Budget: $1.00/chapter
Hard Limit: $2.00/chapter
Max Retries: 3
Timeout: 180s
Note: Cần quality cao hơn → ưu tiên model mạnh
```

## 2.5 Continuity Agent
```yaml
name: continuity_checker
version: 1.0
category: quality
priority: P1

Purpose: Kiểm tra tính nhất quán giữa các chapters

Checks:
  - Terminology consistency
  - Logical flow between chapters
  - No contradictions in advice/information
  - Proper callbacks to earlier content
  - No repetitive content

Input:
  - full_draft: string (all chapters)
  - outline: Outline

Output:
  - continuity_score: float (0-10)
  - flow_issues: Issue[]
  - terminology_inconsistencies: Issue[]
  - contradictions: Issue[]
  - missing_callbacks: Issue[]
  - summary: string

Provider Priority: [openai, anthropic]
Cost Budget: $1.00/run
Hard Limit: $2.00/run
Max Retries: 2
Timeout: 120s
```

## 2.6 Fact Check Agent
```yaml
name: fact_checker
version: 1.0
category: quality
priority: P1

Purpose: Phát hiện claims cần verify, flagging hallucination

Input:
  - draft: string
  - knowledge_domain: string

Output:
  - claims_found: int
  - verified_claims: int
  - flagged_claims: Claim[]
      - text: string
      - location: string
      - concern: string
      - severity: "low" | "medium" | "high"
      - suggestion: string

Provider Priority: [openai, anthropic]
Cost Budget: $0.80/run
Hard Limit: $1.50/run
Max Retries: 2
Timeout: 120s
```

## 2.7 Style Consistency Agent
```yaml
name: style_checker
version: 1.0
category: quality
priority: P2

Purpose: Đảm bảo tone, reading level, và style nhất quán

Checks:
  - Tone consistency across chapters
  - Reading level alignment with target audience
  - Excessive jargon detection
  - Passive voice overuse
  - Sentence length variation

Input:
  - content: string
  - target_style: StyleGuide

Output:
  - style_score: float (0-10)
  - tone_consistent: boolean
  - reading_level: string (grade level)
  - issues: StyleIssue[]
  - suggestions: string[]

Provider Priority: [groq, gemini]
Cost Budget: $0.30/run
Hard Limit: $0.50/run
Max Retries: 2
Timeout: 60s
```

## 2.8 Cover Generator Agent
```yaml
name: cover_designer
version: 1.0
category: packaging
priority: P1

Purpose: Tạo cover variations cho book

Input:
  - title: string
  - subtitle: string
  - genre: string
  - mood: string
  - color_preference: string (optional)

Output:
  - covers: CoverDesign[]
      - image_url: string
      - style: string
      - color_scheme: string
      - prompt_used: string

Provider: [dall-e-3, ideogram]
Cost Budget: $2.00/run (5 variations)
Hard Limit: $5.00/run
Max Retries: 2
Timeout: 120s
```

## 2.9 Metadata Generator Agent
```yaml
name: metadata_generator
version: 1.0
category: packaging
priority: P0 (MVP)

Purpose: Tạo SEO-optimized metadata cho Amazon

Input:
  - book_summary: string
  - niche: string
  - target_reader: string
  - competitor_titles: string[]

Output:
  - titles: TitleVariant[] (5 options)
  - subtitles: SubtitleVariant[] (5 options)
  - description: string (Amazon HTML format)
  - keywords: string[] (7 for Amazon)
  - categories: string[] (BISAC codes)
  - target_audience: string

Provider Priority: [groq, gemini]
Cost Budget: $0.20/run
Hard Limit: $0.50/run
Max Retries: 3
Timeout: 60s
```

## 2.10 Analytics Agent
```yaml
name: analytics_agent
version: 1.0
category: optimization
priority: P2

Purpose: Phân tích performance và suggest actions

Input:
  - sales_data: SalesRecord[]
  - review_data: Review[]
  - marketing_data: CampaignData[]

Output:
  - performance_score: float (0-10)
  - insights: Insight[]
  - recommended_actions: Action[]
      - type: "rewrite" | "new_cover" | "price_change" | "increase_ads" | "pause_ads" | "create_sequel" | "archive"
      - target_book_id: string
      - reasoning: string
      - expected_impact: string

Provider Priority: [groq, gemini]
Cost Budget: $0.30/run
Hard Limit: $0.50/run
Max Retries: 2
Timeout: 60s
```

## 2.11 Chapter Summary Agent
```yaml
name: chapter_summarizer
version: 1.0
category: utility
priority: P0 (MVP)

Purpose: Tạo summary cho continuity tracking giữa chapters

Input:
  - chapter_content: string
  - chapter_num: int

Output:
  - summary: ChapterSummary
      - main_theme: string
      - key_concepts: string[]
      - key_advice: string[]
      - examples_used: string[]
      - promises_made: string[]

Provider Priority: [groq, gemini]
Cost Budget: $0.05/run
Hard Limit: $0.10/run
Max Retries: 3
Timeout: 30s
```

> Prompt implementations for all agents: [05_PROMPTS.md](05_PROMPTS.md)

---

# 3. AGENT ORCHESTRATION

## 3.1 Pipeline Flow
```
Niche Analyzer ($0.50)
  → Concept Generator ($0.30)
    → [HUMAN GATE 1: Concept Selection]
      → Outline Architect ($0.50)
        → [HUMAN GATE 2: Outline Approval]
          → Writing Agent ($8-10 total, per chapter loop)
            → Chapter Summary Agent ($0.50 total)
              → Continuity Agent ($1.00)
                → Fact Check Agent ($0.80)
                  → Style Consistency Agent ($0.30)
                    → [HUMAN GATE 3: Draft Approval]
                      → Metadata Generator ($0.20)
                        → Cover Designer ($2.00)
                          → [HUMAN GATE 4: Pre-Publish Check]
                            → PUBLISH

Total Agent Cost: ~$14-16 per book
Total with Human Time: ~$22-26 per book
```

## 3.2 Human Gates (Mandatory)

| Gate | What | Who | Criteria | Escalation |
|------|------|-----|----------|------------|
| Gate 1 | Concept + Outline | Content Operator | Score > 7, unique angle | Owner review if borderline |
| Gate 2 | Outline Detail | Content Operator | Logical flow, no red flags | Rerun outline agent |
| Gate 3 | Draft Quality | QA Editor | Quality score > 7, no plagiarism | Rewrite specific chapters |
| Gate 4 | Pre-Publish | Content Operator | All metadata complete, cover approved | Fix issues before publish |

## 3.3 Error Recovery Flow
```
Agent fails
  → Retry (same provider, max 3 times)
    → Fallback (next provider in priority list)
      → Manual queue (human takes over)
        → Alert to operator/owner

Cost exceeded
  → Log warning
    → If > hard limit: Abort + alert
    → If > budget but < hard limit: Continue + flag for review
```

---

# 4. AGENT VERSIONING & GOVERNANCE

## 4.1 Version Control
```
Naming: agent_name@version (e.g., writing_agent@1.2)

Versioning rules:
- Major version (1.0 → 2.0): Output schema change, breaking
- Minor version (1.0 → 1.1): Prompt improvement, non-breaking
- All versions stored in prompt_versions table
- A/B testing between versions allowed
```

## 4.2 Prompt Version Table
```sql
CREATE TABLE prompt_versions (
    id UUID PRIMARY KEY,
    agent_name VARCHAR(50),
    version VARCHAR(10),
    system_prompt TEXT,
    user_prompt_template TEXT,
    output_schema JSONB,
    performance_metrics JSONB,
    is_active BOOLEAN DEFAULT false,
    created_at TIMESTAMP,
    notes TEXT
);
```

## 4.3 Agent Performance Tracking
```sql
-- View: agent performance summary
SELECT
    agent_type,
    agent_version,
    COUNT(*) as total_runs,
    AVG(cost) as avg_cost,
    AVG(duration_ms) as avg_duration,
    SUM(CASE WHEN success THEN 1 ELSE 0 END)::float / COUNT(*) as success_rate,
    AVG(tokens_used) as avg_tokens
FROM agent_runs
WHERE created_at > NOW() - INTERVAL '30 days'
GROUP BY agent_type, agent_version
ORDER BY agent_type, agent_version;
```

## 4.4 SLA Definitions

| Agent | Max Latency | Success Rate | Max Cost/Run | Availability |
|-------|------------|-------------|-------------|-------------|
| Niche Analyzer | 60s | > 95% | $1.00 | Best effort |
| Concept Generator | 90s | > 95% | $0.75 | Best effort |
| Outline Architect | 90s | > 95% | $1.00 | Best effort |
| Writing Agent | 180s/chapter | > 90% | $2.00/ch | Critical |
| Quality Checker | 120s | > 95% | $2.00 | Critical |
| Metadata Generator | 60s | > 98% | $0.50 | Best effort |
| Cover Designer | 120s | > 85% | $5.00 | Best effort |

## 4.5 Cost Governance

### Monthly Agent Cost Budget
| Phase | Monthly Budget | Alert at | Hard Stop |
|-------|---------------|---------|-----------|
| MVP | $200 | $150 | $250 |
| Growth | $500 | $400 | $650 |
| Scale | $2,000 | $1,600 | $2,500 |
| Industrial | $5,000 | $4,000 | $6,500 |

### Cost Optimization Tactics
1. **Prompt compression:** Reduce tokens 20-30% without quality loss
2. **Free tier rotation:** Use Groq/Gemini for non-critical agents
3. **Caching:** Cache identical niche analyses, common metadata patterns
4. **Batch processing:** Group similar requests to amortize overhead
5. **Model selection:** Use smallest adequate model per agent

### Provider Mapping per Agent

Each agent's provider priority is listed in its spec above (Section 2). For detailed vendor profiles, pricing, failover architecture, and cost optimization strategies:

> See: [06_SERVICES.md](06_SERVICES.md) Part 2 (Vendor Profiles) & Part 5 (Failover Design)

---

# 5. AGENT OUTPUT FORMAT (Standard)

## 5.1 Standard Response Envelope
```json
{
  "task_id": "uuid",
  "agent": "agent_name",
  "version": "1.0",
  "input_ref": "reference to input data",
  "output": {},
  "confidence": 0.85,
  "flags": [],
  "cost": {
    "tokens_in": 1500,
    "tokens_out": 800,
    "cost_usd": 0.0034,
    "provider": "groq",
    "model": "llama-3.1-8b"
  },
  "metadata": {
    "duration_ms": 2340,
    "retries": 0,
    "timestamp": "2026-02-07T10:30:00Z"
  }
}
```

## 5.2 Error Response Format
```json
{
  "task_id": "uuid",
  "agent": "agent_name",
  "version": "1.0",
  "error": {
    "code": "PROVIDER_TIMEOUT",
    "message": "Request timed out after 60s",
    "provider": "groq",
    "recoverable": true,
    "retry_count": 2,
    "next_action": "fallback_provider"
  }
}
```

---

# 6. MONITORING & ALERTING

## 6.1 Agent Dashboard Metrics
- **Real-time:** Active runs, queue depth, error rate
- **Hourly:** Cost accumulation, throughput
- **Daily:** Success rates, avg latency, cost per book
- **Weekly:** Provider comparison, version performance, cost trends

## 6.2 Alert Rules
| Condition | Severity | Action |
|-----------|----------|--------|
| Error rate > 10% (1 hour) | Critical | Pause pipeline, notify owner |
| Cost > daily budget | Warning | Notify operator |
| Agent latency > 2x SLA | Warning | Log, consider provider switch |
| Provider down | Critical | Auto-switch to fallback |
| Quality score trending down | Warning | Review prompts, consider version update |
| Success rate < 80% (24h) | Critical | Investigate, pause if needed |

---

*Document Version: 3.0*
*Previous Version: 2.0 (AGENT_SPEC.md — provider mapping consolidated to 06_SERVICES.md)*
*Next Review: After MVP (with production data)*
