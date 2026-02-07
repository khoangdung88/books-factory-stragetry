# MVP CHECKLIST - 28 Days to First Revenue

**Document Version:** 3.0
**Last Updated:** 2026-02-07
**Status:** Active
**Reading Order:** 08 of 08 (final) — See [00_INDEX.md](00_INDEX.md) for full document map

---

## Context Documents

Before starting the 28-day sprint, review:
- **[01_VISION.md](01_VISION.md)** — Business strategy and niche selection framework
- **[02_ECONOMICS.md](02_ECONOMICS.md)** — Financial targets and per-book cost budgets
- **[07_OPERATIONS.md](07_OPERATIONS.md)** — Daily SOPs you'll follow after MVP

During implementation, reference:
- **[03_PIPELINE.md](03_PIPELINE.md)** — Technical architecture and data model
- **[04_AGENTS.md](04_AGENTS.md)** — Agent specs to implement
- **[05_PROMPTS.md](05_PROMPTS.md)** — Production-ready prompts
- **[06_SERVICES.md](06_SERVICES.md)** — API keys, vendor setup, failover design

---

## PRE-LAUNCH PREPARATION (Day 0)

### Legal & Business Setup
- [ ] Decide business entity (LLC recommended)
- [ ] Register LLC (Wyoming/Delaware for US, or local equivalent)
- [ ] Get EIN (Employer Identification Number) if US
- [ ] Open separate business bank account
- [ ] Amazon KDP account created
- [ ] Bank account linked to KDP
- [ ] Tax information submitted (W-9 or W-8BEN)
- [ ] Pen name(s) decided (2-3 for MVP)
- [ ] Domain registered for primary pen name (optional, $12/year)
- [ ] Business insurance research (action for Scale phase)

### Technical Setup
- [ ] GitHub repository initialized
- [ ] Supabase project created + schema deployed
- [ ] Groq API key obtained (free tier)
- [ ] Google Gemini API key obtained (free tier)
- [ ] OpenAI API key obtained (backup, paid)
- [ ] DALL-E API access configured
- [ ] Vercel account + project deployed
- [ ] Environment variables configured (.env.local)
- [ ] Database connection tested

### Tools & Subscriptions
- [ ] Publisher Rocket (keyword research) - $97 one-time
- [ ] Copyscape account (plagiarism) - Pay per check (~$0.03/search)
- [ ] Calibre installed (free) - EPUB formatting
- [ ] Kindle Previewer installed (free) - EPUB validation
- [ ] Amazon Ads account created (free, linked to KDP)
- [ ] Bookkeeping tool setup (Wave free / spreadsheet)

### Financial Setup
- [ ] Startup budget confirmed ($2,500-5,000 recommended)
- [ ] Monthly budget breakdown documented
- [ ] Per-book cost target set ($20 production, $50 marketing)
- [ ] Financial tracking spreadsheet/tool ready
- [ ] Tax calendar set (quarterly payments if applicable)

---

## WEEK 1: BUILD FOUNDATION

### Day 1: Project Setup + Legal
```
Morning:
[ ] Create project structure (if not done)
[ ] Initialize dependencies (npm install)
[ ] Setup environment variables
[ ] Connect to Supabase
[ ] Test database connection

Afternoon:
[ ] Create/verify database schema
[ ] Setup basic logging system
[ ] Setup cost tracking in agent_runs table
[ ] Test basic API route
```

**Deliverable:** Working dev environment + database connection

### Day 2: Niche Analyzer (Part 1)
```
Morning:
[ ] Research niche analysis data sources
[ ] Design Niche Analyzer agent prompt
[ ] Implement agent wrapper with retry logic
[ ] Add cost tracking per agent run

Afternoon:
[ ] Implement niche scoring algorithm
[ ] Test with seed keywords (3 niches from Tier 1 list)
[ ] Store results in database
[ ] Verify cost tracking accuracy
```

**Deliverable:** Can analyze and score a niche

### Day 3: Niche Analyzer (Part 2) + Data
```
Morning:
[ ] Integrate keyword research data (manual input for MVP)
[ ] Refine scoring weights based on test results
[ ] Add competitor analysis to niche scoring
[ ] Create niche ranking view

Afternoon:
[ ] Test with 5 niches total
[ ] Validate scores against manual assessment
[ ] Document niche selection process
[ ] Select 3 best niches for first books
```

**Deliverable:** Working niche scoring system with 3 qualified niches

### Day 4: Concept Generator
```
Morning:
[ ] Design concept generator prompt (system + user)
[ ] Create agent wrapper function
[ ] Implement JSON output parsing + validation
[ ] Add retry logic + provider fallback

Afternoon:
[ ] Test with 3 different niches
[ ] Refine prompt based on output quality
[ ] Store concepts in database
[ ] Select best concepts for each niche
```

**Deliverable:** Generate 10 quality concepts from any niche

### Day 5: Outline Architect
```
Morning:
[ ] Design outline generator prompt
[ ] Implement chapter structure logic
[ ] Add word count estimation per chapter
[ ] Add front matter + back matter specification

Afternoon:
[ ] Test with 5 selected concepts
[ ] Refine for different book lengths (25K, 35K, 50K words)
[ ] Add validation rules (min/max chapters, word count)
[ ] Manually review and approve best outlines
```

**Deliverable:** Generate detailed, publishable outlines

### Day 6-7: Basic UI + Pipeline Foundation
```
Day 6 Morning:
[ ] Setup Next.js dashboard layout
[ ] Create niche list view with scores
[ ] Create concept list view with selection

Day 6 Afternoon:
[ ] Build outline preview component
[ ] Add concept approval workflow (select/reject)
[ ] Connect frontend to backend API routes

Day 7 Morning:
[ ] Create pipeline status view
[ ] Add book creation workflow
[ ] Connect pipeline trigger to agents

Day 7 Afternoon:
[ ] Basic styling (Tailwind)
[ ] Error handling in UI
[ ] Test full flow: Niche → Concept → Outline
```

**Deliverable:** Functional dashboard to view niches, concepts, approve outlines

---

## WEEK 2: PRODUCTION PIPELINE

### Day 8: Writing Agent (Core)
```
Morning:
[ ] Design chapter writing prompt (system + user)
[ ] Implement context management (chapter summaries)
[ ] Create chapter-by-chapter generation loop
[ ] Add chapter summary agent (for continuity between chapters)

Afternoon:
[ ] Test single chapter generation
[ ] Measure token usage + cost
[ ] Optimize prompt length (reduce unnecessary tokens)
[ ] Verify output quality on 3 different chapter types
```

**Deliverable:** Generate one quality chapter reliably

### Day 9: Writing Agent (Full Book)
```
Morning:
[ ] Implement full book generation pipeline
[ ] Add continuity summary between chapters
[ ] Handle API rate limits (delay between calls)
[ ] Add progress tracking (% complete)

Afternoon:
[ ] Test full book generation (8-10 chapters)
[ ] Measure total cost per book
[ ] Store all chapters in database
[ ] Review generated book quality
```

**Deliverable:** Generate complete 8-10 chapter book, cost tracked

### Day 10: Quality Checker (Part 1)
```
Morning:
[ ] Implement quality scoring prompt
[ ] Build content analysis agent
[ ] Create quality scoring logic (0-10 scale)
[ ] Add plagiarism risk assessment

Afternoon:
[ ] Test on generated content
[ ] Define quality thresholds (pass: >= 7, revise: 5-7, reject: < 5)
[ ] Create flag system for issues
[ ] Store quality reports in database
```

**Deliverable:** Basic quality scoring with flags

### Day 11: Quality Checker (Part 2)
```
Morning:
[ ] Add consistency checking between chapters
[ ] Add factual claim detection + flagging
[ ] Implement hallucination warning system
[ ] Add legal/compliance check (medical, financial claims)

Afternoon:
[ ] Test on 2 complete generated books
[ ] Refine detection accuracy
[ ] Create human review report format
[ ] Document quality criteria and thresholds
```

**Deliverable:** Comprehensive quality report per book

### Day 12: Metadata Generator
```
Morning:
[ ] Design metadata prompt (titles, descriptions, keywords)
[ ] Generate title variations (5 per book)
[ ] Generate Amazon-optimized descriptions (AIDA format)
[ ] Generate 7 keywords per book

Afternoon:
[ ] Test SEO effectiveness (compare with competitor metadata)
[ ] Create metadata review UI
[ ] Store metadata options in database
[ ] Select best title/description combinations
```

**Deliverable:** Complete metadata package per book

### Day 13: EPUB Formatter
```
Morning:
[ ] Research EPUB generation (use existing library)
[ ] Implement basic EPUB structure
[ ] Add chapter formatting (headings, paragraphs)
[ ] Add front matter (title page, copyright, disclaimer, TOC)

Afternoon:
[ ] Add back matter (about author, other books)
[ ] Add disclaimer page (standard legal disclaimer)
[ ] Add AI disclosure statement
[ ] Test on Kindle Previewer - fix formatting issues
```

**Deliverable:** Valid, professional EPUB file generation

### Day 14: Cover Generator
```
Morning:
[ ] Design cover generation prompt for DALL-E 3
[ ] Implement cover generation (5 variations per book)
[ ] Create different style templates per genre
[ ] Store cover images in Supabase storage

Afternoon:
[ ] Add text overlay logic (title, subtitle, author name)
[ ] Test with different book types
[ ] Ensure covers meet Amazon requirements (1600x2560px)
[ ] Verify covers pass "thumbnail test" (readable at small size)
```

**Deliverable:** 5 cover variations per book, professional quality

---

## WEEK 3: POLISH & TEST

### Day 15: Pipeline Integration
```
Morning:
[ ] Connect all agents in sequence (end-to-end pipeline)
[ ] Add error handling between pipeline steps
[ ] Implement checkpoint saves (resume from failure point)
[ ] Add cost accumulation tracking per book

Afternoon:
[ ] Test end-to-end flow (niche → concept → outline → draft → quality → package)
[ ] Fix integration bugs
[ ] Add progress tracking UI
[ ] Measure total pipeline time and cost
```

**Deliverable:** Complete automated pipeline, tested end-to-end

### Day 16: Dashboard + Financial Tracking
```
Morning:
[ ] Add pipeline status view (real-time progress)
[ ] Add book preview (read generated content)
[ ] Add quality report view
[ ] Add per-book cost tracking view

Afternoon:
[ ] Add cover selection UI
[ ] Add metadata editing UI
[ ] Add export/download functionality
[ ] Add simple financial dashboard (costs, running total)
```

**Deliverable:** Production-ready dashboard with financial tracking

### Day 17: Human Review Flow
```
Morning:
[ ] Implement review checkpoint UI (Gate 1: Concept, Gate 2: Outline)
[ ] Add approve/reject buttons with feedback input
[ ] Implement Gate 3: Draft quality review

Afternoon:
[ ] Implement Gate 4: Pre-publish checklist
[ ] Add revision request flow (feedback → re-generate)
[ ] Test full review cycle
[ ] Document review criteria for QA editor
```

**Deliverable:** Human-in-the-loop workflow at all 4 gates

### Day 18: First Real Book (Production Run)
```
Morning:
[ ] Select best niche from analysis
[ ] Generate 10 concepts
[ ] Choose best concept (use scoring + manual judgment)
[ ] Generate outline, review and approve

Afternoon:
[ ] Start draft generation
[ ] Monitor progress
[ ] Review first 3 chapters for quality
[ ] Make prompt adjustments if needed
```

**Deliverable:** First real book in pipeline (draft in progress)

### Day 19: First Book Complete
```
Morning:
[ ] Review completed draft
[ ] Run all quality checks
[ ] Make manual improvements (fix flagged issues)
[ ] Approve draft through Gate 3

Afternoon:
[ ] Generate metadata (select title, description, keywords)
[ ] Generate covers (select best)
[ ] Format EPUB, validate in Kindle Previewer
[ ] Run through pre-publish checklist (Gate 4)
```

**Deliverable:** First book ready for publishing

### Day 20: Books 2-3
```
All Day:
[ ] Repeat pipeline for 2 more books (different niches)
[ ] Note all friction points
[ ] Document improvements needed
[ ] Track time and cost per book
```

**Deliverable:** 3 books ready for publishing, friction points documented

### Day 21: Bug Fixes + Optimization
```
Morning:
[ ] Fix bugs found during Books 1-3 production
[ ] Optimize slow pipeline steps
[ ] Improve prompts based on quality review feedback
[ ] Reduce any cost overruns

Afternoon:
[ ] Document all processes (update SOPs)
[ ] Create QA editor checklist
[ ] Prepare for production run (Week 4)
[ ] Setup Amazon Ads account (if not done)
```

**Deliverable:** Stable, documented system ready for production

---

## WEEK 4: LAUNCH

### Day 22: Production Batch (Books 4-6)
```
All Day:
[ ] Generate 3 more books using optimized pipeline
[ ] Full quality review on each
[ ] Apply learnings from Books 1-3
[ ] Track costs per book (target: < $25 each)
```

### Day 23: Production Batch (Books 7-10)
```
All Day:
[ ] Generate 4 more books
[ ] Full quality review
[ ] Cover + metadata finalized for all
[ ] All EPUBs validated
```

### Day 24: Pre-Publish Review (ALL 10 Books)
```
Morning:
[ ] Final review all 10 books (spot-check chapters)
[ ] Verify Amazon compliance for all
[ ] Verify all metadata complete and compelling
[ ] Verify all disclaimers + AI disclosure in place

Afternoon:
[ ] Final cover selection for each book
[ ] Final title/subtitle selection
[ ] Export all EPUBs (final versions)
[ ] Complete pre-publish checklist for each book
```

### Day 25: Publish Day
```
Morning:
[ ] Upload books 1-5 to KDP
[ ] Fill all metadata carefully (copy from prepared data)
[ ] Set pricing ($4.99 for standard, $0.99 for 1-2 lead books)
[ ] Enable KDP Select (recommended for first 90 days)

Afternoon:
[ ] Upload books 6-10
[ ] Double-check all listings (metadata, cover, price, categories)
[ ] Submit all for review
[ ] Record all ASINs in database
```

### Day 26: Marketing Setup
```
Morning:
[ ] Setup Amazon Ads automatic campaign for each book ($5/day)
[ ] Configure daily budget limits
[ ] Setup sales tracking spreadsheet
[ ] Setup BSR tracking (manual daily check or tool)

Afternoon:
[ ] Create daily monitoring routine
[ ] Document marketing procedures
[ ] Plan optimization actions for different scenarios
[ ] Setup cost tracking for ad campaigns
```

### Day 27: First Results + Monitoring
```
All Day:
[ ] Check approval status for all books
[ ] Monitor any publishing issues
[ ] Check for first sales
[ ] Review any early ad performance
[ ] Note any issues or rejections
[ ] Respond to any Amazon notifications
```

### Day 28: Review & Plan Phase 2
```
Morning:
[ ] Compile all metrics:
    - Total production cost
    - Cost per book (actual)
    - Time per book (actual)
    - Quality scores (average)
    - Amazon acceptance rate
[ ] Calculate actual unit economics

Afternoon:
[ ] Document all learnings (what worked, what didn't)
[ ] Identify top-performing niches (even with limited data)
[ ] Plan next 10 books (niches, concepts)
[ ] Prioritize improvements for next sprint
[ ] Update financial projections with real data
[ ] Set goals for Month 2
```

---

## SUCCESS CRITERIA

### MVP Complete When:
```
MUST ACHIEVE:
[ ] 10 books published on Amazon KDP
[ ] All books approved (zero rejections/takedowns)
[ ] Average production cost < $25/book
[ ] Pipeline time < 48 hours per book (after first 3)
[ ] At least 1 organic sale

GOOD TO HAVE:
[ ] 10+ total sales across portfolio
[ ] Average quality score > 7.0
[ ] At least 1 review received
[ ] Amazon Ads ACOS < 70% on test campaigns
[ ] Total cost within startup budget

GREAT:
[ ] 50+ total sales
[ ] Positive review received
[ ] Total revenue > total production cost
[ ] Second batch of 10 planned with learnings
[ ] Clear winner niche identified
```

---

## POST-MVP PRIORITIES (Month 2)

### Week 5-6: Optimize
- [ ] Analyze Book 1-10 performance data
- [ ] Optimize prompts based on quality feedback
- [ ] Reduce production cost to < $18/book
- [ ] Optimize Amazon Ads (manual campaigns for winners)
- [ ] A/B test covers on top 3 books

### Week 7-8: Scale
- [ ] Publish 10 more books (focusing on winning niches)
- [ ] Start first series (3-book series in best niche)
- [ ] Implement feedback loop (analytics → optimization)
- [ ] Test paperback for top performer
- [ ] Build email list with lead magnet in books

---

## DAILY STANDUP TEMPLATE

```markdown
## Date: ___________

### Yesterday
- Completed:
- Blocked by:

### Today
- Will complete:
- Need help with:

### Metrics
- Books in pipeline: X
- Books published: X / 10
- Cost spent (total): $X / $X budget
- Quality issues found: X
- Pipeline runs today: X
```

---

## WEEKLY REVIEW TEMPLATE

```markdown
## Week: ___________

### Progress
- Books completed: X
- Books published: X
- Pipeline success rate: X%
- On track for Day 28 goal: Yes/No

### Costs
- LLM API: $___
- Image generation: $___
- Tools: $___
- Human editing: $___
- Total this week: $___
- Running total: $___ / $___ budget

### Quality
- QA pass rate: ___% (first pass)
- Average quality score: ___
- Issues found: [list]
- Improvements made: [list]

### Top Learnings
1.
2.
3.

### Next Week Focus
1.
2.
3.
```

---

*Document Version: 3.0*
*Previous Version: 2.0 (MVP_CHECKLIST.md — added reading order and cross-references)*
*Next Review: Day 28 (with actual results)*
