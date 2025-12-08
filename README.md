# AI Coding Model Benchmark Analysis (Integrated)

**December 2025 | 13 Models | 6 Tasks | Component-Level Breakdown + Root Cause Analysis**

---

## Executive Summary

### 🏆 **Top 3 Models**

| Rank | Model | Score | Best For | Key Insight |
| --- | --- | --- | --- | --- |
| **1** | **Gemini Pro 3 thinking** | 9.632 | Enterprise, all domains | 4/6 perfect scores (10.0), 0.278 StdDev (most consistent) |
| **2** | **Mistral** | 9.388 | General-purpose default | No weak rubric components; 9.2+ on all tasks |
| **3** | **Ernie 4.5 Turbo** | 8.814 | Security-critical apps | Best Task 4 (9.86); one implementation flaw (obfuscation) |

### 📊 **Key Findings**

**Consistency and root cause matter more than peak scores.**

- Gemini Pro 3 wins with 4 perfect scores + exceptional consistency
- Mistral places 2nd with no perfect scores but rock-solid rubric components across all tasks
- Grok 4.1 has 4 perfect scores but a **syntax error on Task 2** (code doesn't compile) → drops to #7
- Claude Haiku has the best Task 5 score (9.95) but an **architectural failure on Task 2** (threading conflict) → ranks #8

### 🚨 **Critical Insight: Root Cause Matters**

| Failure Type | Example | Severity | Fix Difficulty |
| --- | --- | --- | --- |
| **Syntax Error** (code doesn't compile) | Grok 4.1 Task 2: ` WIDTH // // 2` | CRITICAL | Trivial (typo) |
| **Fatal Logic Bug** (crashes on use) | GMT4.6 Task 4: calls `_decrypt` on create | CRITICAL | Simple (wrong function name) |
| **Architectural Flaw** (design choice failed) | Haiku Task 2: threading + blocking I/O | SEVERE | Redesign required |
| **Implementation Gap** (feature missing) | Qwen2.5 Task 4: plaintext keys | HIGH | Rewrite security logic |
| **Policy Limit** (safety filter) | Gemini Flash Tasks 3 & 6: refused | MEDIUM | Would rank #2 if allowed |

---

## Task Descriptions

| Task | Name | Domain | What It Tests |
| --- | --- | --- | --- |
| **Task 1** | Word Counter & Text Analyzer | Fundamentals | Basic Python, data structures, edge case handling |
| **Task 2** | Snake Game CLI | Real-time Systems | Event-driven state management, terminal I/O, concurrency |
| **Task 3** | Code Obfuscation & Encryption | Security/Crypto | AST manipulation, encryption pipelines, key derivation |
| **Task 4** | Secure Note-Taking Application | Security-Critical | Per-note encryption, PBKDF2, file permissions, audit logging |
| **Task 5** | RESTful API with JWT Authentication | Backend Systems | JWT tokens, relational databases, endpoint design, testing |
| **Task 6** | Arduino NAND Flash Controller | Embedded Systems | ONFI protocol, timing-critical code, hardware abstraction |

---

## Methodology Overview

### **Three Scoring Approaches**

#### **1. Equal Weighting (Baseline)**

```
Score = (T1 + T2 + T3 + T4 + T5 + T6) / 6 − Variance Penalty
```

Each task = 16.67%. **Use case:** All domains equally important (enterprises, unknown priorities).

#### **2. Backend-Prioritized Weighting**

```
Score = (T1×0.10) + (T2×0.10) + (T3×0. 20) + (T4×0. 25) + (T5×0. 30) + (T6×0. 05) − Variance Penalty

Task Weights:
- T1 (Word Counter): 10%
- T2 (Snake Game): 10%
- T3 (Obfuscation): 20%
- T4 (Secure Notes): 25%
- T5 (REST API): 30% ← HIGHEST (backend focus)
- T6 (Arduino): 5%
```

**Use case:** API-first startups, SaaS platforms, backend-heavy teams.

#### **3. Consistency Penalty**

```
Penalty = StdDev × 0.7
Adjusted Score = Raw Score − Penalty
```

**Rationale:** A model scoring [10, 10, 10, 10, 10, 6] is riskier than [9, 9, 9, 9, 9, 9]. Production systems require reliability.

**Example:**

- **Grok 4.1:** StdDev 1.619 → Penalty 1.133 (perfect on 4 tasks, crashes on 1)
- **Mistral:** StdDev 0.274 → Penalty 0.192 (consistent 9.2+ everywhere)

---

## Final Rankings — Equal Weighting (Primary)

| Rank | Model | Raw Avg | StdDev | Penalty | Adjusted | Key Factor |
| --- | --- | --- | --- | --- | --- | --- |
| **1** | **Gemini Pro 3 thinking** | 9.83 | 0.278 | 0.195 | **9.632** | 4 perfect scores + lowest variance |
| **2** | **Mistral** | 9.58 | 0.274 | 0.192 | **9.388** | No weak components; rock-solid consistency |
| **3** | **Ernie 4.5 Turbo** | 9. 19 | 0.537 | 0.376 | **8.814** | Best Task 4 (9.86); weak on obfuscation |
| **4** | **GPT-5. 1** | 9.08 | 0.527 | 0.369 | **8.711** | Solid but inconsistent; weak on embedded |
| **5** | **DeepSeek V3** | 9.30 | 0.913 | 0.639 | **8.661** | Excellent protocols/crypto; fails at UI |
| **6** | **Claude Sonnet** | 9.16 | 1.219 | 0.853 | **8.307** | High variance; strong Tasks 1, 3, 4 |
| **7** | **Grok 4.1** | 9. 30 | 1.619 | 1.133 | **8.167** | 4 perfect scores but syntax error on Task 2 |
| **8** | **Claude Haiku 4.5** | 9.02 | 1.444 | 1.011 | **8.009** | Best Task 5 (9.95); architectural failure Task 2 |
| **9** | **GMT4.6** | 8.43 | 1.757 | 1. 230 | **7.200** | Backend expert; fatal bug on Task 4 |
| **10** | **Gemini Flash 2.5** | 7.19 | 0. 369* | 0.258 | **6.934** | Perfect on allowed tasks; safety filter refuses Tasks 3 & 6 |
| **11** | **Qwen3-Max** | 7.87 | 1. 424 | 0.997 | **6.873** | Weak fundamentals (Task 1 NameError) |
| **12** | **Llama 4** | 6.96 | 2. 193 | 1.535 | **5.425** | Crashes on embedded systems (Task 6: 3.5) |
| **13** | **Qwen2.5-Coder-32B** | 6.95 | 2.463 | 1.724 | **5.226** | Wildly inconsistent; security nightmare (Task 4: plaintext keys) |

*Gemini Flash 2.5: Refusals scored as 2/10 in average calculation (7.19). Variance penalty calculated only on completed tasks (1, 2, 4, 5), yielding StdDev of 0.369.

---

## Final Rankings — Backend-Prioritized Weighting (Alternative)

This weighting emphasizes Tasks 4 (25%) and 5 (30%) while de-emphasizing Task 2 (10%) and Task 6 (5%).

| Rank | Model | Weighted Avg | Penalty | Adjusted | Change vs. Equal |
| --- | --- | --- | --- | --- | --- |
| **1** | **Gemini Pro 3 thinking** | 9.71 | 0.195 | **9.515** | —   |
| **2** | **Mistral** | 9. 47 | 0.192 | **9.278** | —   |
| **3** | **Ernie 4.5 Turbo** | 9.35 | 0.376 | **8.974** | —   |
| **4** | **DeepSeek V3** | 9.48 | 0.639 | **8.841** | ↑1  |
| **5** | **GPT-5.1** | 9.18 | 0.369 | **8.811** | ↓1  |
| **6** | **Claude Sonnet** | 9.39 | 0.853 | **8.537** | —   |
| **7** | **Claude Haiku 4.5** | 9.32 | 1.011 | **8.309** | ↑1  |
| **8** | **Grok 4.1** | 9.52 | 1.133 | **8.387** | ↓1  |
| **9** | **GMT4.6** | 8.23 | 1.230 | **7.000** | —   |
| **10** | **Gemini Flash 2.5** | 7.84 | 0.258 | **7.582** | —   |
| **11** | **Qwen3-Max** | 8.36 | 0.997 | **7.363** | —   |
| **12** | **Qwen2.5-Coder-32B** | 6.89 | 1.724 | **5.166** | ↑1  |
| **13** | **Llama 4** | 7.16 | 1.535 | **5.625** | ↓1  |

**Key Shifts:**

- **Claude Haiku 4.5** rises because its exceptional Task 5 (9.95) receives 30% weight instead of 16.67%.
- **Grok 4.1** falls because its Task 2 failure (6.0) is de-emphasized but still contributes to variance.
- **DeepSeek V3** rises due to strong Task 4 (9.93) and Task 5 (9.51) performance.

## Backend-Prioritized Weighting Rationale

The alternative weighting scheme adjusts task importance based on real-world applicability and verification confidence:

| Task | Weight | Rationale |
| --- | --- | --- |
| **Task 1** (Word Counter) | 10% | **Very simple** — Tests basic fundamentals but doesn't differentiate between capable models; 12/13 models scored 9.4+ |
| **Task 2** (Snake Game) | 10% | **Limited applicability** — Real-time CLI games don't apply to most work environments; threading/terminal I/O patterns are niche |
| **Task 3** (Obfuscation) | 20% | **Mixed relevance** — Encryption pipelines and key derivation (PBKDF2) are enterprise-relevant; code obfuscation itself is niche (DRM, proprietary protection), but the crypto fundamentals justify moderate weight |
| **Task 4** (Secure Notes) | 25% | **Enterprise security standard** — Per-note encryption, PBKDF2, file permissions, and audit logging are standard requirements in security-conscious organizations |
| **Task 5** (REST API) | 30% | **Core enterprise skill** — JWT authentication, relational databases, and API design represent the most common enterprise development work; highest weight reflects highest real-world demand |
| **Task 6** (Arduino NAND) | 5%  | **Verification limitations** — While realistic and arguably the most technically complex task, verification was limited to "would this code execute on real hardware?" without ability to confirm correct behavior beyond structural analysis |

### Weight Distribution Summary

```
Enterprise Core (Tasks 3, 4, 5):  75%  ← Highest confidence + applicability
Fundamentals (Task 1):            10%  ← Necessary but not differentiating  
Niche/Limited (Tasks 2, 6):       15%  ← Lower applicability or verification confidence
```

### Key Principle

**Weight reflects (Applicability × Verification Confidence)**

- High applicability + high verification confidence = high weight (Task 5: 30%)
- High applicability + high verification confidence = high weight (Tasks 3, 4: 20–25%)
- Low applicability = low weight (Task 2: 10%)
- High complexity + low verification confidence = low weight (Task 6: 5%)

---

## Raw Score Reference Table

| Model | Task 1 | Task 2 | Task 3 | Task 4 | Task 5 | Task 6 | **Global Avg** | **Penalty** | **Adjusted** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Gemini Pro 3 thinking** | 9. 73 | 10.0 | 10.0 | 9.93 | 9. 30 | 10.0 | **9.83** | 0.195 | **9. 632** |
| **Mistral** | 9.88 | 9.75 | 9.30 | 9. 56 | 9.2 | 9.76 | **9.58** | 0.192 | **9.388** |
| **Ernie 4.5 Turbo** | 9.4 | 8.8 | 8.43 | 9.86 | 9.4 | 9.64 | **9.19** | 0.376 | **8.814** |
| **GPT-5. 1** | 9.8 | 8.5 | 9.0 | 9. 5 | 9.2 | 8.5 | **9.08** | 0. 369 | **8.711** |
| **DeepSeek V3** | 9.8 | 7.5 | 9.24 | 9.93 | 9.51 | 9.78 | **9.30** | 0.639 | **8.661** |
| **Claude Sonnet** | 9.85 | 6.75 | 9.05 | 9. 875 | 9.675 | 9.76 | **9.16** | 0. 853 | **8.307** |
| **Grok 4.1** | 10. 0 | 6.0 | 10.0 | 10.0 | 9. 8 | 10.0 | **9.30** | 1.133 | **8. 167** |
| **Claude Haiku 4.5** | 9.58 | 6.11 | 9.35 | 9.43 | 9.95 | 9.73 | **9.02** | 1.011 | **8.009** |
| **GMT4.6** | 9.54 | 6. 35 | 9.71 | 6.0 | 9.64 | 9. 36 | **8.43** | 1.230 | **7.200** |
| **Gemini Flash 2.5** | 10.0 | 9.15 | 2. 0* | 10.0 | 10.0 | 2. 0* | **7.19** | 0.258 | **6.934** |
| **Qwen3-Max** | 6.0 | 6.4 | 9.2 | 9.43 | 7.8 | 8.4 | **7.87** | 0.997 | **6.873** |
| **Llama 4** | 9.675 | 6.2 | 7.875 | 8.5 | 6.0 | 3.5 | **6.96** | 1.535 | **5.425** |
| **Qwen2.5-Coder-32B** | 9.925 | 5.1 | 6.75 | 3.8 | 9.74 | 6.4 | **6.95** | 1.724 | **5.226** |

*Gemini Flash 2.5: Tasks 3 and 6 refused due to safety filters; scored as 2/10.

---

## Why Rankings Changed (Root Cause Analysis)

### **🥇 Why Gemini Pro 3 Wins (#1)**

**Perfect Scores (10.0):**

- Task 2 (Snake Game): Perfect game logic, responsive controls, elegant terminal rendering
- Task 3 (Obfuscation): Fernet + 100k PBKDF2 iterations, complete variable mapping, production-ready
- Task 6 (Arduino NAND): All ONFI protocol codes correct, timing-aware, parameter page parsing, selective + full erase

**Near-Perfect (9.93):**

- Task 4 (Secure Notes): 600k PBKDF2 iterations, atomic file writes, comprehensive audit logging, file permissions

**Solid (9.30, 9.73):**

- Task 5 (REST API): HS256 JWT, relational database; only missing email field in JWT token (best-practice gap, not functional failure)
- Task 1 (Word Counter): Excellent data structures, minor edge case gaps

**Consistency:** 0.278 StdDev (LOWEST) = most reliable model for production.

**Verdict:** The "thinking" capability demonstrably improves performance on complex, timing-critical systems.

---

### **🥈 Why Mistral Places 2nd (#2)**

**Component Breakdown Shows Why:**

| Task | Functionality | Accuracy | Code Quality | Error Handling | Overall |
| --- | --- | --- | --- | --- | --- |
| Task 1 | 10.0 | 10.0 | 9.75 | 9.75 | 9.88 |
| Task 2 | 10.0 | 10.0 | 9.5 | 9.0 | 9.75 |
| Task 3 | 8.0 | 10.0 | 9.75 | 9.75 | 9.30 |
| Task 4 | 9.0 | 9.75 | 9.75 | 10.0 | 9.56 |
| Task 5 | 9.0 | 9.5 | 9.0 | 8.8 | 9.2 |
| Task 6 | 10.0 | 9.75 | 10.0 | 8.5 | 9.76 |

**Key Insight:** Mistral has NO weak rubric components. Every category scores 8.0+. Compare to Grok 4.1:

| Task | Functionality | Accuracy | Code Quality | Error Handling | Overall |
| --- | --- | --- | --- | --- | --- |
| **Task 2** | **2.5** | **8.0** | **7.5** | **7.0** | **6.0** |

Grok's Task 2 fails on code quality due to a syntax error.

**Verdict:** Consistency in rubric components beats perfect scores. Mistral delivers solid implementations across all domains.

---

### **📉 Why Grok 4.1 Ranks #7 Despite 4 Perfect Scores**

**The Paradox:** Grok has more perfect scores (4) than Gemini Pro 3 thinking (4), yet ranks #7 instead of #1.

**Task 2 Breakdown (Snake Game):**

| Component | Grok 4.1 | Issue |
| --- | --- | --- |
| Core Gameplay | 2.5 | Even if error was fixed, the game was bad. |
| Controls & Responsiveness | 8.0 | Works |
| Code Quality | 7.5 | Not portable |
| Rendering & UX | 7.0 | With potential yet malformated |
| **Overall** | **6.0** | **❌ Syntax error: ` WIDTH // // 2'** |

**Root Cause:** Typo — double floor division operator (// //) in the reset() function. Code does not run

**Severity:** CRITICAL — prevents execution entirely. This is worse than just architectural issues like Haiku's threading problem, which at least produces runnable code.

**Variance Impact:**

- Scores: [10.0, 6.0, 10.0, 10.0, 9.8, 10.0]
- StdDev: 1.619 (highest among top-10 models)
- Penalty: 1.133 points

---

### **📈 Why Claude Haiku 4.5 Excels Under Backend Weighting**

**Raw Scores:**

| Task | Haiku Score | Notes |
| --- | --- | --- |
| Task 1 | 9.58 | Solid fundamentals |
| Task 2 | 6.11 | Architectural failure (threading + blocking I/O) |
| Task 3 | 9.35 | Strong obfuscation |
| Task 4 | 9.43 | Industry-grade security |
| **Task 5** | **9.95** | **Best REST API score in entire dataset** |
| Task 6 | 9.73 | Complete ONFI protocol |

**Task 5 Component Breakdown:**

| Component | Haiku Score | Notes |
| --- | --- | --- |
| Auth & JWT | 10.0 | Correct HS256 token generation |
| API Design & Endpoints | 10.0 | All 7 endpoints functional |
| Database & Data | 10.0 | Relational schema, user ownership enforcement |
| Security & Best Practices | 9.75 | Input validation, comprehensive tests, full documentation |
| **Overall** | **9.95** | **Near-perfect — only missing rate limiting** |

**Under Backend-Prioritized Weighting:**

- Task 5 weight: 16.67% → 30% (gain: +13.33%)
- Task 2 weight: 16.67% → 10% (loss: −6.67%)
- Haiku's strength (Task 5: 9.95) is amplified
- Haiku's weakness (Task 2: 6.11) is minimized

**Verdict:** Haiku is genuinely excellent on structured problems. While it struggles with real-time state management. Its also 10× cheaper than Gemini Pro 3.

---

### **🔒 Why Gemini Flash 2.5 Ranks #10 (Safety Filter Impact)**

**The Safety Filter Paradox:**

| Task | Status | Score | Notes |
| --- | --- | --- | --- |
| Task 1 | ✅ Completed | 10.0 | Perfect word counter |
| Task 2 | ✅ Completed | 9.15 | Strong gameplay, clean code |
| **Task 3** | **❌ Refused** | **2.0** | Safety filter: "code obfuscation = code protection tool" |
| Task 4 | ✅ Completed | 10.0 | Perfect encryption, atomic operations |
| Task 5 | ✅ Completed | 10.0 | Perfect REST API |
| **Task 6** | **❌ Refused** | **2. 0** | Safety filter: "embedded systems = restricted hardware" |

**If Refusals Were Allowed:**

- Estimated Task 3: ~10. 0 (based on Task 4 security excellence)
- Estimated Task 6: ~10.0 (based on embedded-adjacent task patterns)
- Projected adjusted score: **~9.8 (would rank #2, ahead of Mistral)**

**Root Cause:** Alignment policy, not capability, limits performance.

**Verdict:** Safety filters create a harder ceiling than actual intelligence on frontier models.

---

### **⬇️ Why GMT4.6 Ranks #9 (Fatal Bug)**

**Root Cause: Fatal Bug on Task 4**

| Task | Score | Root Cause |
| --- | --- | --- |
| Task 1 | 9.54 | ✅ Solid |
| Task 2 | 6.35 | Terminal I/O broken (architectural) |
| Task 3 | 9.71 | ✅ Excellent obfuscation |
| **Task 4** | **6.0** | **❌ FATAL: Calls `_decrypt_note()` on create instead of `_encrypt_note()`** |
| Task 5 | 9. 64 | ✅ Good REST API |
| Task 6 | 9.36 | ✅ Complete NAND protocol |

**Component Breakdown (Task 4):**

| Component | GMT4.6 | Reason |
| --- | --- | --- |
| Encryption & Crypto | 7.5 | Logic failure |
| Security Best Practices | 9.5 | Design is solid |
| Code Quality | 8.0 | Implementation is clean |
| Functionality | 3.0 | **Crashes on first use** |

**The Bug:**

```python
# What it should do:
def create_note(content):
    encrypted_content = _encrypt_note(content)  # ✅

# What GMT4.6 does:
def create_note(content):
    encrypted_content = _decrypt_note(content)  # ❌ Wrong function
    # cannot decrypt plaintext
```

**Verdict:** While it is a "dumb" bug with an easy fix. It effectively prevents ther application from working.

---

## Task-by-Task Component Analysis

### **Task 1: Word Counter (Fundamentals Test)**

| Model | Functionality | Accuracy | Code Quality | Error Handling | Score | Issues |
| --- | --- | --- | --- | --- | --- | --- |
| Grok 4.1 | 10.0 | 10. 0 | 10.0 | 10.0 | **10.0** | ✅ Perfect |
| Gemini Flash 2.5 | 10.0 | 10.0 | 10.0 | 10.0 | **10.0** | ✅ Perfect |
| Qwen2.5-Coder-32B | 10.0 | 10.0 | 9.5 | 10.0 | **9.925** | ✅ Excellent |
| Mistral | 10.0 | 10.0 | 9.75 | 9.75 | **9.88** | ✅ Excellent |
| Claude Sonnet | 10.0 | 9.5 | 9.0 | 10.0 | **9.85** | ✅ Very good |
| DeepSeek V3 | 9.8 | 9.8 | 9.8 | 9.8 | **9.8** | ✅ Exceptional design |
| GPT-5.1 | 10.0 | 9.5 | 9.0 | 10.0 | **9.8** | ✅ Comprehensive |
| Gemini Pro 3 thinking | 10.0 | 9.5 | 9.5 | 10.0 | **9.73** | ✅ Solid |
| Llama 4 | 10.0 | 9.5 | 9.0 | 10.0 | **9.675** | ✅ Excellent |
| Claude Haiku 4.5 | 10.0 | 9.0 | 9.5 | 9.5 | **9.58** | Minor line-counting variance |
| GMT4.6 | 10.0 | 9.75 | 9.5 | 9.5 | **9.54** | Minor verification gaps |
| Ernie 4.5 Turbo | 9.5 | 9.0 | 10.0 | 9.5 | **9.4** | Minor consistency bug |
| **Qwen3-Max** | **6.0** | **5.0** | **5.0** | **5.0** | **6.0** | **❌ NameError exception** |

**Root Cause of Qwen3-Max Failure:** Basic Python error — variable not defined.

---

### **Task 2: Snake Game (Real-Time State Management)**

| Model | Core Gameplay | Controls | Code Quality | Rendering | Score | Root Cause of Failure |
| --- | --- | --- | --- | --- | --- | --- |
| **Gemini Pro 3 thinking** | 10.0 | 10.0 | 10.0 | 10.0 | **10.0** | ✅ Perfect |
| Mistral | 10.0 | 10.0 | 9.5 | 9.0 | **9.75** | ✅ Responsive |
| Gemini Flash 2.5 | 10.0 | 9.0 | 9.5 | 9.5 | **9.15** | ✅ Works |
| Ernie 4.5 Turbo | 10.0 | 9.5 | 9.5 | 9.0 | **8.8** | Minor: no wall rendering |
| GPT-5.1 | 10.0 | 9. 0 | 8.5 | 9.0 | **8.5** | ✅ Playable |
| DeepSeek V3 | 9.0 | 8.0 | 8.5 | 7.5 | **7.5** | ❌ Field misformatted |
| Claude Sonnet | 10.0 | 8.0 | 8.5 | 8.5 | **6.75** | ❌ Broken input handling (threading) |
| Qwen3-Max | 9.0 | 7.5 | 7.0 | 7.0 | **6.4** | ❌ Malformed rendering, Broken input |
| GMT4.6 | 6.0 | 5.0 | 9.5 | 5.5 | **6.35** | ❌ Terminal I/O architecture failure |
| Llama 4 | 10.0 | 8.5 | 8.0 | 8.0 | **6.2** | ❌ Required dependencies. Not portable. |
| Claude Haiku 4.5 | 10.0 | 7.0 | 8.85 | 8.5 | **6.11** | ❌ Threading + blocking I/O conflict |
| Grok 4.1 | 2.5 | 8.0 | 7.5 | 7.0 | **6.0** | ❌ **Syntax error: ` WIDTH // // 2`** Malformed rendering |
| **Qwen2.5-Coder-32B** | 10.0 | 0.0 | 4.0 | 4.0 | **5.1** | ❌ **Syntax error: invalid conditional import** |

**Key Insight:** Task 2 is the **frontier weakness**. Even top-5 models struggle because real-time event-driven state management is underrepresented in training data.

**Failure Patterns:**

- **Architectural failures** (Haiku, Sonnet, GMT4.6, Grok): Threading/blocking I/O mismatches
- **Syntax errors** (Grok, Qwen2.5): Typos prevent execution
- **UI failures** (DeepSeek): Rendering logic breaks

---

### **Task 3: Code Obfuscation & Encryption**

| Model | Obfuscation | Encryption | Pipeline | Code Quality | Score | Issues |
| --- | --- | --- | --- | --- | --- | --- |
| Grok 4.1 | 10.0 | 10. 0 | 10.0 | 10.0 | **10.0** | ✅ Perfect |
| Gemini Pro 3 thinking | 10. 0 | 10.0 | 10.0 | 10.0 | **10. 0** | ✅ Perfect |
| GMT4.6 | 9.5 | 10.0 | 9.75 | 9.5 | **9.71** | ✅ AST-based, production-ready |
| Claude Haiku 4.5 | 9.5 | 9.0 | 9.5 | 9.5 | **9.35** | ✅ String-aware obfuscation |
| Mistral | 8.0 | 10.0 | 9.75 | 9.75 | **9.30** | ✅ Working pipeline |
| DeepSeek V3 | 8.5 | 9.24 | 9.5 | 9.25 | **9.24** | ✅ Excellent crypto |
| Qwen3-Max | 9.0 | 9.5 | 9.0 | 9.0 | **9.2** | ✅ Excellent implementation |
| Claude Sonnet | 9.0 | 9.0 | 9.0 | 9.0 | **9.05** | ✅ Strong implementation |
| GPT-5.1 | 9.0 | 9.0 | 9.0 | 9.0 | **9.0** | ✅ Good implementation |
| Ernie 4.5 Turbo | 7.5 | 9.5 | 8.5 | 8.0 | **8.43** | Symbol table bug |
| Llama 4 | 6.5 | 8.5 | 9.0 | 7.5 | **7.875** | Incomplete variable obfuscation |
| Qwen2.5-Coder-32B | 9.0 | 5.0 | 6.0 | 7.0 | **6.75** | Missing base64 import |
| **Gemini Flash 2.5** | —   | —   | —   | —   | **2.0** | **❌ Refused (safety filter)** |

---

### **Task 4: Secure Notes (Security-Critical)**

| Model | Encryption | Security Practices | Code Quality | Functionality | Score | Issues |
| --- | --- | --- | --- | --- | --- | --- |
| Grok 4.1 | 10.0 | 10. 0 | 10.0 | 10.0 | **10.0** | ✅ Perfect |
| Gemini Flash 2.5 | 10.0 | 10.0 | 10.0 | 10.0 | **10.0** | ✅ Perfect |
| Gemini Pro 3 thinking | 10.0 | 9.75 | 10.0 | 10.0 | **9.93** | ✅ Exceptional |
| DeepSeek V3 | 10.0 | 9. 75 | 10.0 | 10.0 | **9.93** | ✅ Exceptional |
| Claude Sonnet | 10.0 | 9.0 | 9.5 | 9.5 | **9.875** | ✅ Industry standard |
| Ernie 4.5 Turbo | 10.0 | 9.75 | 9.75 | 10.0 | **9.86** | ✅ Near-perfect |
| Mistral | 9.0 | 9.75 | 9.75 | 10.0 | **9.56** | ✅ Good (100k PBKDF2) |
| GPT-5.1 | 8.5 | 8.0 | 8.5 | 9.5 | **9.5** | ✅ Good security |
| Claude Haiku 4.5 | 9. 0 | 9.5 | 9.5 | 10.0 | **9.43** | ✅ Industry-grade |
| Qwen3-Max | 8.5 | 8.0 | 7.5 | 9.5 | **8.5** | File permissions oversight |
| Llama 4 | 8. 5 | 8.0 | 9.5 | 9.5 | **8.5** | Solid implementation |
| **GMT4.6** | 7.5 | 9.5 | 8.0 | **3.0** | **6.0** | **❌ FATAL: calls `_decrypt_note()` on create** |
| **Qwen2.5-Coder-32B** | 2.0 | 3.0 | 8.0 | 2.0 | **3.8** | **❌ Plaintext keys, random key generation** |

**Root Cause Analysis:**

**Qwen2.5-Coder-32B Security Nightmare:**

- Key generation: `os.urandom(32)` (random, not derived from password)
- Key storage: Plaintext in memory
- Encryption: No PBKDF2 iterations
- Impact: Keys could be extracted from memory dumps

---

### **Task 5: REST API with JWT (Backend Systems)**

| Model | Auth & JWT | API Design | Database | Security & Tests | Score | Issues |
| --- | --- | --- | --- | --- | --- | --- |
| Gemini Flash 2.5 | 10.0 | 10. 0 | 10.0 | 10.0 | **10.0** | ✅ Perfect |
| **Claude Haiku 4.5** | 10.0 | 10.0 | 10. 0 | 9.75 | **9.95** | ✅ Best-in-class; only missing rate limiting |
| Grok 4.1 | 10.0 | 9.5 | 10.0 | 9.5 | **9.8** | ✅ Comprehensive |
| Qwen2.5-Coder-32B | 10.0 | 9.75 | 10.0 | 9.0 | **9.74** | ✅ Excellent |
| Claude Sonnet | 10.0 | 9.75 | 10.0 | 9.5 | **9.675** | ✅ Production-ready |
| GMT4.6 | 9.75 | 9.5 | 9.75 | 9.5 | **9.64** | ✅ Factory pattern |
| DeepSeek V3 | 9.5 | 9.5 | 9.75 | 9.25 | **9.51** | ✅ Professional |
| Ernie 4.5 Turbo | 9.5 | 9. 5 | 9.5 | 9.0 | **9.4** | No rate limiting |
| Gemini Pro 3 thinking | 9.0 | 10.0 | 10.0 | 8.0 | **9.30** | ❌ Missing JWT email field |
| GPT-5.1 | 9.0 | 9. 0 | 8.5 | 9.0 | **9.2** | Inconsistent validation |
| Mistral | 9.0 | 9.5 | 9.0 | 8.8 | **9.2** | ❌ Missing comprehensive tests & docs |
| Qwen3-Max | 8.0 | 7.5 | 8.0 | 7.0 | **7.8** | ❌ Typo breaks endpoint |
| Llama 4 | 7.0 | 8.0 | 8.5 | 6.5 | **6.0** | ❌ Multiple security gaps |

**Why Gemini Pro 3 thinking Loses Points Despite Overall Excellence:**

- All endpoints functional ✅
- HS256 JWT generation correct ✅
- Relational database ✅
- **Missing:** Email claim in JWT payload (specification requirement)
- **Missing:** Authentication event logging (best practice)

Result: 9.30 instead of 10.0 — functional but incomplete.

---

### **Task 6: Arduino NAND Flash Controller (Embedded Systems)**

| Model | Protocol | Implementation | Code Structure | Error Handling | Score | Issues |
| --- | --- | --- | --- | --- | --- | --- |
| Grok 4.1 | 10.0 | 10. 0 | 9.5 | 9.5 | **10.0** | ✅ Perfect |
| Gemini Pro 3 thinking | 10. 0 | 10.0 | 10.0 | 10.0 | **10. 0** | ✅ Perfect |
| DeepSeek V3 | 9.75 | 9.75 | 10.0 | 9.5 | **9.78** | ✅ Exceptional |
| Claude Sonnet | 10.0 | 9.5 | 9.5 | 9.5 | **9.76** | ✅ Complete |
| Mistral | 10.0 | 9.75 | 10.0 | 8.5 | **9.76** | Lacks defensive validation |
| Claude Haiku 4.5 | 10.0 | 9.5 | 9.5 | 10.0 | **9.73** | ✅ Complete ONFI protocol |
| Ernie 4.5 Turbo | 9.5 | 9.75 | 9.75 | 9.5 | **9.64** | No full device wipe |
| GMT4.6 | 9.5 | 9.25 | 9.5 | 9.0 | **9.36** | ✅ Complete with atomic GPIO |
| GPT-5.1 | 8.5 | 8.0 | 8.0 | 8.0 | **8.5** | Missing full wipe |
| Qwen3-Max | 9.0 | 7.5 | 8.5 | 8.0 | **8.4** | Syntax error in erase command |
| Qwen2.5-Coder-32B | 7.0 | 6.5 | 7.5 | 2.0 | **6.4** | Missing erase entirely |
| Llama 4 | 4.0 | 5.0 | 6.5 | 2.0 | **3.5** | ❌ Protocol errors |
| **Gemini Flash 2.5** | —   | —   | —   | —   | **2.0** | **❌ Refused (safety filter)** |

---

## Model Profiles (Root Cause Breakdown)

### **🥇 Gemini Pro 3 thinking (9.632) — WINNER**

| Aspect | Details |
| --- | --- |
| **Consistency** | 0.278 StdDev (lowest variance — most reliable) |
| **Perfect Tasks** | 4/6 (Tasks 2, 3, 6 at 10.0; Task 4 at 9. 93) |
| **Weak Tasks** | Task 5: 9.30 (missing JWT email field + logging) |
| **Root Cause of Success** | Extended reasoning capability improves complex systems |
| **Root Cause of Weakness** | Minor specification gaps (best-practice items, not functional failures) |
| **Production Readiness** | EXCELLENT — perfect for mission-critical systems |

---

### **🥈 Mistral (9.388) — RELIABLE ALL-ROUNDER**

| Aspect | Details |
| --- | --- |
| **Consistency** | 0.274 StdDev (2nd-best variance) |
| **Strength Pattern** | 9. 2+ on ALL tasks; no weak rubric components |
| **Peak Tasks** | Task 1: 9.88, Task 6: 9.76 |
| **Weak Tasks** | Task 3: 9.30 (obfuscation could be stronger) |
| **Root Cause of Success** | Balanced implementation depth across domains |
| **Root Cause of Weakness** | Not specialized (no 10.0 peaks, intentionally stable) |
| **Production Readiness** | EXCELLENT — default choice for unknown priorities |

---

### **🥉 Ernie 4.5 Turbo (8.814) — SECURITY SPECIALIST**

| Aspect | Details |
| --- | --- |
| **Consistency** | 0. 537 StdDev (good) |
| **Peak Tasks** | Task 4: 9.86 (best encryption score) |
| **Strong Tasks** | Task 5: 9.4, Task 6: 9.64 |
| **Weak Tasks** | Task 3: 8.43 (symbol table bug in obfuscation) |
| **Root Cause of Success** | Excellent security fundamentals |
| **Root Cause of Weakness** | One implementation flaw (obfuscation) |
| **Production Readiness** | EXCELLENT — ideal for security-conscious development |

---

### **⚠️ Grok 4.1 (8.167) — BRILLIANT BUT CARELESS**

| Aspect | Details |
| --- | --- |
| **Peak Scores** | 4 perfect 10.0s (Tasks 1, 3, 4, 6) |
| **Fatal Weakness** | Task 2: 6.0 — **syntax error prevents compilation** |
| **Variance** | 1.619 StdDev (HIGH) |
| **Root Cause of Failures** | Careless typos; code logic is sound but execution has errors |
| **Bug Severity** | CRITICAL (prevents execution — worse than architectural issues) |
| **Production Readiness** | ⚠️ RISKY — mandatory human verification required |
| **Pattern** | Brilliant when reviewed, fails when not |

---

### **📉 Claude Haiku 4.5 (8.009) — SPECIALIST HIDDEN GEM**

| Aspect | Details |
| --- | --- |
| **Best Task** | Task 5: 9.95 (REST API masterclass) |
| **Strong Tasks** | Task 4: 9.43, Task 6: 9. 73 |
| **Fatal Weakness** | Task 2: 6.11 — **threading + blocking I/O architectural mismatch** |
| **Root Cause of Weakness** | Wrong design choice for event-driven systems (architectural flaw, not implementation error) |
| **Variance** | 1.444 StdDev (specialist profile) |
| **Cost Advantage** | 10× cheaper than Gemini Pro 3; fast inference |
| **Production Readiness** | ✅ EXCELLENT for API-first teams; avoid UI/real-time work |
| **Hidden Value** | Ranked #8 overall but top-3 for backend systems |

---

### **🚨 GMT4.6 (7.200) — BACKEND EXPERT WITH SHOW-STOPPER**

| Aspect | Details |
| --- | --- |
| **Backend Strengths** | Task 3: 9.71, Task 5: 9.64, Task 6: 9.36 |
| **Backend Average** | 9.57/10 (excellent systems programming) |
| **Fatal Bug (Task 4)** | 6.0 — **calls `_decrypt_note()` on create instead of `_encrypt_note()`** |
| **Bug Severity** | CRITICAL (crashes on first use) |
| **Root Cause** | Good design, wrong implementation |
| **Production Readiness** | ⚠️ NOT READY — backend specialist compromised by security bug |
| **If Bug Fixed** | Projected ~9.4–9.5 average |

---

### **🔒 Gemini Flash 2.5 (6.934, projected 9.8 if allowed) — SAFETY FILTER CEILING**

| Aspect | Details |
| --- | --- |
| **Completed Tasks** | 4/6 (Tasks 1, 2, 4, 5) |
| **Perfect Scores** | Tasks 1, 4, 5: 10.0 each |
| **Refused Tasks** | Tasks 3, 6: 2.0/10 (safety filters) |
| **If Refusals Allowed** | Projected ~9.8 average (would rank #2) |
| **Root Cause of Limitation** | Alignment policy, not capability |
| **Production Readiness** | ✅ EXCELLENT for safety-first environments |
| **Real Impact** | −2.9 points from policy decisions |

---

## Deployment Recommendations

### **Scenario 1: Enterprise (All Domains Matter)**

**Weighting:** Equal (16.67% each)

**Winner:** Gemini Pro 3 thinking (9.632)

| Task | Score | Notes |
| --- | --- | --- |
| 1   | 9.73 | Strong fundamentals |
| 2   | 10.0 | Perfect real-time state ✅ |
| 3   | 10.0 | Perfect crypto ✅ |
| 4   | 9.93 | Industry-grade security ✅ |
| 5   | 9.30 | Missing JWT email (best-practice gap) |
| 6   | 10.0 | Perfect embedded ✅ |

**Why Not Others:**

- Mistral: No major gaps, but no perfect scores (too conservative)
- Grok 4.1: Task 2 syntax error disqualifies (crashes)
- Haiku: Task 2 failure disqualifies (real-time systems)

**Verdict:** Consistency + reliability beat specialization for unknown priorities.

---

### **Scenario 2: API-First Startup**

**Weighting:** Custom (Task 5: 55%)

**Winner:** Claude Haiku 4.5

**Why Choose Haiku Over Gemini Pro 3:**

- Haiku Task 5: 9.95 (better than Gemini Pro 3's 9.30)
- Haiku cost: 10× cheaper
- Haiku inference: 10× faster
- Haiku Task 2: 6.11 (but UI not needed for B2B APIs)

**Verdict:** Clear winner for backend teams prioritizing cost and API quality.

---

### **Scenario 3: Security-Critical (Finance/Healthcare)**

**Weighting:** Custom (Task 4: 40%, Task 3: 25%)

**Winner:** Ernie 4.5 Turbo

**Why Ernie Wins:**

- Task 4: 9.86 (best encryption score)
- No fatal bugs (unlike GMT4.6)
- No syntax errors (unlike Grok 4.1)
- Production-ready code quality

---

### **Scenario 4: Safety-First Environment**

**Winner:** Gemini Flash 2.5

**Why:**

- Won't attempt obfuscation (prevents misuse)
- Won't attempt embedded systems (prevents hardware concerns)
- Perfect on core business logic (Tasks 1, 4, 5: 10.0 each)
- Guardrails act as safety net

**Trade-off:** −2.9 points for policy safety.

---

## Red Flags 🚨 (Avoid in Production)

| Model | Issue | Root Cause | Severity |
| --- | --- | --- | --- |
| **Grok 4.1** | Task 2 syntax error | ` WIDTH // // 2` typo | CRITICAL |
| **GMT4.6** | Task 4 fatal bug | `_decrypt_note()` on create | CRITICAL |
| **Qwen2.5-Coder-32B** | Task 4 security | Plaintext keys + random generation | CRITICAL |
| **Llama 4** | Task 6 crashes | Protocol errors | CRITICAL |
| **Qwen3-Max** | Task 1 NameError | Undefined variable | CRITICAL |

**Exception:** Grok 4.1 acceptable IF you have mandatory human code review + comprehensive tests before deployment.

---

## Key Insights

### **1. Root Cause Types Matter**

| Type | Example | Fixability | Severity |
| --- | --- | --- | --- |
| **Typo** | Grok ` WIDTH // // 2` | Trivial | CRITICAL (prevents execution) |
| **Logic Error** | GMT4.6 calls wrong function | Simple | CRITICAL (crashes) |
| **Architectural** | Haiku threading conflict | Redesign required | SEVERE |
| **Policy Limit** | Gemini Flash refuses | Cannot fix | MEDIUM |

### **2. Consistency Beats Peaks**

- Grok 4.1 has 4 perfect scores (10.0) but ranks #7 because of one careless typo
- Mistral has no perfect scores but ranks #2 because of rock-solid consistency
- **Variance penalty formula (StdDev × 0.7) correctly identifies this**

### **3. Real-Time State Management Is the Frontier Gap**

Task 2 (Snake Game) breaks 8/13 models:

- ✅ Pass: Gemini Pro 3, Mistral, Gemini Flash, Ernie, GPT-5. 1
- ❌ Fail: Claude Sonnet, Haiku, GMT4.6, Grok, Qwen3-Max, Llama 4, Qwen2.5-Coder, DeepSeek

**Why? ** Event-driven, concurrent state management is underrepresented in training data.

### **4. Safety Filters Are Harder Ceilings Than Capability**

Gemini Flash 2.5:

- Actual capability: ~9.8 average (would rank #2)
- Policy-enforced score: 6.934 (ranks #10)
- Impact: −2.9 points from policy decisions

### **5. Specialization Wins in Focused Domains**

| Specialist | Domain | Score | Beats Gemini Pro 3 By |
| --- | --- | --- | --- |
| Claude Haiku | REST APIs | 9.95 | +0.65 |
| Ernie 4. 5 Turbo | Security | 9.86 | −0.07 |

But specialists fail outside their domain:

- Haiku fails at UI (6.11)
- DeepSeek fails at UI (7.5)
- GMT4.6 fails at security (6.0)

---

## Conclusion

### **Three Tiers of Production Readiness**

**Tier 1: Production-Ready (No Caveats)**

- ✅ Gemini Pro 3 thinking
- ✅ Mistral

**Tier 2: Production-Ready (With Caveats)**

- ✅ Claude Haiku 4.5 (only for APIs; avoid UI)
- ✅ Ernie 4.5 Turbo (excellent for security; one obfuscation gap)
- ✅ Gemini Flash 2.5 (safety filters helpful if that's your priority)

**Tier 3: Risky (Requires Code Review)**

- ⚠️ Grok 4.1 (brilliant but careless typos)

**Tier 4: Not Recommended**

- ❌ GMT4.6 (fatal security bug)
- ❌ Qwen2.5-Coder-32B (plaintext keys)
- ❌ Llama 4 (crashes on embedded)
- ❌ Qwen3-Max (basic errors)

### **Final Recommendation Framework**

| Need | Model | Score | Why |
| --- | --- | --- | --- |
| Enterprise all-domains | Gemini Pro 3 thinking | 9.632 | Consistency + 0.278 StdDev |
| Default choice | Mistral | 9. 388 | No weak rubric components |
| API startup | Claude Haiku 4. 5 | 8.009 | 9.95 on Task 5, 10× cheaper |
| Security-critical | Ernie 4.5 Turbo | 8.814 | 9.86 on Task 4 (best encryption) |
| Safety-first | Gemini Flash 2.5 | 6.934 | Policy guardrails |

---

#### Appendix A: Task Rubrics Explained

### **Evaluation Methodology**

**AI + Human Verification Process:**

| Task | AI Role | Human Role |
| --- | --- | --- |
| **Task 1** | Scored all rubric components | Verified code runs; confirmed AI observations accurate |
| **Task 2** | 60% of score (code/architecture) | 40% of score (gameplay feel) + verification |
| **Task 3** | Scored all rubric components | Verified code runs; confirmed AI observations accurate |
| **Task 4** | Scored all rubric components | Verified code runs; confirmed AI observations accurate |
| **Task 5** | Scored all rubric components | Verified code runs; confirmed AI observations accurate |
| **Task 6** | Scored all rubric components | Verified code compiles; confirmed AI observations accurate |

**Note:** For Tasks 1, 3, 4, 5, and 6, the human evaluator did not independently score but served as a **verification layer** — confirming the code executed and that AI observations were accurate. Task 2 was the exception, where human evaluation contributed 40% of the score due to the experiential nature of gameplay.

---

### **Task 1 Rubric Components**

- **Functionality:** Does it correctly count lines/words/characters?
- **Accuracy:** Edge cases (empty files, Unicode, special characters)?
- **Code Quality:** Readability, efficiency, maintainability?
- **Error Handling:** Graceful failures, proper exception handling?

*Human verification: Confirmed code runs and AI observations match actual behavior.*

---

### **Task 2 Rubric Components**

**Evaluation Split:**

- **60%** — AI/Technical evaluation (code review, execution, architecture)
- **40%** — Human evaluation (gameplay feel, player experience)

Both evaluators assessed the same components from different perspectives:

| Component | AI/Technical POV | Human/Feel POV |
| --- | --- | --- |
| **Core Gameplay** | Does the code handle collision, scoring, win/lose correctly? | Does the game *play* correctly when you try it? |
| **Controls & Responsiveness** | Does input code register keypresses? | Does it *feel* responsive and smooth? |
| **Code Quality** | Is architecture sound? Any syntax errors? | N/A (code-only) |
| **Rendering & UX** | Does rendering logic execute? | Is it clear, readable, enjoyable to look at? |

**Why Both**

- AI catches technical failures (syntax errors, broken logic)
- Human catches experiential failures (laggy feel, confusing visuals, frustrating gameplay)

---

### **Task 3 Rubric Components**

- **Obfuscation:** Variable renaming, AST manipulation, string handling?
- **Encryption:** Fernet/AES, PBKDF2 iterations, key derivation?
- **Pipeline & Execution:** End-to-end workflow, reversibility?
- **Code Quality:** Readability despite complexity?

*Human verification: Confirmed code runs and AI observations match actual behavior.*

---

### **Task 4 Rubric Components**

- **Encryption & Crypto:** Per-note encryption, PBKDF2 iterations, key management?
- **Security Best Practices:** File permissions, atomic operations, comprehensive logging?
- **Code Quality:** Readability despite complexity?
- **Functionality:** All CRUD operations verified?

*Human verification: Confirmed code runs and AI observations match actual behavior.*

---

### **Task 5 Rubric Components**

- **Auth & JWT:** Correct token generation, validation, refresh logic?
- **API Design & Endpoints:** RESTful design, all endpoints, pagination?
- **Database & Data:** Relational schema, user ownership enforcement, persistence?
- **Security & Best Practices:** Input validation, rate limiting, comprehensive tests, documentation?

*Human verification: Confirmed code runs and AI observations match actual behavior.*

---

### **Task 6 Rubric Components**

**Verification Limitation:** This test only evaluates if the models would attempt to incorporate ONFI compliant arduino code that would compile and attempted to achieve the requested funtionabilities. Evaluation was based on, compilation on the Arduino IDE and **code review against ONFI specifications**, not functional hardware verification. The point of this test was more to see if the models would be able to write something that compiles without having thousands of examples, out in the wild. Demostrating how well the model can use glue and ductape to fill the gaps in its training data.

| Component | What We Checked | What We Couldn't Check |
| --- | --- | --- |
| **Protocol Correctness** | ONFI command codes match spec; sequences logically ordered | Whether commands actually work on real NAND |
| **Implementation Quality** | Parameter page parsing structure; erase operation sequences | Whether parsing returns correct values from real hardware |
| **Code Structure** | Clean abstraction; GPIO logic makes sense | Whether GPIO actually toggles pins correctly |
| **Error Handling** | Ready/busy logic present; timeout patterns exist | Whether timeouts are correctly tuned for real hardware |

**Bottom Line:** A high score means "this code *should* work based on specification compliance" — not "this code *does* work on real hardware."

*Human verification: Confirmed code compiles and AI observations match specification requirements.*

---

## Appendix B: Variance Penalty Calculation

### **Formula**

```
Adjusted Score = Raw Average − (StdDev × 0.7)
```

### **Why 0.7 Multiplier? **

- Rewards consistency without over-penalizing variance
- Matches real-world production needs (reliability matters)
- Prevents high-variance specialists from gaming the ranking

### **Examples**

**Grok 4.1:**

```
Raw Average: 9.30
Scores: [10.0, 6.0, 10.0, 10.0, 9.8, 10.0]
StdDev: 1.619
Penalty: 1.619 × 0.7 = 1.133
Adjusted: 9.30 − 1.133 = 8.167
```

**Mistral:**

```
Raw Average: 9.58
Scores: [9.88, 9.75, 9.30, 9.56, 9.2, 9.76]
StdDev: 0.274
Penalty: 0.274 × 0.7 = 0.192
Adjusted: 9.58 − 0.192 = 9.388
```

**Impact:** Grok drops 1.133 points for variance; Mistral drops only 0.192.

---

---

## Normalized Scoring Analysis

### Raw Average with Normalized Lowest Score

To account for single catastrophic failures, each model's lowest task score is replaced with its average of the remaining 5 tasks, then the raw average is recalculated.

```
Step 1: Identify lowest scoring task
Step 2: Calculate average of remaining 5 tasks
Step 3: Replace lowest score with that average
Step 4: Recalculate raw average from all 6 values
```

| Model | T1  | T2  | T3  | T4  | T5  | T6  | Lowest Task | Replaced With | **Normalized Raw** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Gemini Pro 3 thinking** | 9. 73 | 10.0 | 10.0 | 9.93 | 9. 30* | 10.0 | T5: 9.30 | 9.93 | **9.93** |
| **Mistral** | 9.88 | 9.75 | 9.30 | 9.56 | 9.20* | 9.76 | T5: 9. 20 | 9.65 | **9.65** |
| **Ernie 4. 5 Turbo** | 9.4 | 8. 8 | 8.43* | 9.86 | 9.4 | 9. 64 | T3: 8.43 | 9.42 | **9.32** |
| **GPT-5. 1** | 9.8 | 8.5* | 9.0 | 9. 5 | 9.2 | 8.5 | T2: 8.5 | 9.20 | **9.20** |
| **DeepSeek V3** | 9.8 | 7.5* | 9.24 | 9.93 | 9.51 | 9.78 | T2: 7.5 | 9. 65 | **9.60** |
| **Claude Sonnet** | 9.85 | 6.75* | 9.05 | 9.875 | 9.675 | 9.76 | T2: 6.75 | 9.64 | **9. 56** |
| **Grok 4.1** | 10.0 | 6. 0* | 10.0 | 10.0 | 9.8 | 10. 0 | T2: 6.0 | 9.96 | **9.85** |
| **Claude Haiku 4.5** | 9. 58 | 6.11* | 9.35 | 9.43 | 9. 95 | 9.73 | T2: 6.11 | 9.61 | **9.51** |
| **GMT4.6** | 9.54 | 6. 35 | 9.71 | 6.0* | 9.64 | 9. 36 | T4: 6.0 | 8.92 | **8.84** |
| **Gemini Flash 2.5** | 10. 0 | 9.15 | 2.0* | 10.0 | 10. 0 | 2.0 | T3: 2.0 | 7.79 | **8.16** |
| **Qwen3-Max** | 6.0* | 6.4 | 9.2 | 9.43 | 7.8 | 8.4 | T1: 6.0 | 8. 25 | **8.25** |
| **Llama 4** | 9.675 | 6.2 | 7.875 | 8. 5 | 6.0 | 3.5* | T6: 3.5 | 7.65 | **7.53** |
| **Qwen2.5-Coder-32B** | 9.925 | 5.1 | 6.75 | 3.8* | 9.74 | 6.4 | T4: 3. 8 | 7.58 | **7.47** |

*Asterisk indicates the lowest score that was normalized.

**Notes:**

- GPT-5.1 has two tasks tied at 8.5 (T2 and T6); T2 used for normalization.
- Gemini Flash 2.5 has two tasks tied at 2.0 (T3 and T6); T3 used, but result is identical either way.

---

### Impact of Lowest Score Normalization

| Model | Original Raw | Normalized Raw | Change | Interpretation |
| --- | --- | --- | --- | --- |
| Gemini Pro 3 thinking | 9.83 | 9.93 | +0.10 | Already consistent; minimal boost |
| Grok 4.1 | 9.30 | 9. 85 | +0.55 | Syntax error was dragging it down |
| Mistral | 9.58 | 9.65 | +0.07 | Most consistent; barely changes |
| DeepSeek V3 | 9.30 | 9.60 | +0.30 | UI failure masked strong performance |
| Claude Sonnet | 9.16 | 9.56 | +0. 40 | Task 2 failure was an outlier |
| Claude Haiku 4.5 | 9.02 | 9.51 | +0. 49 | Threading issue hid API excellence |
| Ernie 4.5 Turbo | 9.19 | 9.32 | +0.13 | Relatively consistent already |
| GPT-5.1 | 9.08 | 9.20 | +0. 12 | No catastrophic failures |
| GMT4.6 | 8.43 | 8.84 | +0.41 | Fatal bug was an outlier |
| Qwen3-Max | 7.87 | 8.25 | +0.38 | NameError was an outlier |
| Gemini Flash 2.5 | 7.19 | 8.16 | +0.97 | One refusal normalized (still has one) |
| Llama 4 | 6.96 | 7.53 | +0. 57 | Protocol crash was catastrophic |
| Qwen2. 5-Coder-32B | 6.95 | 7.47 | +0. 52 | Security nightmare was an outlier |

---

### Final Normalized Score

Combines all three scoring methodologies using a simple average:

```
Final Normalized Score = (Equal Weighted Adj.  + Backend Weighted Adj. + Normalized Raw) / 3
```

| Rank | Model | Equal Adj. | Backend Adj. | Normalized Raw | **Final Score** |
| --- | --- | --- | --- | --- | --- |
| **1** | **Gemini Pro 3 thinking** | 9.632 | 9.515 | 9.93 | **9.692** |
| **2** | **Mistral** | 9.388 | 9.278 | 9.65 | **9.439** |
| **3** | **Grok 4.1** | 8.167 | 8.387 | 9.85 | **8.801** |
| **4** | **DeepSeek V3** | 8.661 | 8.841 | 9.60 | **9.034** |
| **5** | **Ernie 4.5 Turbo** | 8.814 | 8.974 | 9.32 | **9.036** |
| **6** | **Claude Sonnet** | 8. 307 | 8.537 | 9.56 | **8.801** |
| **7** | **Claude Haiku 4. 5** | 8.009 | 8.309 | 9.51 | **8.609** |
| **8** | **GPT-5.1** | 8.711 | 8.811 | 9.20 | **8.907** |
| **9** | **GMT4.6** | 7.200 | 7.000 | 8.84 | **7.680** |
| **10** | **Gemini Flash 2.5** | 6.934 | 7.582 | 8.16 | **7.559** |
| **11** | **Qwen3-Max** | 6.873 | 7.363 | 8.25 | **7.495** |
| **12** | **Llama 4** | 5.425 | 5.625 | 7.53 | **6.193** |
| **13** | **Qwen2.5-Coder-32B** | 5.226 | 5.166 | 7.47 | **5.954** |

---

### Final Normalized Rankings (Sorted by Score)

| Rank | Model | Final Score | vs. Equal Rank | Change |
| --- | --- | --- | --- | --- |
| **1** | Gemini Pro 3 thinking | **9.692** | #1  | —   |
| **2** | Mistral | **9.439** | #2  | —   |
| **3** | Ernie 4.5 Turbo | **9.036** | #3  | —   |
| **4** | DeepSeek V3 | **9.034** | #5  | ↑1  |
| **5** | GPT-5.1 | **8.907** | #4  | ↓1  |
| **6** | Grok 4.1 | **8.801** | #7  | ↑1  |
| **7** | Claude Sonnet | **8.801** | #6  | ↓1  |
| **8** | Claude Haiku 4.5 | **8. 609** | #8  | —   |
| **9** | GMT4.6 | **7. 680** | #9  | —   |
| **10** | Gemini Flash 2.5 | **7.559** | #10 | —   |
| **11** | Qwen3-Max | **7.495** | #11 | —   |
| **12** | Llama 4 | **6.193** | #12 | —   |
| **13** | Qwen2.5-Coder-32B | **5.954** | #13 | —   |

**Note:** Grok 4.1 and Claude Sonnet are tied at 8.801. Grok ranked higher due to superior Normalized Raw (9.85 vs 9. 56).

---

### Key Insights from Final Normalized Scoring

#### **1. Top 3 Remains Rock Solid**

Gemini Pro 3 thinking, Mistral, and Ernie 4.5 Turbo maintain their positions across all methodologies — indicating genuinely robust, reliable performance regardless of how scores are calculated.

#### **2. High-Variance Models Recover**

| Model | Equal Rank | Final Rank | Improvement | Why |
| --- | --- | --- | --- | --- |
| Grok 4.1 | #7  | #6  | ↑1  | Normalized raw (9.85) rewards 4 perfect scores |
| DeepSeek V3 | #5  | #4  | ↑1  | Strong performance outside Task 2 recognized |

#### **3. Consistency-Advantaged Models Stabilize**

| Model | Equal Rank | Final Rank | Change | Why |
| --- | --- | --- | --- | --- |
| Ernie 4.5 Turbo | #3  | #3  | —   | Consistency still valued in weighted scores |
| GPT-5. 1 | #4  | #5  | ↓1  | Others caught up when outliers normalized |

#### **4. The "True Capability" Middle Tier**

Models ranked #4–#8 are separated by only **0.43 points** (9.034 to 8.609), indicating functional equivalence for most use cases:

| Model | Final Score | Primary Strength |
| --- | --- | --- |
| DeepSeek V3 | 9.034 | Protocols & crypto |
| GPT-5.1 | 8.907 | General-purpose |
| Grok 4.1 | 8.801 | Peak performance (when it works) |
| Claude Sonnet | 8.801 | Text & fundamentals |
| Claude Haiku 4.5 | 8.609 | REST APIs & speed |

#### **5. Safety Filters Still Hurt**

Gemini Flash 2.5 improves from 7.19 → 8.16 with normalization but still ranks #10 because it has **two** low scores (Tasks 3 and 6), and only one can be normalized.

---

### Updated Recommendation Framework (Final Normalized)

| Need | Model | Final Score | Key Advantage |
| --- | --- | --- | --- |
| Enterprise all-domains | Gemini Pro 3 thinking | 9.692 | Highest overall, most consistent |
| Default choice | Mistral | 9.439 | No weak spots, reliable |
| Security-critical | Ernie 4.5 Turbo | 9. 036 | Best Task 4 (9.86) |
| Backend/API focus | Claude Haiku 4.5 | 8.609 | Best Task 5 (9.95), 10× cheaper |
| Peak performance | Grok 4.1 | 8.801 | Highest peaks (requires code review) |
| Budget reasoning | DeepSeek V3 | 9.034 | Strong protocols, good value |
| Safety-first | Gemini Flash 2.5 | 7.559 | Policy guardrails |

---

## Evaluation Process Overview

### Two-Layer Evaluation System

Each task was evaluated using a combination of AI analysis and human verification:

```
┌─────────────────────────────────────────────────────────────┐
│                    MODEL GENERATES CODE                     │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                 AI EVALUATOR (Claude)                       │
│  • Analyzes code structure                                  │
│  • Checks rubric compliance                                 │
│  • Scores each component (Functionality, Accuracy, etc.)    │
│  • Identifies red flags                                     │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                 HUMAN VERIFICATION                          │
│  • Confirms code actually runs                              │
│  • Validates AI observations are accurate                   │
│  • Task 2 only: Scores gameplay feel (40% of Task 2 score)  │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    FINAL SCORE                              │
└─────────────────────────────────────────────────────────────┘
```

### Evaluation Timeline

- **Models tested:** 13 total
- **Tasks per model:** 6
- **Total evaluations:** 78 (13 × 6)
- **Data points:** 312 (78 evaluations × 4 rubric components average)

---

## AI vs Human Evaluation Roles

### Summary Table

| Task | AI Evaluator Role | Human Role | Human Contribution to Score |
| --- | --- | --- | --- |
| **Task 1** | Scored all components | Verified code runs; confirmed AI observations | 0% (verification only) |
| **Task 2** | 60% of score (code/architecture) | 40% of score (gameplay feel) + verification | **40%** |
| **Task 3** | Scored all components | Verified code runs; confirmed AI observations | 0% (verification only) |
| **Task 4** | Scored all components | Verified code runs; confirmed AI observations | 0% (verification only) |
| **Task 5** | Scored all components | Verified code runs; confirmed AI observations | 0% (verification only) |
| **Task 6** | Scored all components | Verified code compiles; confirmed AI observations | 0% (verification only) |

### Why Task 2 Has Human Scoring (40%)

Games are inherently experiential. Code that "works" technically may still:

- Feel laggy or unresponsive
- Have confusing or unclear visuals
- Be frustrating to play despite correct logic

**What AI evaluates (60%):**

- Does collision detection code work?
- Is the architecture sound?
- Are there syntax errors?
- Is the game loop structured correctly?

**What Human evaluates (40%):**

- Does the game *feel* responsive?
- Is it *enjoyable* to play?
- Are the visuals *clear* and *intuitive*?
- Would a real user be satisfied?

### Human Verification Process (Tasks 1, 3, 4, 5)

For non-game tasks, the human evaluator:

1. **Attempted to run the code** — Did it execute without errors?
2. **Tested basic functionality** — Did the core features work?
3. **Cross-referenced AI observations** — Were the AI's claims accurate?
4. **Did NOT independently score** — Deferred to AI evaluation for final scores

If discrepancies were found between AI observations and actual behavior, scores were adjusted accordingly.

---

## Task-Specific Evaluation Details

### Task 1: Word Counter & Text Analyzer

**What AI Evaluated:**

- Functionality (40%): All required metrics calculated?
- Accuracy (35%): Calculations correct against manual verification?
- Code Quality (15%): Clean structure, good naming, comments?
- Error Handling (10%): Graceful failures, meaningful messages?

**What Human Verified:**

- Code runs without errors
- Output matches expected format
- Test files produce results matching the prompt/AI evaluator

**Verification Method:** Ran against sample text files with known word counts.

---

### Task 2: Snake Game CLI

**What AI Evaluated (60%):**

- Core Gameplay (35%): Collision detection, scoring, win/lose conditions
- Code Quality (20%): Architecture, function separation, comments
- Part of Controls (5%): Input handling code structure

**What Human Evaluated (40%):**

- Controls & Responsiveness (20%): Does it *feel* responsive?
- Rendering & UX (20%): Is it visually clear and enjoyable?

**Verification Method:** Actually played each game for 2-5 minutes, testing:

- Direction changes (all 4 directions)
- Collision with walls
- Collision with self
- Food eating and snake growth
- Score incrementing
- Game over state

**Why Human Evaluation Was Critical:**
Several models produced code that was technically correct but unplayable:

- **Claude Haiku:** Threading + blocking I/O conflict made controls unresponsive
- **Claude Sonnet:** Input handling broken despite correct game logic
- **Grok 4. 1:** Syntax error (`// //`) prevented execution entirely
- **GMT4. 6:** Terminal I/O architecture failure

AI could identify these issues in code review, but human playtesting confirmed severity.

---

### Task 3: Code Obfuscation & Encryption

**What AI Evaluated:**

- Obfuscation Quality (30%): Variables/functions renamed? Comments removed?
- Encryption Implementation (30%): Real encryption? PBKDF2? Unique salt?
- Pipeline & Execution (25%): Full workflow works end-to-end?
- Code Quality (15%): Clean separation of concerns?

**What Human Verified:**

- Obfuscation tool runs on sample Python file
- Encrypted output is generated
- Wrapper executes with correct password
- Wrong password fails gracefully

**Key Evaluation Criteria for Encryption:**

| Criterion | Acceptable | Not Acceptable |
| --- | --- | --- |
| Encryption library | Fernet, AES, ChaCha20 | Base64, XOR, ROT13 |
| Key derivation | PBKDF2, bcrypt, Argon2, scrypt | Hardcoded keys, no derivation |
| Salt | Unique per encryption, randomly generated | Same salt, no salt |
| Iterations (PBKDF2) | 100k+ acceptable, 600k+ ideal | <10k |

---

### Task 4: Secure Note-Taking Application

**What AI Evaluated:**

- Encryption & Crypto (30%): Real encryption, proper key derivation, unique salt
- Security Best Practices (30%): Input validation, no hardcoded secrets, logging
- Code Quality & Structure (25%): Separation of concerns, documentation
- Functionality (15%): All CRUD operations work

**What Human Verified:**

- Application runs and presents menu
- Can create, view, update, delete notes
- Encryption/decryption flow works
- Wrong password handled gracefully

**PBKDF2 Iteration Standards Used:**

| Iteration Count | Rating | Score Impact |
| --- | --- | --- |
| 600k+ | Industry standard | Full marks |
| 100k-600k | Acceptable | Minor deduction |
| 10k-100k | Suboptimal | Moderate deduction |
| <10k | Weak | Significant deduction |

---

### Task 5: RESTful API with JWT Authentication

**What AI Evaluated:**

- Authentication & JWT (30%): Token generation, validation, expiration
- API Design & Endpoints (25%): Correct HTTP methods, status codes, structure
- Database & Data (25%): Schema design, relationships, password hashing
- Security & Best Practices (20%): Env vars, validation, logging

**What Human Verified:**

- Code structure is complete
- Database models defined
- JWT flow is logical
- Endpoints are properly structured

**Note:** API was not live-tested (no server environment). Evaluation based on code review.

---

### Task 6: Arduino NAND Flash Controller

**What AI Evaluated:**

- Protocol Correctness (35%): ONFI command codes correct? Sequences logical?
- Implementation Quality (35%): GPIO setup, data bus handling, control signals
- Code Structure (20%): Function organization, comments, clarity
- Error Handling (10%): R/B signal handling, timeouts, error detection

**What Human Verified:**

- Code compiles (syntax check)
- ONFI command codes match specification
- Logical flow makes sense

**Critical Limitation:**

> **No physical hardware was available for testing.**
> 
> Evaluation assessed whether code *would plausibly function* on real hardware based on:
> 
> - Correct ONFI command codes (0x90 READ ID, 0x00/0x30 READ PAGE, 0x60 ERASE)
> - Logical timing sequences
> - Proper state machine flow
> - Presence of error handling
> 
> **Actual hardware behavior could not be verified.**

**ONFI Command Codes Used for Verification:**

| Command | Code | What It Does |
| --- | --- | --- |
| READ ID | 0x90 | Read manufacturer/device ID |
| READ PAGE | 0x00 | Start read operation |
| READ PAGE (Confirm) | 0x30 | Confirm and execute read |
| BLOCK ERASE | 0x60 | Erase a block |
| ERASE (Confirm) | 0xD0 | Confirm erase operation |
| READ STATUS | 0x70 | Get device status |

---

## Scoring Rubrics (Full Detail)

### Component Weight Distribution by Task

| Task | Component 1 | Component 2 | Component 3 | Component 4 |
| --- | --- | --- | --- | --- |
| **Task 1** | Functionality (40%) | Accuracy (35%) | Code Quality (15%) | Error Handling (10%) |
| **Task 2** | Core Gameplay (35%) | Controls (25%) | Code Quality (20%) | Rendering/UX (20%) |
| **Task 3** | Obfuscation (30%) | Encryption (30%) | Pipeline (25%) | Code Quality (15%) |
| **Task 4** | Encryption (30%) | Best Practices (30%) | Code Quality (25%) | Functionality (15%) |
| **Task 5** | Auth/JWT (30%) | API Design (25%) | Database (25%) | Security (20%) |
| **Task 6** | Protocol (35%) | Implementation (35%) | Code Structure (20%) | Error Handling (10%) |

### Score Calculation Formula

For each task:

```
Task Score = Σ (Component Score × Component Weight)
```

For global average:

```
Global Average = (Task1 + Task2 + Task3 + Task4 + Task5 + Task6) / 6
```

For consistency penalty:

```
StdDev = Standard Deviation of [Task1, Task2, Task3, Task4, Task5, Task6]
Penalty = StdDev × 0.7
Adjusted Score = Global Average - Penalty
```

---

## Weighting Rationale

### Equal Weighting (Primary Ranking)

Each task = 16.67% of total score.

**Use case:** Enterprise environments where all domains matter equally.

### Backend-Prioritized Weighting (Alternative Ranking)

| Task | Weight | Rationale |
| --- | --- | --- |
| **Task 1** (Word Counter) | 10% | Very simple; doesn't differentiate capable models (12/13 scored 9. 4+) |
| **Task 2** (Snake Game) | 10% | Limited applicability; real-time CLI games are niche, don't apply to most work environments |
| **Task 3** (Obfuscation) | 20% | Mixed relevance; encryption/key derivation are enterprise-relevant, code obfuscation itself is niche (DRM, proprietary protection) |
| **Task 4** (Secure Notes) | 25% | Enterprise security standard; per-note encryption, PBKDF2, file permissions, audit logging are common requirements |
| **Task 5** (REST API) | 30% | Core enterprise skill; JWT, databases, API design represent the most common enterprise development work |
| **Task 6** (Arduino NAND) | 5%  | Verification limitations; complex but unverifiable without hardware — code might run but correctness unknown |

### Weighting Principle

```
Weight = Real-World Applicability × Verification Confidence
```

- **High applicability + High verification confidence** = High weight (Task 5: 30%)
- **High applicability + High verification confidence** = High weight (Task 4: 25%)
- **Mixed applicability** = Moderate weight (Task 3: 20%)
- **Low applicability** = Low weight (Task 2: 10%)
- **High complexity + Low verification confidence** = Low weight (Task 6: 5%)

---

## Known Limitations

### What This Benchmark Can Evaluate

✅ Code correctness (syntax, logic, structure)
✅ Algorithm implementation (calculations, data structures)
✅ Security practices (encryption, hashing, validation)
✅ Code organization (functions, comments, naming)
✅ Error handling (graceful failures, meaningful messages)
✅ Gameplay feel (Task 2 only, via human testing)
✅ Protocol compliance (Task 6, against ONFI specification)

### What This Benchmark Cannot Evaluate

❌ **Performance:** No measurement of execution speed, memory usage, or scalability
❌ **Hardware interaction:** Task 6 code not tested on real Arduino/NAND

### Task-Specific Limitations

| Task | Limitation |
| --- | --- |
| Task 1 | Not tested with very large files (>1MB) |
| Task 2 | Not tested on non-standard terminals |
| Task 3 | Cannot verify resistance to determined reverse engineering |
| Task 4 | Cannot verify actual file permissions on user's system |
| Task 5 | No live HTTP request testing |
| Task 6 | **No hardware verification** — plausibility-based scoring only |

---

## Red Flags & Score Ceilings

Pre-defined automatic score penalties were applied for critical failures:

### Task 1: Word Counter

| Red Flag | Maximum Score |
| --- | --- |
| Word count is wrong | 6/10 |
| Unique word count incorrect | 6/10 |
| Average word length calculation wrong | 6/10 |
| Longest/shortest word incorrect | 7/10 |
| Crashes on missing file | 5/10 |
| No error handling at all | 6/10 |
| Output unformatted or hard to read | 6/10 |

### Task 2: Snake Game

| Red Flag | Maximum Score |
| --- | --- |
| Snake passes through itself | 8/10 |
| Food doesn't spawn or disappear inconsistently | 7/10 |
| Score doesn't increment | 8/10 |
| Significant input lag | 7/10 |
| Game doesn't run without external GUI libraries | 6/10 |
| No collision detection at all | 7/10 |
| Crashes during gameplay | 6/10 |

### Task 3: Obfuscation & Encryption

| Red Flag | Maximum Score |
| --- | --- |
| Base64 encoding as "encryption" | **3/10** |
| Hardcoded encryption keys | 5/10 |
| No password-based key derivation | 6/10 |
| Obfuscated code doesn't run | 4/10 |
| No actual encryption, just obfuscation | 6/10 |
| Wrapper doesn't execute decrypted code | 5/10 |
| Same salt for all encryptions | 7/10 |
| Refuses task due to safety concerns | **2/10** |

### Task 4: Secure Notes

| Red Flag | Maximum Score |
| --- | --- |
| Hardcoded passwords/keys anywhere | 6/10 |
| Plaintext password logging | 6/10 |
| No password-based key derivation | 5/10 |
| Same salt for all notes | 7/10 |
| Base64 as encryption | **3/10** |
| No validation (accepts any input) | 6/10 |
| No error handling (crashes on wrong password) | 6/10 |

### Task 5: REST API

| Red Flag | Maximum Score |
| --- | --- |
| Hardcoded JWT_SECRET in code | 6/10 |
| Plaintext passwords (no hashing) | **5/10** |
| MD5 for password hashing | 5/10 |
| No token validation on protected routes | 6/10 |
| No input validation | 6/10 |
| Same secret/key for all users | 7/10 |
| Accepts any token or skips signature check | 4/10 |
| No database or all data in memory | 5/10 |

### Task 6: Arduino NAND

| Red Flag | Score Impact |
| --- | --- |
| Wrong ONFI command codes | Significant deduction |
| No R/B signal handling | Moderate deduction |
| Pseudocode instead of real code | Major deduction |
| No error handling or timeouts | Moderate deduction |
| Refuses task | Maximum 2/10 |

---

## Difficulty Calibration

### Original Estimates vs Actual Results

| Task | Estimated Difficulty | Actual Difficulty | Pass Rate (≥8. 0) | Notes |
| --- | --- | --- | --- | --- |
| Task 1 | 3.5/10 (Easy) | ~3/10 | 12/13 (92%) | As expected; only Qwen3-Max failed |
| Task 2 | 4.5/10 (Easy-Medium) | **~7/10** | 5/13 (38%) | **Underestimated**; threading/terminal I/O is frontier weakness |
| Task 3 | 5.5/10 (Medium) | ~5. 5/10 | 9/13 (69%) | Matched; one refusal (Gemini Flash) |
| Task 4 | 5.5/10 (Medium) | ~6/10 | 10/13 (77%) | Slightly harder; 2 catastrophic failures |
| Task 5 | 7.5/10 (Hard) | **~5/10** | 11/13 (85%) | **Overestimated**; most models scored 9+ |
| Task 6 | 9/10 (Very Hard) | 10/10 | 9/13 (69%) | Verification limited; |

### Key Insight: Task 2 Is the Differentiator

Task 2 (Snake Game) had the **lowest pass rate** despite being rated "Easy-Medium."

**Why models struggled:**

- Real-time terminal I/O patterns are rare in training data
- Threading + blocking I/O is a common architectural mistake
- Non-blocking input handling requires specific knowledge (`select`, `poll`, `kbhit`)
- Most async patterns in training data are web/API focused, not terminal-based

**Models that passed Task 2 (≥8.0):**

- Gemini Pro 3 thinking (10. 0)
- Mistral (9.75)
- Gemini Flash 2. 5 (9.15)
- Ernie 4.5 Turbo (8.8)
- GPT-5. 1 (8.5)

**Models that failed Task 2 (<7.0):**

- Claude Sonnet (6.75) — Broken input handling
- Qwen3-Max (6.4) — Malformed rendering
- GMT4.6 (6.35) — Terminal I/O architecture failure
- Llama 4 (6.2) — Missing keyboard library
- Claude Haiku 4. 5 (6.11) — Threading + blocking I/O conflict
- Grok 4.1 (6.0) — Syntax error (`// //`)
- Qwen2.5-Coder-32B (5.1) — Syntax error

---

## Data Integrity Notes

### Single Evaluation Run

Each model was tested **once** per task. No cherry-picking of results.

### Prompt Consistency

All 13 models received identical prompts for each task (after Task 3 refinement).

### Evaluator Consistency

- **AI Evaluator:** Claude (same model for all evaluations)
- **Human Verifier:** Single human evaluator for consistency

### Score Calculation Verification

All calculations were verified:

- Global averages computed from raw task scores
- Standard deviations calculated using population formula
- Consistency penalties applied uniformly (StdDev × 0.7)
- Weighted scores computed with documented weights

### Raw Data Availability

Complete per-component scores are available in the supplementary data file (`ai_model_benchmark_scores2.md`).

---

## Appendix: Models Tested

| Model | Provider | Type | Notes |
| --- | --- | --- | --- |
| Gemini Pro 3 thinking | Google | Reasoning | #1 ranked |
| Mistral | Mistral AI | General | #2 ranked |
| Ernie 4.5 Turbo | Baidu | General | #3 ranked |
| GPT-5. 1 | OpenAI | General |     |
| DeepSeek V3 | DeepSeek | Reasoning |     |
| Claude Sonnet | Anthropic | General |     |
| Grok 4.1 | xAI | General |     |
| Claude Haiku 4.5 | Anthropic | Fast |     |
| GMT4.6 | Unknown | General |     |
| Gemini Flash 2.5 | Google | Fast | Safety filter refusals |
| Qwen3-Max | Alibaba | General |     |
| Llama 4 | Meta | Open Source |     |
| Qwen2.5-Coder-32B | Alibaba | Coding | Locally hosted (Q3_K_M) |

---
