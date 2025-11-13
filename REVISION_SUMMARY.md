# MixTex Paper Revision Summary

## Overview
This document summarizes all major changes made to the paper based on our systematic review and planning process.

---

## 🎯 Core Repositioning

### Before:
- Primary focus: Reducing "bias" in LaTeX OCR
- Unclear what "bias" means (mixed definitions)
- Secondary mention of data efficiency

### After:
- **Primary focus**: Cost-effective synthetic data generation
- **Secondary benefit**: Reduced contextual over-reliance (as side effect)
- Clear value proposition: 2 hours + 20GB vs weeks of manual curation

---

## 📝 Section-by-Section Changes

### 1. Title

**Old:**
```
MixTex: Unambiguous Recognition Should Not Rely Solely on Real Data
```

**New:**
```
MixTex: Efficient LaTeX OCR with Mixed Real and Synthetic Training Data
```

**Rationale:**
- "Unambiguous Recognition" was confusing and technically inaccurate
- New title clearly states the method (mixed real/synthetic data) and benefit (efficient)
- No over-claiming

---

### 2. Abstract

**Major Changes:**
- ✅ **Added**: Concrete data generation cost (2 hours, consumer hardware, zero manual effort)
- ✅ **Added**: Specific dataset composition (40M real + 80M synthetic = 120M tokens)
- ✅ **Added**: Test set description (400 samples: 200 CN + 200 EN, including 100 challenge cases)
- ✅ **Clarified**: Semantic incoherence as intentional design choice
- ✅ **Repositioned**: Contextual over-reliance reduction as "additional benefit" not primary goal
- ❌ **Removed**: "multilingual" claim (only CN/EN validated)
- ❌ **Removed**: Music performance error detection (no experimental support)
- ❌ **Removed**: Vague "broader applicability" claims

**Key Narrative Shift:**
```
Before: "We solve the bias problem in LaTeX OCR"
After: "We dramatically reduce data collection costs while maintaining accuracy"
```

---

### 3. Introduction

**Major Changes:**

**Added Paragraphs:**
1. **Problem statement**: Real document collection is costly and English-biased
2. **Contextual over-reliance explanation**: Concrete example of e-t vs e^{-t}
3. **Key insight**: Semantic incoherence forces visual reliance
4. **Three value propositions**:
   - Cost efficiency (quantified)
   - Language extensibility (with specific examples)
   - Reduced contextual over-reliance

**Removed:**
- Confusing/contradictory "bias" definitions
- Claims about "unambiguous recognition"
- Education applications (student error correction)
- Music applications

**Strengthened:**
- Comparison to Nougat's data collection process
- Concrete cost/time numbers
- Clearer problem motivation

---

### 4. Related Work

**Changes:**
- ✅ Added subsection structure for clarity
- ✅ More accurate characterization of existing work
- ✅ Added context about synthetic data for vision-language tasks
- ✅ Added discussion of contextual bias and hallucination
- ⚠️ **TODO**: Need to add actual citations for:
  - Hallucination in vision-language models
  - Synthetic data effectiveness
  - InftyReader and other formula OCR systems

**Note:** The paper currently has `\cite{CITATION_NEEDED}` placeholders. You'll need to find and add appropriate references.

---

### 5. Dataset → Synthetic Data Generation (Completely Rewritten)

**Old Section Issues:**
- Called "Dataset" but described three different training configurations (Real/Fake/Mixed)
- No details on pseudo-formula generation
- Vague "measured proportion" without numbers
- "Mixed" concept was unclear

**New Section Structure:**

**5.1 Data Components**
- Text sources: Wikipedia (CN/EN)
- Formulas: im2latex-100k (150K formulas)
- Pseudo-formulas: 10% (50% rule-based + 50% mutation)
- Tables: Simple random generation

**5.2 Document Generation Process**
Step-by-step algorithm:
1. Sample text (100-300 words)
2. Insert formulas:
   - Every 2-4 sentences: 1 display formula
   - 0-4 inline formulas per sentence
   - 90% real, 10% pseudo
3. Inject noise:
   - Every 20 words: 1 random insertion
   - Every 20 words: 1 spelling error
4. Compile with XeLaTeX

**5.3 Dataset Statistics**
- Total: 120M tokens (60M CN + 60M EN)
- ~2M formulas (1.8M real + 0.2M pseudo)
- ~120K training images
- Generation: 2 hours on i5
- Storage: 20GB
- Manual effort: ~0

**Key Addition:**
Explicit explanation of why semantic incoherence is beneficial:
> "Unlike real academic documents where formulas are semantically related to surrounding text, our synthetic documents contain formulas that are contextually irrelevant. This semantic incoherence is intentional: it prevents the model from learning spurious correlations."

---

### 6. Model Architecture

**Changes:**
- ✅ Kept overall structure (it's fine)
- ❌ **Removed**: Problematic claim that "smaller decoder reduces bias"
  - This causal claim was not well-supported
  - The decoder size is now justified only by efficiency
- ✅ **Clarified**: Custom tokenizer necessity (LaTeX + CN/EN)
- ✅ **Streamlined**: Less speculative reasoning, more factual description

---

### 7. Training and Results → Experiments (Major Restructure)

**Completely New Structure:**

**7.1 Test Set Construction**
- 400 samples total (200 CN + 200 EN)
- 100 printed + 100 handwritten per language
- **100 challenge cases** with 4 categories:
  1. Contextual Traps (30)
  2. Unconventional Notation (30)
  3. Ambiguous Symbols (20)
  4. Complex Formulas (20)
- Reference to detailed specifications in supplementary materials

**7.2 Evaluation Metrics**
- Standard: Edit Distance, BLEU, Precision, Recall
- **New specialized metrics**:
  - Hallucination Rate (%)
  - Repetition Rate (%)
  - Inference Time (seconds)

**7.3 Baseline Models**
- Nougat (state-of-the-art)
- LaTeX-OCR (formula specialist)
- Pix2Text (composite system)

**Removed:**
- ❌ Real vs Fake vs Mixed model comparison
- ❌ Handwritten-only test set (Table 3)
- ❌ Separate "typo" vs "typo-free" test sets (confusing concept)

**7.4 Results**
Four new tables:

**Table 1: Main Results (400 samples)**
- All metrics across all baselines
- Includes inference time

**Table 2: Language Breakdown**
- Chinese vs English performance
- Helps show balanced performance

**Table 3: Challenge Cases (100 samples)**
- Edit Distance, BLEU
- **Hallucination Rate** (key metric for contextual over-reliance)
- **Repetition Rate** (failure mode detection)

**Table 4: Qualitative Comparison**
- Three concrete examples:
  1. e-t vs e^{-t} (exponential context)
  2. Sum symbol vs computed result
  3. F=m-a vs F=ma (physics context)
- Shows baseline errors vs MixTex correctness
- Demonstrates the contextual over-reliance problem

**7.5 Data Generation Efficiency**
- New subsection explicitly comparing costs
- Quantified comparison: 2 hours vs weeks/months
- Emphasizes language extensibility advantage

---

### 8. Discussion and Limitations (New Section)

**Why This is Important:**
Shows maturity and honesty about the work.

**Content:**
1. **Why semantic incoherence helps**: Theoretical explanation
2. **Limitations**:
   - Only validated on CN/EN (not truly "multilingual" yet)
   - Test set is relatively small (400 samples)
3. **Applicability**: Appropriate speculation about other domains (student grading, historical documents)

---

### 9. Conclusion

**Old Conclusion Issues:**
- Only 1 paragraph (too brief)
- Jumped to education/music applications without support
- Didn't summarize key findings

**New Conclusion:**
1. **Restate contribution**: Synthetic data → cost reduction
2. **Key results**: Competitive accuracy + reduced hallucination
3. **Value proposition**: 2 hours vs weeks
4. **Future work**: Specific, reasonable extensions
   - Validate on more languages
   - Optimize real/pseudo ratio
   - Adaptive noise strategies
   - Other document understanding tasks

**Removed:**
- ❌ Music performance error detection
- ❌ Automatic spelling correction for education
- ❌ Vague "broader applicability"

---

## 📊 New Experimental Design

### Test Set Composition

```
Total: 400 samples
├── Chinese: 200
│   ├── Printed: 100
│   ├── Handwritten: 100
│   └── Challenge cases: 50
└── English: 200
    ├── Printed: 100
    ├── Handwritten: 100
    └── Challenge cases: 50
```

### Challenge Cases (100 total)

See `challenge_cases_design.md` for complete specifications.

**Category A: Contextual Traps (30)**
- Context misleads toward incorrect output
- Tests contextual over-reliance
- Example: e-t in exponential decay context

**Category B: Unconventional Notation (30)**
- Non-standard but valid symbols
- Tests notation flexibility
- Example: d𝜏 instead of dt

**Category C: Ambiguous Symbols (20)**
- Visually similar characters
- Tests visual discrimination
- Example: 0 vs O, 1 vs l vs I

**Category D: Complex Formulas (20)**
- Long/nested expressions
- Tests sequence modeling
- Example: 4-level nested fractions

### New Metrics

**Hallucination Rate:**
```
Definition: % of samples where output differs from image
            but matches contextual expectations

Detection: Automated script checking for:
  - Context-aligned errors (e-t → e^{-t})
  - Auto-simplification (sum → computed value)
  - Domain knowledge application (m-a → ma in physics)
```

**Repetition Rate:**
```
Definition: % of samples with repetitive token loops

Detection: Regex pattern matching
  Pattern: (.+)\1{3,}  (3+ repetitions)
  Example: "\frac{1}{2}\frac{1}{2}\frac{1}{2}..."
```

---

## 🔄 Narrative Arc Changes

### Before:
```
Problem: Models have bias in LaTeX OCR
Solution: Use mixed real/fake data to reduce bias
Result: Bias is reduced
```

**Issues:**
- "Bias" is poorly defined
- Not clear why this matters
- Not compelling value proposition

### After:
```
Problem: Collecting real LaTeX documents is costly and English-biased
         Current models also exhibit contextual over-reliance

Solution: Generate synthetic data by randomly mixing text + formulas
          - Semantic incoherence → forces visual reliance
          - Automated generation → 2 hours vs weeks

Result: Competitive accuracy + reduced hallucination
        + trivial language extensibility

Value: Enables LaTeX OCR for low-resource scenarios
       (non-English languages, historical documents)
```

**Strengths:**
- Clear problem (cost + bias)
- Concrete solution (quantified)
- Dual benefits (efficiency + quality)
- Specific applications (historical literature)

---

## ⚠️ Over-claims Removed

### Deleted Claims:
1. ❌ "Multilingual" support (only CN/EN validated)
2. ❌ Music performance error detection (no experiments)
3. ❌ Automatic student error correction (no experiments)
4. ❌ "High accuracy" (subjective, replaced with specific numbers)
5. ❌ "Low latency" (must provide measurements)
6. ❌ "Broader applicability to various tasks" (moved to speculation in Discussion)

### Retained but Qualified Claims:
1. ✅ Cost reduction: **Quantified** (2 hours, 20GB, zero manual effort)
2. ✅ Reduced contextual over-reliance: **Will be measured** (hallucination rate)
3. ✅ Language extensibility: **Acknowledged as future work** (not validated yet)

---

## 📋 TODO Before Submission

### Critical:
1. **Run experiments** and fill in all `\textbf{TBD}` values in tables
2. **Add citations** for all `\cite{CITATION_NEEDED}` placeholders:
   - Hallucination in vision-language models
   - Synthetic data for vision-language tasks
   - InftyReader, TexTeller, Mathpix
   - Additional formula OCR references
3. **Collect and annotate** the 400 test samples
4. **Implement** hallucination and repetition rate detection scripts
5. **Verify** Figure 1 caption matches the new narrative

### Important:
6. **Create** supplementary materials with full challenge case specifications
7. **Add** data generation code availability statement
8. **Proofread** for consistency (especially new terminology usage)
9. **Check** that all tables fit within column width constraints

### Optional but Recommended:
10. **Add** a data generation flow diagram (replace Figure 3)
11. **Include** more qualitative examples (beyond Table 4)
12. **Add** failure case analysis
13. **Provide** training loss curves

---

## 📊 Tables Summary

### Table 1: Main Results
- **Purpose**: Overall performance comparison
- **Data needed**: All 4 models × all 5 metrics on 400 samples
- **Key comparison**: MixTex vs Nougat (SOTA)

### Table 2: Language Breakdown
- **Purpose**: Show balanced CN/EN performance
- **Data needed**: All 4 models × CN/EN × 2 metrics (Edit Dist, BLEU)
- **Key insight**: Synthetic data works for both languages

### Table 3: Challenge Cases
- **Purpose**: Test contextual over-reliance
- **Data needed**: All 4 models × 4 metrics on 100 challenge samples
- **Key metrics**: Hallucination rate, Repetition rate
- **Expected outcome**: MixTex should have lower hallucination rate

### Table 4: Qualitative Comparison
- **Purpose**: Concrete examples of contextual over-reliance
- **Data needed**: 3 carefully selected error cases from baselines
- **Key message**: MixTex avoids these errors

---

## 🎯 Key Messages to Convey

1. **Data Collection is Expensive**
   - Nougat: weeks of crawling + cleaning
   - MixTex: 2 hours automated generation

2. **Synthetic Data Works**
   - Competitive accuracy with real-data models
   - No quality sacrifice

3. **Semantic Incoherence is a Feature**
   - Prevents spurious correlations
   - Forces visual reliance
   - Reduces hallucinations

4. **Language Extensibility**
   - Wikipedia available in 300+ languages
   - Enables historical document digitization
   - Particular value for DE/FR/RU math literature

5. **Practical Value**
   - Low-resource scenarios
   - Student grading (preserve errors)
   - Historical preservation

---

## 📁 Files in This Revision

1. **PaperForReview_REVISED.tex** - Complete new version with \textbf{TBD} placeholders
2. **REVISION_SUMMARY.md** - This document
3. **challenge_cases_design.md** - Detailed specifications for 100 test cases

---

## 🔍 Self-Check Questions

Before submission, verify:

- [ ] Is every claim supported by experiments or properly qualified?
- [ ] Are all tables properly filled with real data?
- [ ] Are all citations complete (no CITATION_NEEDED)?
- [ ] Does the abstract match the paper content?
- [ ] Is the narrative consistent throughout?
- [ ] Are limitations honestly discussed?
- [ ] Is the value proposition clear?
- [ ] Can readers reproduce the method from the description?

---

## 💡 Strengths of This Revision

1. **Clear Value Proposition**: Cost reduction + competitive accuracy
2. **Honest Limitations**: Only CN/EN, no over-claiming
3. **Reproducible Method**: Detailed data generation algorithm
4. **Quantified Benefits**: 2 hours, 20GB, specific token counts
5. **Concrete Evaluation**: 400 samples + 100 challenge cases
6. **Novel Metrics**: Hallucination rate directly tests the claim
7. **Practical Applications**: Historical literature digitization

---

## 🚀 Next Steps

1. **Review** this revision carefully
2. **Provide feedback** on any sections that need adjustment
3. **Run experiments** to fill in TBD values
4. **Add citations** for CITATION_NEEDED placeholders
5. **Test compile** the LaTeX to check formatting
6. **Prepare supplementary materials**

---

END OF REVISION SUMMARY
