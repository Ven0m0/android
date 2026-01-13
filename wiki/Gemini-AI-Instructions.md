# Gemini AI Instructions

Configuration and best practices for using Gemini AI in development and analysis workflows.

## 🎯 Purpose

**Purpose:** Development & analysis instructions for Gemini AI
**Model:** gemini-* (2.0-flash-exp, exp-1206, 2.5)
**Tone:** Blunt, precise. `Result ∴ Cause`. Lists ≤7

---

## 🤖 Model & Capabilities

### Current Model

**Model:** `gemini-2.5`

### Key Features

| Feature | Status | Description |
|---------|--------|-------------|
| **Vision** | ✅ Yes | Multimodal image analysis |
| **Code Analysis** | ✅ Yes | Multi-language code understanding |
| **Web Grounding** | ✅ Yes | Real-time web search integration |
| **Long Context** | ✅ Yes | Extended context window |
| **JSON Mode** | ✅ Yes | Structured output generation |

---

## 🛡️ Safety & Accuracy

### Core Principles

1. **No Hallucinations** - Cite claims with sources
2. **Verify Versions** - Check current versions before recommendations
3. **Web Search** - Use only for freshness (latest APIs/docs)
4. **Factual Grounding** - Ground responses in verifiable information

### Verification Checklist

```plaintext
☐ Claim has source citation
☐ Version numbers are current
☐ API recommendations are up-to-date
☐ Dependencies are compatible
☐ Code examples have been tested
```

---

## 📝 Prompts & Patterns

### System-Level Thinking

**Focus:** High-level design & tradeoff analysis

```plaintext
Analyze system architecture considering:
- Scalability constraints
- Performance tradeoffs
- Maintainability impacts
- Cost implications
```

### Task Execution

**Priority:** Analyze > Implement

1. **Analyze** - Understand before acting
2. **Subtract** - Remove before adding
3. **User>Rules** - Follow user directives

### Output Preferences

#### Structured Output

**Preferred:**
```json
{
  "recommendation": "string",
  "rationale": "string",
  "tradeoffs": ["list"],
  "alternatives": ["list"]
}
```

**With Human Context:**
```plaintext
Recommendation: Use async/await
Rationale: Better readability with minimal performance cost
Tradeoffs: +3% CPU, -40% complexity
Alternatives: Callbacks, Promises
```

### Constraints

- **Lists** - Maximum 7 items
- **Changes** - Edit > Create (minimize Δ)
- **Brevity** - Concise > Verbose

---

## 🧠 Reasoning Framework

### Decision Template

```plaintext
Approach A: [Description]
  ✅ Pro: [Benefit]
  ❌ Con: [Drawback]
  ⚡ Perf: [Performance impact]

Approach B: [Description]
  ✅ Pro: [Benefit]
  ❌ Con: [Drawback]
  ⚡ Perf: [Performance impact]

⇒ Recommendation: [Choice]
∵ Rationale: [Reason why]
```

### Symbols

| Symbol | Meaning |
|--------|---------|
| ✅ | Pro / Advantage |
| ❌ | Con / Disadvantage |
| ⚡ | Performance Impact |
| ⇒ | Recommendation |
| ∵ | Rationale / Because |
| Δ | Delta / Change |

---

## ⚡ Performance Analysis

### Frontend Optimization

**Key Areas:**

1. **Min DOM Δ** - Minimize DOM changes
   - Use virtual DOM diffing
   - Batch updates
   - Avoid layout thrashing

2. **Lazy Load** - Load on demand
   - Code splitting
   - Route-based splitting
   - Component lazy loading

3. **Stable Keys** - React reconciliation
   - Use stable, unique keys
   - Avoid index as key
   - Preserve component state

**Example Analysis:**

```plaintext
Current: Render entire list (1000 items)
⚡ Perf: 450ms render time

Optimized: Virtual scrolling
⚡ Perf: 12ms render time (-97%)
✅ Pro: Scales to any list size
❌ Con: +15KB bundle size
⇒ Rec: Virtual scroll ∵ 97% perf gain >> 15KB
```

### Backend Optimization

**Key Patterns:**

1. **Async I/O** - Non-blocking operations
2. **Pools** - Connection/thread pooling
3. **Cache** - Multi-level caching
4. **Avoid N+1** - Query optimization

**Database Example:**

```plaintext
❌ N+1 Query Pattern:
for user in users:
    posts = db.query(user_id)  # N queries

✅ Single Query:
posts = db.query_all_with_users()  # 1 query
⚡ Perf: 1000ms → 50ms (-95%)
```

### Infrastructure Considerations

**Focus Areas:**

- **Latency Budgets** - Set and measure targets
- **Circuit Breakers** - Fault tolerance
- **Cost Scaling** - Resource optimization

---

## 🖼️ Multimodal Analysis

### Vision Capabilities

**Extract from Images:**
- Code structure and flows
- Architecture diagrams
- UI/UX patterns
- Data flow diagrams

**Detect Anti-Patterns:**
- Circular dependencies
- Tight coupling
- Missing error handling
- Security vulnerabilities

### Analysis Template

```plaintext
Image: [Description]

Observations:
1. [Pattern/Structure identified]
2. [Potential issues]
3. [Optimization opportunities]

Recommendations:
⇒ [Actionable advice]
∵ [Reasoning]
```

---

## 🎯 Execution Framework

### 4-Step Process

#### 1. Analyze
```plaintext
State:       [Current situation]
Constraints: [Limitations]
Goals:       [Objectives]
```

#### 2. Design
```plaintext
Option A: [Approach]
  ✅ [Pros]
  ❌ [Cons]
  ⚡ [Performance]

Option B: [Approach]
  ✅ [Pros]
  ❌ [Cons]
  ⚡ [Performance]
```

#### 3. Validate
```plaintext
Risks:       [Potential problems]
Edge Cases:  [Boundary conditions]
Bottlenecks: [Performance issues]
```

#### 4. Recommend
```plaintext
⇒ [Clear recommendation]
∵ [Rationale with tradeoffs]
```

---

## 🔧 Model Quirks

### Best Practices

#### Structured Output (JSON Mode)

**Use for:**
- API responses
- Configuration generation
- Data transformation
- Structured analysis

```json
{
  "analysis": {
    "approach": "async/await",
    "pros": ["readable", "debuggable"],
    "cons": ["slightly slower"],
    "performance_impact": "-3%"
  }
}
```

#### Grounding for Factual Queries

**Enable grounding when:**
- Checking library versions
- Verifying API availability
- Confirming documentation
- Researching best practices

#### Temperature Settings

**Recommended Values:**

| Task Type | Temperature | Reasoning |
|-----------|-------------|-----------|
| Code Generation | 0.0-0.2 | Deterministic, accurate |
| Analysis | 0.2-0.4 | Balanced, focused |
| Creative Design | 0.6-0.8 | Exploratory |
| Brainstorming | 0.8-1.0 | Diverse ideas |

---

## 📚 Examples

### Example 1: Async I/O Comparison

**Task:** Compare async I/O approaches for Node.js API
**Input:** High-throughput read-heavy API (100k req/s)

**Output:**

```plaintext
Approach A (callbacks):
  ❌ Callback hell - difficult maintenance
  ⚡ Fastest - 0ms overhead

Approach B (async/await):
  ✅ Readable - easier maintenance
  ✅ Debuggable - stack traces work
  ⚡ -3% perf - negligible at scale

⇒ Rec: B (async/await)
∵ Maintainability > 3% perf delta at 100k req/s
```

**Result:** Clear tradeoff analysis with rationale.

### Example 2: Database Query Optimization

**Task:** Optimize slow user profile endpoint
**Input:** Loading 1000 users takes 5 seconds

**Analysis:**

```plaintext
Current State:
- Query: SELECT * FROM users (1000 rows)
- Includes: 20 unnecessary columns
- Missing: Index on email field
- N+1: Separate queries for roles

Optimizations:
1. SELECT only needed columns
   ⚡ Perf: 5000ms → 3000ms (-40%)

2. Add index on email
   ⚡ Perf: 3000ms → 400ms (-87%)

3. JOIN roles in single query
   ⚡ Perf: 400ms → 50ms (-88%)

⇒ Total: 5000ms → 50ms (-99%)
∵ Combined optimizations compound
```

### Example 3: Frontend Bundle Analysis

**Task:** Reduce initial bundle size
**Input:** 850KB initial bundle, 3.2s load time

**Recommendations:**

```plaintext
Current Bundle: 850KB, 3.2s load

Optimizations:
A. Code Splitting:
   ✅ Lazy load routes
   ⚡ -400KB initial (-47%)
   ❌ +1 network request per route

B. Tree Shaking:
   ✅ Remove unused exports
   ⚡ -120KB (-14%)
   ✅ No runtime cost

C. Compression:
   ✅ Brotli compression
   ⚡ -180KB (-21%)
   ❌ Requires server config

⇒ Rec: All three
∵ Combined: 850KB → 150KB (-82%)
   Load: 3.2s → 0.6s (-81%)
```

---

## 🔍 Troubleshooting

### Common Issues

#### Hallucination Prevention

```plaintext
❌ Wrong:
"React 19 has built-in signals"

✅ Right:
"As of React 18.2 (latest stable), signals are not built-in.
Preact Signals can be used with React."
[Source: React docs, Dec 2024]
```

#### Version Verification

```plaintext
Before recommending:
1. Check current stable version
2. Verify feature availability
3. Note any experimental status
4. Cite documentation source
```

---

## 🔗 Model Comparison

### Gemini vs Other Models

| Feature | Gemini 2.5 | Claude 3.5 | GPT-4 |
|---------|-----------|-----------|-------|
| Vision | ✅ Native | ✅ Native | ✅ Native |
| Grounding | ✅ Built-in | ❌ No | ❌ No |
| JSON Mode | ✅ Yes | ✅ Yes | ✅ Yes |
| Context | 1M tokens | 200K tokens | 128K tokens |
| Code Exec | ✅ Yes | ❌ No | ❌ No |

### When to Use Gemini

**Best for:**
- Fact-checking with grounding
- Large context analysis
- Vision-based code review
- Real-time information needs

**Consider alternatives for:**
- Highly creative writing
- Specialized domain tasks
- Cost-sensitive applications

---

## 🔙 Navigation

- [← Back to Home](Home)
- [← Claude AI Instructions](Claude-AI-Instructions)

---

*Configuration version: 2.5 | Last updated: 2026-01*
