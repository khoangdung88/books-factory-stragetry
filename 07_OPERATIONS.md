# AI BOOK PRODUCTION ENGINE
## OPERATING MANUAL & ORGANIZATIONAL GOVERNANCE

**Document Version:** 3.0
**Last Updated:** 2026-02-07
**Status:** Active
**Reading Order:** 07 of 08 — See [00_INDEX.md](00_INDEX.md) for full document map

---

# 1. MỤC ĐÍCH

Tài liệu mô tả quy trình vận hành, quản trị tổ chức, và SOP (Standard Operating Procedures) cho hệ thống AI Book Production Engine.

**Mục tiêu:**
- Xuất bản sách nhanh, có kiểm soát chất lượng
- Giảm phụ thuộc cá nhân (bất kỳ ai cũng có thể operate)
- Tối ưu vòng lặp dữ liệu → cải tiến liên tục
- Minh bạch tài chính → kiểm soát chi phí
- Compliance → không bị ban/takedown

---

## Cross-References

| Topic | Reference Document |
|-------|-------------------|
| Pipeline stages & technical flow | [03_PIPELINE.md](03_PIPELINE.md) |
| Agent specifications & SLAs | [04_AGENTS.md](04_AGENTS.md) |
| Quality gates (detailed checklists) | [03_PIPELINE.md](03_PIPELINE.md) Section 7 |
| Financial targets & per-book costs | [02_ECONOMICS.md](02_ECONOMICS.md) |
| 28-day MVP execution plan | [08_MVP_CHECKLIST.md](08_MVP_CHECKLIST.md) |
| Vendor/service incident response | [06_SERVICES.md](06_SERVICES.md) Part 7 |

---

# 2. TỔ CHỨC & VAI TRÒ

## 2.1 Organization Chart

### MVP Phase (1-2 người)
```
Owner/Founder
├── Builder (same person): Development + Operations
└── QA Editor (part-time): Content review
```

### Growth Phase (3-5 người)
```
Owner/CEO
├── Content Operator: Pipeline management, QA oversight
├── Developer: Platform maintenance, feature development
├── Marketing Ops: Amazon Ads, SEO, analytics
└── QA Editor (PT): Content review, quality gate
```

### Scale Phase (8-15 người)
```
CEO
├── CTO / Lead Developer
│   ├── Backend Developer
│   └── Data Engineer
├── Head of Content
│   ├── Content Operator 1
│   ├── Content Operator 2
│   └── QA Editor(s)
├── Marketing Manager
│   ├── Ads Specialist
│   └── Analytics Analyst
└── Operations / Finance
```

## 2.2 RACI Matrix

| Activity | Owner | Content Op | Developer | QA Editor | Marketing |
|----------|-------|-----------|-----------|-----------|-----------|
| Niche selection | A | R | I | - | C |
| Concept approval | A | R | - | C | - |
| Outline approval | I | R | - | C | - |
| Pipeline execution | I | R | C | - | - |
| Quality review | I | A | - | R | - |
| Cover approval | A | R | - | C | C |
| Metadata approval | I | R | - | - | A |
| Publishing | I | R | - | - | C |
| Ad campaigns | A | I | - | - | R |
| Financial review | R | I | I | - | I |
| Incident response | A | R | R | C | C |

**R** = Responsible (does the work)
**A** = Accountable (final decision)
**C** = Consulted (provides input)
**I** = Informed (kept in the loop)

## 2.3 Role Descriptions

### Content Operator
**Responsibilities:**
- Run daily niche intake + scoring
- Manage concept generation and selection pipeline
- Oversee book production from concept to publish
- First-line quality review
- Manage 10-20 books in active pipeline

**Skills Required:**
- Understanding of KDP publishing
- Basic prompt engineering
- Attention to detail
- Decision-making ability (niche selection)
- English proficiency (reading level assessment)

### QA Editor
**Responsibilities:**
- Review AI-generated content for quality
- Check for factual accuracy, tone consistency
- Verify plagiarism reports
- Approve or reject drafts with specific feedback
- Maintain editorial style guides

**Skills Required:**
- Strong English writing/editing skills
- Familiarity with non-fiction publishing standards
- Attention to detail
- Ability to distinguish AI-generated filler from genuine value

---

# 3. LUỒNG VẬN HÀNH TỔNG THỂ

```
1. Market Intake (Daily)
   → 2. Niche Selection (Daily)
      → 3. Concept Generation (Per niche)
         → [GATE 1: Concept Approval]
            → 4. Outline Creation (Per concept)
               → [GATE 2: Outline Approval]
                  → 5. Draft Production (Per chapter)
                     → 6. Quality Review (Per book)
                        → [GATE 3: Draft Approval]
                           → 7. Packaging (Cover + metadata + format)
                              → [GATE 4: Pre-Publish Check]
                                 → 8. Publish
                                    → 9. Analytics & Monitoring (Ongoing)
                                       → 10. Optimization / Rewrite (Triggered)
```

---

# 4. STANDARD OPERATING PROCEDURES (SOPs)

## SOP-001: Daily Market Intake

**Frequency:** Daily (Morning)
**Owner:** Content Operator
**Duration:** 30-60 minutes

### Steps:
1. Check trending keywords in target niches (Publisher Rocket / manual)
2. Review Amazon movers & shakers in target categories
3. Check Google Trends for emerging topics
4. Run Niche Analyzer agent on 2-3 new keyword sets
5. Review niche scores → add qualifying niches (score >= 7) to pipeline
6. Log results in daily standup template

### Output:
- 0-3 new qualified niches added to database
- Updated niche pipeline in dashboard

---

## SOP-002: Concept Generation & Selection

**Frequency:** Per qualified niche
**Owner:** Content Operator
**Duration:** 15-30 minutes per niche

### Steps:
1. Select top-scored niche from queue
2. Run Concept Generator agent (10 concepts)
3. Review concepts against criteria:
   - Unique angle? (not copy of top competitors)
   - Clear target reader?
   - Realistic scope?
   - Legal/liability risk?
4. Select top 1-3 concepts
5. Mark selected concepts in database
6. Move to Outline generation

### Decision Criteria:
| Score | Action |
|-------|--------|
| Confidence > 0.8 + unique angle | Select immediately |
| Confidence 0.6-0.8 | Review competitor titles, decide |
| Confidence < 0.6 | Skip, generate new batch |

---

## SOP-003: Outline Review & Approval

**Frequency:** Per selected concept
**Owner:** Content Operator + QA Editor (consulted)
**Duration:** 15-20 minutes

### Checklist:
- [ ] Logical chapter flow (each builds on previous)
- [ ] Each chapter has clear value proposition
- [ ] No chapter requires expert credentials to write
- [ ] Word count targets are realistic per chapter
- [ ] Opening hook is compelling
- [ ] Conclusion provides actionable takeaways
- [ ] Total word count matches niche expectations (25K-50K typical)

### If Rejected:
1. Note specific issues
2. Re-run Outline Architect with feedback
3. If 2nd attempt fails → skip concept, move to next

---

## SOP-004: Draft Production

**Frequency:** Per approved outline
**Owner:** Content Operator (monitoring)
**Duration:** 2-4 hours (automated, requires monitoring)

### Steps:
1. Initiate pipeline run in dashboard
2. Monitor chapter generation progress
3. After completion, run Continuity Agent
4. Run Fact Check Agent
5. Review agent reports
6. Queue for human QA review

### Monitoring Checkpoints:
- After chapter 1: Verify tone and style match expectations
- After chapter 5 (midpoint): Check for repetition
- After final chapter: Full continuity review

### If Pipeline Fails:
1. Check error logs
2. Identify failed agent + error type
3. If recoverable: Retry from failed chapter
4. If not recoverable: Log issue, escalate to developer

---

## SOP-005: Quality Review (Human)

**Frequency:** Per completed draft
**Owner:** QA Editor
**Duration:** 45-90 minutes

### Review Checklist:
- [ ] **Content Quality:** Is this genuinely useful to the reader?
- [ ] **Accuracy:** No obvious factual errors or misleading claims
- [ ] **Originality:** Plagiarism score < 5%
- [ ] **Consistency:** Tone, terminology, advice consistent throughout
- [ ] **No Filler:** Every chapter adds real value
- [ ] **Disclaimers:** Required disclaimers included
- [ ] **Flow:** Introduction → chapters → conclusion reads naturally
- [ ] **Actionability:** Reader can actually DO something after reading

### Scoring:
| Score | Action |
|-------|--------|
| 8-10 | Approve, move to packaging |
| 6-7 | Approve with minor edits (list specific fixes) |
| 4-5 | Return for revision (list specific chapters to rewrite) |
| < 4 | Reject, archive this book |

### If Revisions Needed:
1. Document specific issues per chapter
2. Run Writing Agent on specific chapters with feedback
3. Re-review only revised chapters
4. Maximum 2 revision cycles, then archive if still failing

---

## SOP-006: Packaging & Pre-Publish

**Frequency:** Per approved draft
**Owner:** Content Operator
**Duration:** 30-60 minutes

### Steps:
1. Run Metadata Generator agent
2. Review and select title + subtitle combination
3. Edit description for maximum appeal (AIDA format)
4. Verify 7 keywords are relevant and non-competing
5. Select Amazon categories (2 categories)
6. Run Cover Designer agent (5 variations)
7. Select best cover (passes "thumbnail test" - readable at small size)
8. Add text overlay to cover (title, subtitle, author name)
9. Generate EPUB file
10. Validate EPUB in Kindle Previewer
11. Final checklist before publishing

### Pre-Publish Final Checklist:
- [ ] Title + subtitle finalized
- [ ] Description complete and compelling
- [ ] 7 keywords set
- [ ] 2 categories selected
- [ ] Cover passes thumbnail test
- [ ] EPUB validated (no formatting errors)
- [ ] Disclaimers in place
- [ ] AI disclosure included
- [ ] Author/pen name correct
- [ ] Price set ($4.99 standard or promotional)
- [ ] KDP Select enrollment decision made

---

## SOP-007: Publishing

**Frequency:** Per packaged book
**Owner:** Content Operator
**Duration:** 15-20 minutes per book

### Steps:
1. Log into KDP dashboard
2. Create new title
3. Upload manuscript (EPUB)
4. Upload cover (JPG/TIFF)
5. Fill all metadata fields
6. Set pricing (ebook + optional paperback)
7. Select territories (worldwide)
8. Enable KDP Select if chosen
9. Submit for review
10. Log publish date + ASIN in database

### Post-Publish:
- Record ASIN and publish date in system
- Set up BSR tracking for new book
- Prepare Amazon Ads campaign (paused until approved)
- Monitor for approval (typically 24-72 hours)

---

## SOP-008: Performance Monitoring

**Frequency:** Daily (5 min), Weekly (30 min), Monthly (2 hours)
**Owner:** Marketing Ops (or Content Operator in MVP)

### Daily (5 minutes):
- Check KDP dashboard for sales
- Check for any notifications/warnings
- Monitor active ad campaigns (ACOS check)
- Check for new reviews

### Weekly (30 minutes):
- Run Analytics Agent on portfolio
- Review per-book performance
- Adjust ad bids (increase on winners, pause losers)
- Identify books needing optimization (cover change, description edit)
- Update financial tracker

### Monthly (2 hours):
- Full P&L review
- Portfolio performance analysis
- Niche performance ranking
- Cost analysis (production + marketing per book)
- Plan next month's publishing schedule
- Review and update niche strategy
- Optimize underperforming books or archive

---

## SOP-009: Incident Response

**Frequency:** As needed
**Owner:** Responsible party per incident type

### Incident Types & Response:

| Incident | Severity | First Response | Escalation |
|----------|----------|---------------|------------|
| Book takedown by Amazon | Critical | Pause all publishing, review book | Owner within 1 hour |
| Account warning | Critical | Pause publishing, review policies | Owner within 1 hour |
| Agent pipeline failure | Medium | Check logs, retry | Developer within 4 hours |
| Cost overrun (> 120% budget) | Medium | Pause non-essential pipelines | Owner within 24 hours |
| Negative review (1-2 stars) | Low | Analyze feedback, consider revision | Content Op within 48 hours |
| Refund spike (> 5% on any book) | Medium | Pause ads, review book quality | Content Op within 24 hours |
| API provider down | Medium | Auto-failover to backup | Developer if failover fails |
| Data breach/security | Critical | Rotate API keys, assess damage | Owner immediately |

### Incident Log Template:
```
Date: ___
Severity: Critical / Medium / Low
Type: ___
Description: ___
Impact: ___
Root Cause: ___
Action Taken: ___
Resolution: ___
Lessons Learned: ___
Preventive Measures: ___
```

---

# 5. LỊCH VẬN HÀNH

## 5.1 Daily Schedule (Content Operator)

| Time | Activity | Duration |
|------|----------|----------|
| 09:00 | Market intake + niche scoring | 30 min |
| 09:30 | Review overnight pipeline results | 15 min |
| 09:45 | Quality gate reviews | 30-60 min |
| 10:45 | Concept generation + selection | 30 min |
| 11:15 | Pipeline management (start new books) | 30 min |
| 14:00 | Packaging + publish ready books | 60 min |
| 15:00 | Monitor ads + sales check | 15 min |
| 15:15 | Admin / documentation | 15 min |

**Total active time: ~4-5 hours/day (MVP)**

## 5.2 Weekly Schedule

| Day | Focus |
|-----|-------|
| Monday | New niche research, start week's production batch |
| Tuesday | Pipeline management, quality reviews |
| Wednesday | Pipeline management, packaging |
| Thursday | Publishing, ad campaign management |
| Friday | Analytics review, weekly metrics, planning |
| Saturday | Optional: performance optimization |
| Sunday | Off (automated monitoring only) |

## 5.3 Monthly Schedule

| Week | Focus |
|------|-------|
| Week 1 | Production push (new books) |
| Week 2 | Production + initial publishing |
| Week 3 | Optimize existing portfolio + continue production |
| Week 4 | Monthly review, financial reconciliation, next month planning |

## 5.4 Quarterly Schedule
- Full business review (P&L, KPIs, strategy)
- Niche strategy update
- Technology review (new AI models, tools)
- Compliance review (Amazon policy updates, AI regulations)
- Team performance review (if applicable)
- Goal setting for next quarter

---

# 6. KPI VẬN HÀNH

## 6.1 Operator Performance KPIs

| KPI | Target | Measurement |
|-----|--------|-------------|
| Books published per week | 2-3 (MVP), 5-7 (Scale) | Count |
| Quality gate pass rate | > 85% first pass | Approved / Total reviewed |
| Pipeline utilization | > 80% | Active pipelines / Total capacity |
| Time per book (operator time) | < 3 hours | Total operator hours / books |
| Incident response time | < 4 hours (Medium), < 1 hour (Critical) | Logged |

## 6.2 System Performance KPIs

| KPI | Target | Alert Threshold |
|-----|--------|-----------------|
| Pipeline success rate | > 90% | < 80% |
| Average book production time | < 48 hours | > 72 hours |
| Agent error rate | < 5% | > 10% |
| Cost per book (agent) | < $16 | > $25 |
| Uptime | > 99% | < 95% |

---

# 7. COMMUNICATION & REPORTING

## 7.1 Daily Standup Template
```markdown
## Date: YYYY-MM-DD

### Yesterday
- Completed: [list of books/tasks completed]
- Published: [books published]
- Blocked by: [any blockers]

### Today
- Will complete: [planned tasks]
- Need help with: [any issues]

### Metrics
- Books in pipeline: X
- Books published (this week): X
- Daily revenue: $X
- Issues found: X
- Cost spent today: $X
```

## 7.2 Weekly Report Template
```markdown
## Week of: YYYY-MM-DD

### Production
- Books completed: X
- Books published: X
- Pipeline utilization: X%
- Quality pass rate: X%

### Financial
- Revenue: $X (ebooks: $X, KU: $X, paperback: $X)
- Production costs: $X
- Marketing costs: $X
- Net profit: $X
- Per-book ROI: X%

### Marketing
- Active ad campaigns: X
- Average ACOS: X%
- Best performing book: [title]
- New reviews: X

### Quality
- Takedowns: X
- Refund rate: X%
- Average rating: X.X

### Issues & Actions
1. [Issue] → [Action taken / planned]
2. ...

### Next Week Focus
1. ...
2. ...
3. ...
```

## 7.3 Monthly Report Template
```markdown
## Month: YYYY-MM

### Executive Summary
[2-3 sentence summary of month]

### KPI Dashboard
| Metric | Target | Actual | Status |
|--------|--------|--------|--------|
| Revenue | $X | $X | On/Off Track |
| Books published | X | X | On/Off Track |
| Net profit | $X | $X | On/Off Track |
| Avg ROI | X% | X% | On/Off Track |
| Quality score | X | X | On/Off Track |

### Financial Summary
- Total revenue: $X
- Total costs: $X
- Net profit: $X
- Cash balance: $X
- Burn rate: $X/month

### Top Performers (Books)
1. [Title] - $X revenue, X copies sold
2. ...

### Underperformers (Action Required)
1. [Title] - Reason - Action plan
2. ...

### Next Month Plan
- Books to publish: X
- Niches to explore: [list]
- Budget allocation: Production $X, Marketing $X
- Key initiatives: [list]
```

---

# 8. EMERGENCY CONTACTS & PROCEDURES

| Issue | Action | Contact |
|-------|--------|---------|
| API Down | Auto-failover, if fails → check status page | Provider status pages |
| Account Suspended | Stop all activity, document everything | Amazon KDP Support |
| Quality Crisis | Pause publishing, audit all recent books | QA Editor + Owner |
| Cost Overrun | Pause all non-essential pipelines | Owner |
| Legal Issue | Cease related activity, document | Legal counsel |
| Data Security | Rotate all API keys, assess exposure | Developer + Owner |

---

# 9. CONTINUOUS IMPROVEMENT

## 9.1 Retrospective (Monthly)
After each month, conduct a retrospective:
1. **What worked well?** (Keep doing)
2. **What didn't work?** (Stop or change)
3. **What should we try?** (Experiment next month)
4. **Key learnings** (Document for future reference)

## 9.2 Process Optimization
- Track time spent per SOP step
- Identify bottlenecks (> 30% of time on one step)
- Automate repetitive manual steps
- Update SOPs quarterly based on learnings

## 9.3 Knowledge Base
- Document all learnings in `NOTES & LEARNINGS` section
- Share winning prompts, niche insights, marketing tactics
- Build internal wiki as team grows

---

# 10. NOTES & LEARNINGS

*Use this section to capture insights during operations:*

```
Date: YYYY-MM-DD
Category: [Niche | Production | Quality | Marketing | Technical]
Learning: [What was learned]
Action: [What to do about it]
Impact: [High | Medium | Low]
```

---

*Document Version: 3.0*
*Previous Version: 2.0 (OPERATING_MANUAL.md -- added cross-references to numbered doc set)*
*Next Review: Monthly*
