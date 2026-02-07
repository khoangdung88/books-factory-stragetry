# AI BOOK PRODUCTION ENGINE
## Vision, Strategy & Business Model

**Document Version:** 3.0
**Last Updated:** 2026-02-07
**Status:** Active - Living Document
**Reading Order:** 01 of 08 — See [00_INDEX.md](00_INDEX.md) for full document map

---

# 1. TỔNG QUAN

## 1.1 Mục tiêu hệ thống
Xây dựng một "AI Content Production Engine" chuyên:
- Phân tích thị trường dựa trên dữ liệu thực
- Tạo sách bằng AI với chất lượng xuất bản
- Sản xuất nội dung quy mô lớn, có kiểm soát
- Xuất bản đa nền tảng, đa ngôn ngữ
- Tối ưu doanh thu bằng vòng lặp dữ liệu

Không phải:
- Nền tảng đọc sách (Kindle)
- Marketplace
- AI research lab

Đây là:
> Hệ thống sản xuất nội dung tự động hóa dựa trên dữ liệu, vận hành như một nhà máy số — phụ thuộc vào hệ sinh thái dịch vụ SaaS/API bên ngoài để vận hành.

**Service dependency model:**
Nhà máy sử dụng ~15-20 dịch vụ SaaS/API bên ngoài xuyên suốt pipeline. Quản trị service ecosystem là yếu tố sống còn.

> Chi tiết vendor management: [06_SERVICES.md](06_SERVICES.md)

---

## 1.2 Vị trí trong chuỗi giá trị

Chuỗi giá trị ngành sách:
```
Author → Publisher → Platform → Reader
```

Hệ thống này nằm ở:
> **Publisher + Content Factory**

Không cạnh tranh với:
- Amazon Kindle (nền tảng phân phối)
- Nhà xuất bản truyền thống (thương hiệu cao cấp)

Cạnh tranh trực tiếp với:
- Self-publisher cá nhân (lợi thế tốc độ + data)
- Content mill thủ công (lợi thế chi phí + nhất quán)
- AI publisher khác (lợi thế chất lượng + hệ thống)

---

# 2. CẤU TRÚC PHÁP LÝ & QUẢN TRỊ

## 2.1 Business Entity

### Giai đoạn MVP (Tháng 1-3)
- **Hình thức:** Sole proprietor hoặc LLC (US-based recommended cho KDP)
- **Lý do:** Đơn giản, chi phí thấp, bảo vệ trách nhiệm cá nhân
- **Đăng ký:** Wyoming/Delaware LLC (thuế thấp, privacy tốt)

### Giai đoạn Scale (Tháng 4-12)
- **Hình thức:** LLC chuyển đổi sang S-Corp (nếu lợi nhuận > $40K/năm)
- **Lý do:** Tối ưu thuế self-employment

### Giai đoạn Industrial (Năm 2+)
- **Hình thức:** C-Corp nếu huy động vốn
- **Cân nhắc:** Cấu trúc holding nếu đa thương hiệu

## 2.2 Governance Framework

### Decision Matrix
| Quyết định | Người quyết định | Ngưỡng phê duyệt |
|------------|------------------|-------------------|
| Chi tiêu < $500 | Operator | Tự quyết |
| Chi tiêu $500-$2000 | Manager | Email approval |
| Chi tiêu > $2000 | Owner | Written approval |
| Niche mới | Content Operator + Owner | Scoring > 7.0 |
| Publish sách | QA Reviewer | Quality gate pass |
| Thay đổi pricing | Owner | Data review required |
| Thuê người mới | Owner | Budget + JD approved |
| Thay đổi tech stack | Architect + Owner | Impact analysis |

### Intellectual Property
- **Pen names:** Đăng ký trademark cho các pen name chính
- **Cover art:** Luôn sở hữu quyền thương mại (kiểm tra license AI image)
- **Content:** Theo dõi chính sách bản quyền AI content từng quốc gia
- **Domain/Brand:** Đăng ký domain cho mỗi pen name/series lớn

## 2.3 Compliance & Regulatory

### Amazon KDP Compliance
- [ ] AI Disclosure: Amazon yêu cầu khai báo AI-generated content từ 2023
- [ ] Không spam/duplicate content
- [ ] Không vi phạm trademark trong title
- [ ] Không health/legal/financial advice cụ thể
- [ ] Tuân thủ dimension và format requirements

### Tax Compliance
- [ ] W-9 (US) hoặc W-8BEN (non-US) cho KDP
- [ ] Theo dõi income từ tất cả platforms
- [ ] Quarterly estimated tax payments (nếu US entity)
- [ ] Giữ receipts cho tất cả business expenses
- [ ] Separate bank account cho business

### AI Regulatory Landscape
- **EU AI Act:** Theo dõi yêu cầu labeling cho AI-generated content
- **US:** Theo dõi FTC guidelines về AI disclosure
- **Amazon/Kobo:** Cập nhật chính sách platform hàng quý

---

# 3. BUSINESS MODEL CANVAS

## 3.1 Sản phẩm chính

### Tier 1 - Core Products (MVP)
- Ebook niche (non-fiction, English, Amazon KDP)
- Sách hướng dẫn / how-to
- Sách kỹ năng / self-help

### Tier 2 - Expansion Products (Month 4-12)
- Workbook / journal (low-content books)
- Series / bundle packages
- Paperback via KDP Print

### Tier 3 - Diversification (Year 2+)
- Audiobook (AI voice + human narration)
- Fiction series (romance, mystery - high volume niches)
- Multi-language editions
- Direct sales channel (Gumroad/Shopify)
- Corporate/B2B content production (training manuals, white papers)

## 3.2 Nguồn doanh thu

### A. Direct Publishing Revenue
| Kênh | Margin | Khả năng scale | Priority |
|------|--------|----------------|----------|
| Amazon KDP (ebook) | 70% royalty | Cao | P0 - MVP |
| Amazon KDP (paperback) | 40-60% | Trung bình | P1 |
| Kindle Unlimited (KENP) | ~$0.005/page | Rất cao | P0 - MVP |
| Kobo/B&N | 70% | Trung bình | P2 |
| Direct sales (Gumroad/Shopify) | 90%+ | Thấp ban đầu | P2 |
| Audiobook (ACX/Findaway) | 40-70% | Cao | P3 |

> Chi tiết chi phí & revenue model: [02_ECONOMICS.md](02_ECONOMICS.md)

## 3.3 USP 3 tầng

### Tầng 1 – Data Advantage (Moat)
- Niche intelligence proprietary database
- Trend detection sớm hơn thị trường
- Keyword mining + competitor analysis tự động
- Historical performance data across 500+ books

### Tầng 2 – Production Advantage (Speed)
- Pipeline tự động < 48h / book
- Multi-agent production kiểm soát chất lượng
- Tốc độ xuất bản 20-50 sách/tháng
- Chi phí sản xuất < $30/book

### Tầng 3 – Optimization Advantage (Compounding)
- A/B testing title, cover, description
- Rewrite loop dựa trên reader feedback
- Cross-promotion giữa books trong portfolio
- Revenue attribution per niche/keyword

---

# 4. PHÂN TÍCH CẠNH TRANH

## 4.1 Competitive Landscape

| Đối thủ | Điểm mạnh | Điểm yếu | Mối đe dọa |
|---------|-----------|-----------|-------------|
| Self-publishers (cá nhân) | Authentic voice | Chậm, không scale | Thấp |
| Content mills (Fiverr/Upwork) | Rẻ | Chất lượng thấp, không data | Thấp |
| AI publishers (competitors) | Giống ta | Không có quality loop | Trung bình |
| Traditional publishers | Brand mạnh | Chậm, chi phí cao | Thấp |
| Amazon's own AI tools | Distribution advantage | Không focus on quality | Cao |

## 4.2 Competitive Moat Strategy (4 Layers)
1. **Data moat:** Mỗi sách published tạo thêm data → niche intelligence tích lũy
2. **Process moat:** Pipeline ngày càng tối ưu → khó replicate overnight
3. **Portfolio moat:** Cross-promotion, series, loyal reader base
4. **Quality moat:** AI + human editorial layer → khó copy

> Service ecosystem advantage: Quan hệ tốt với AI providers (volume pricing, startup credits) + multi-vendor redundancy → chi phí thấp hơn và ổn định hơn đối thủ mới.

---

# 5. KIẾN TRÚC HỆ THỐNG (Overview)

## 5.1 Tổng quan kiến trúc

```
DATA SOURCES
↓
Market Intelligence (Niche scoring, trend detection)
↓
Ideation & Planning (Concept generation, outline)
↓
Content Production (Multi-agent writing pipeline)
↓
Editorial & Quality (Plagiarism, fact-check, compliance)
↓
Packaging & Distribution (Cover, format, metadata, publish)
↓
Analytics & Feedback Loop (Sales, reviews, optimization)
↓
[Loop back to Market Intelligence]
```

> Chi tiết pipeline architecture: [03_PIPELINE.md](03_PIPELINE.md)
> Chi tiết agent specs: [04_AGENTS.md](04_AGENTS.md)
> Chi tiết vendor/service integration: [06_SERVICES.md](06_SERVICES.md)

## 5.2 Market Intelligence Layer
- Phân tích niche opportunity
- Dự đoán demand qua keyword/trend data
- Competitive analysis tự động
- Output: Niche score (0-10) with breakdown

## 5.3 Ideation & Planning Layer
- Tạo concept từ niche data (10-20 concepts per niche)
- Outline architecture
- Series planning
- Human review checkpoints (Gate 1 + Gate 2)

## 5.4 Content Production Layer
- Writing agent (chapter-by-chapter)
- Continuity agent (consistency tracking)
- Context management (chapter summaries)
- Multi-provider: Groq → Gemini → OpenAI → Anthropic

## 5.5 Editorial & Quality Layer
- Plagiarism detection (< 5% threshold)
- Hallucination detection
- Style consistency scoring
- Legal/compliance review
- Human review checkpoint (mandatory, Gate 3)

## 5.6 Packaging & Distribution Layer
- Cover generation (AI + template system)
- EPUB/PDF formatting (Calibre integration)
- Metadata SEO optimization
- Multi-platform upload (KDP, Kobo, Google Play)
- Human review checkpoint (Gate 4)

## 5.7 Analytics & Feedback Loop
- Conversion rate tracking
- Review sentiment analysis
- BSR tracking (daily)
- Revenue per niche/book/keyword
- Automated triggers (rewrite, cover A/B test, sequel, archive)

---

# 5.5 NICHE SELECTION FRAMEWORK

## The "Golden Niche" Criteria

### Demand Signals (Score 0-10)
| Criteria | Weight | How to Measure |
|----------|--------|----------------|
| Search Volume | 30% | Amazon autocomplete depth, Google Trends |
| BSR Performance | 25% | Top 10 books BSR < 50,000 |
| Review Velocity | 20% | New reviews/month on competitors |
| Price Point | 15% | Average price $4.99-$14.99 sweet spot |
| Competition Quality | 10% | Are top books actually well-written? |

### Red Flags (Automatic Disqualify)
- [ ] Dominated by celebrity/brand authors
- [ ] Requires credentials (medical, legal, financial advice)
- [ ] High lawsuit risk (defamation potential)
- [ ] Saturated with AI content already (check reviews for "AI written" complaints)
- [ ] Amazon category heavily moderated
- [ ] Niche too narrow (< 1000 searches/month)
- [ ] Niche too broad (impossible to rank)

## Niche Scoring Model

```
NICHE_SCORE = (
    demand_score * 0.30 +
    competition_score * 0.25 +
    profit_potential * 0.25 +
    production_feasibility * 0.20
)

DECISION MATRIX:
- Score >= 8.0 → PROCEED immediately (high confidence)
- Score 7.0-7.9 → PROCEED with specific differentiation angle
- Score 6.0-6.9 → REVIEW manually, needs strong unique angle
- Score < 6.0 → SKIP, move to next niche
```

## MVP Niche Recommendations

### Tier 1 - High Confidence (Start Here)
1. **Productivity & Time Management**
   - Evergreen demand, no seasonal dependency
   - Low research requirement
   - High keyword variety (100+ keywords)
   - Series potential: morning routines, deep work, habits

2. **Personal Finance Basics**
   - Clear structure (budgeting, saving, investing basics)
   - Workbook potential (journal + guide combos)
   - Series opportunity (beginners, millennials, couples)
   - Disclaimer: General information only, no specific financial advice

3. **Career & Professional Development**
   - Interview prep, resume writing, negotiation, leadership
   - Practical, actionable content (high perceived value)
   - Target: professionals willing to pay $9.99+

4. **Mindset & Mental Models**
   - Low fact-check burden (frameworks, not claims)
   - High repackaging potential
   - Evergreen + growing demand

### Tier 2 - Medium Confidence (Month 2+)
- Parenting guides (specific ages/challenges)
- Hobby how-tos (gardening, cooking basics, crafts)
- Relationship communication skills
- Small business / side hustle guides

### Tier 3 - Avoid for MVP
- Health/medical (liability risk too high)
- Legal advice (liability risk too high)
- Current events (outdates quickly)
- Fiction (quality bar too high for MVP, plan for month 4+)
- Crypto/investment specific (regulatory + volatile)

---

# 6. MARKETING STRATEGY

## 6.1 Marketing Funnel

```
DISCOVERY (Top of Funnel)
├── Amazon Search (organic keywords)
├── Amazon Browse (category ranking)
├── Amazon Ads (Sponsored Products)
├── Also-Bought / Recommended
└── External search (Google → Amazon)

CONSIDERATION (Middle of Funnel)
├── Cover (thumbnail appeal)
├── Title + Subtitle (promise)
├── Description (AIDA copy)
├── Reviews (social proof)
├── Look Inside (quality check)
└── Price (value perception)

PURCHASE (Bottom of Funnel)
├── Buy now
├── Kindle Unlimited download
└── Wishlist → Later purchase

RETENTION (Post-Purchase)
├── Read-through rate
├── Review request (back of book)
├── Email list signup (lead magnet in book)
├── Cross-promotion (other books by same author)
└── Series continuation
```

## 6.2 Amazon Ads Strategy

### Phase 1: Discovery (Week 1-2 per book)
- **Campaign type:** Automatic targeting
- **Budget:** $5/day
- **Goal:** Discover profitable keywords
- **Duration:** 14 days

### Phase 2: Optimization (Week 3-4)
- **Campaign type:** Manual targeting (exact keywords from Phase 1)
- **Budget:** $10/day for winners
- **Goal:** ACOS < 50%
- **Actions:** Pause keywords with ACOS > 70%, increase bids on ACOS < 30%

### Phase 3: Scale (Month 2+)
- **Budget:** Scale profitable campaigns to $20-50/day
- **New campaigns:** Competitor ASIN targeting
- **Goal:** ACOS < 40%, profit per book > $2

### Budget Allocation Per Book
```
Month 1: $50-100 (testing)
Month 2: $50-150 (if profitable, scale; if not, pause)
Month 3+: $0-300 (only for proven performers)

Total marketing budget MVP (10 books): $500-1000
```

> Chi tiết marketing costs & ROI: [02_ECONOMICS.md](02_ECONOMICS.md)

## 6.3 Book Launch Playbook

### Pre-Launch (3 days before)
- Finalize metadata (title, description, keywords)
- Select cover variant
- Set up Amazon Ad campaigns (paused)
- Prepare KDP listing

### Launch Day
- Publish at $0.99 promotional price
- Activate Amazon Ads (automatic campaign)
- Monitor for any publishing issues

### Week 1
- Monitor sales and ad performance
- Adjust bids based on early data
- Check for any reader feedback

### Week 2
- Raise price to $2.99 (still competitive)
- Launch manual keyword campaigns
- Evaluate initial ACOS

### Week 3-4
- Raise price to $4.99 (target price)
- Optimize ads based on 3-week data
- Decide: scale marketing or archive book

### Month 2+
- Monthly performance review
- Top 20% books: increase ad budget
- Bottom 20%: pause ads, consider rewrite or archive
- Middle 60%: maintain current spend

## 6.4 Cross-Promotion Strategy
- **Back-of-book:** "Also by this author" page with links
- **Series bundling:** Discount for buying series
- **Email capture:** Free resource/checklist in exchange for email
- **Pen name brand:** Each pen name = consistent niche authority

## 6.5 Content Marketing (Month 3+)
- Blog/website cho mỗi pen name (SEO traffic → book sales)
- Email list building (lead magnet = free chapter)
- Social media presence (minimal, high-value posts only)

---

# 7. KPI ENGINE

## 7.1 Production KPIs
| KPI | MVP Target | Scale Target | Industrial Target |
|-----|-----------|-------------|-------------------|
| Idea → Publish | < 7 ngày | < 3 ngày | < 24 giờ |
| Sách/operator/tháng | 10-15 | 20-30 | 40-50 |
| Cost sản xuất/sách | < $30 | < $20 | < $15 |
| Pipeline automation | > 60% | > 80% | > 90% |
| Quality pass rate | > 85% | > 90% | > 95% |

## 7.2 Business KPIs
| KPI | MVP Target | Scale Target | Industrial Target |
|-----|-----------|-------------|-------------------|
| Revenue/tháng | $500+ | $5,000+ | $30,000+ |
| ROI per book | > 200% | > 500% | > 1000% |
| Amazon acceptance rate | > 95% | > 98% | > 99% |
| Avg rating | > 3.5 | > 4.0 | > 4.2 |
| Refund rate | < 5% | < 3% | < 2% |

## 7.3 Marketing KPIs
| KPI | Target |
|-----|--------|
| ACOS (Ad Cost of Sales) | < 50% |
| Organic/Paid sales ratio | > 60% organic |
| Email list growth | 100 subscribers/month |
| Review acquisition rate | 1 review per 50 sales |

> Chi tiết financial KPIs: [02_ECONOMICS.md](02_ECONOMICS.md) Section 6
> Chi tiết operational KPIs: [07_OPERATIONS.md](07_OPERATIONS.md) Section 6

---

# 8. ROADMAP TRIỂN KHAI

## Tháng 1: Foundation + First Books
- Setup legal entity + accounts
- Build MVP pipeline
- Publish 10 books
- Start data collection

## Tháng 2: Optimize + Scale
- Analyze first batch performance
- Optimize prompts + pipeline
- Start Amazon Ads
- Publish 15-20 books
- Implement feedback loop

## Tháng 3: Validate Model
- Scale to 30-50 total books
- Validate unit economics
- Identify winning niches
- Start series for top performers
- Plan paperback expansion

## Tháng 4-6: Growth
- 100+ total books
- Multi-niche diversification
- Hire content operator
- Launch email lists per pen name
- Test Kobo/Google Play
- Multi-language planning begins

## Tháng 7-12: Industrial Scale
- 200+ total books published
- Multi-platform distribution (Kobo, Google Play, Draft2Digital)
- Audiobook pipeline pilot test
- Advanced analytics + automated feedback loop
- Tối ưu vendor contracts (volume pricing với AI providers)
- Revenue target: $5,000+/month
- Evaluate expansion opportunities cho năm 2

## Multi-Language Strategy (Year 2+)
| Language | Market Size | Strategy |
|----------|-----------|----------|
| Spanish | Large | AI translation + native editor review |
| German | Medium-High | AI translation + native editor review |
| Portuguese | Medium | AI translation + native editor review |
| French | Medium | AI translation + native editor review |

**Approach:** Translate proven winners first (books with 100+ English sales)

## Revenue Diversification Timeline
```
Month 1-3:   100% Amazon KDP
Month 4-6:   85% Amazon, 10% Kobo/GP, 5% Direct
Month 7-12:  70% Amazon, 15% Other marketplaces, 15% Direct
Year 2:      55% Amazon, 20% Other, 15% Direct, 10% Audiobooks
Year 3:      40% Amazon, 20% Other, 20% Direct, 10% Audio, 10% B2B
```

> Chi tiết vendor/service roadmap: [06_SERVICES.md](06_SERVICES.md) Appendix A
> Chi tiết 28-day execution plan: [08_MVP_CHECKLIST.md](08_MVP_CHECKLIST.md)

---

# 9. RỦI RO & MITIGATION

## 9.1 Critical Risks

| Rủi ro | Xác suất | Impact | Mitigation |
|--------|----------|--------|------------|
| Amazon account ban | Trung bình | Rất cao | Multi-account, platform diversification |
| AI content policy change | Cao | Cao | Human editorial layer, disclosure compliance |
| Market saturation | Cao | Trung bình | Niche agility, quality differentiation |
| Legal/copyright issue | Thấp | Cao | Plagiarism check, legal review, disclaimers |
| AI cost tăng đột ngột | Trung bình | Trung bình | Multi-provider, cache, prompt optimization |
| Competitor copy model | Cao | Thấp | Data moat, execution speed, process moat |

## 9.2 Contingency Plans
- **Nếu Amazon ban:** Chuyển sang Kobo + direct sales trong 48h
- **Nếu AI costs tăng 3x:** Chuyển sang open-source models (Llama, Mistral)
- **Nếu quality bị complaint:** Pause publishing, audit toàn bộ, tăng human review
- **Nếu revenue < projection 50%:** Cắt costs, focus top 20% books, pivot niche

> Chi tiết vendor risk management: [06_SERVICES.md](06_SERVICES.md) Part 7
> Chi tiết financial risk: [02_ECONOMICS.md](02_ECONOMICS.md) Section 8

---

# 10. NGUYÊN TẮC THIẾT KẾ

**Không xây:**
- Nền tảng đọc sách
- AI model riêng từ đầu
- Hệ thống quá phức tạp trước khi validate

**Tập trung:**
- Orchestration (kết nối agents hiệu quả)
- Pipeline automation (giảm manual steps)
- Data loop (mỗi sách = data point mới)
- Iteration tốc độ cao (ship fast, learn fast)
- Unit economics (profitable per book trước khi scale)

---

# 11. ĐỊNH NGHĨA CUỐI CÙNG

AI Book Production Engine =
> Hệ thống sản xuất tri thức tự động dựa trên dữ liệu,
> có khả năng tạo, xuất bản và tối ưu nội dung liên tục,
> vận hành với kỷ luật tài chính và quản trị rủi ro của một doanh nghiệp.

Nó là:
- **Content factory** (sản xuất quy mô)
- **Knowledge refinery** (tinh chế thông tin)
- **Data-driven publisher** (ra quyết định bằng dữ liệu)
- **Scalable business** (model có thể nhân rộng)

---

*Document Version: 3.0*
*Previous Version: 2.0 (idea.md — tech stack & resource sections moved to specialized docs)*
*Next Review: Monthly*
