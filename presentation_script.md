# MixTex Presentation Script
**Target Duration: 20-25 minutes**
**Date: Wednesday Meeting**

---

## SLIDE 1: Title Slide (30 seconds)

**Say:**
"Good morning, Professor Weiss and Professor Zhao. Thank you for giving me this opportunity to present my research proposal with a fresh perspective. As you suggested, I'm approaching this as if it were a completely new project, focusing on clearly defining the problem, the solution, and the experimental design.

Today I'll present MixTex: a method for generating training data for LaTeX OCR when authentic mixed text-formula documents are unavailable. I'll spend about 20-25 minutes covering the problem, proposed solution, experimental design, expected contributions, and work assessment, leaving time for your questions and feedback."

---

## SECTION 1: PROBLEM DEFINITION

## SLIDE 2: The Real-World LaTeX OCR Challenge (2 minutes)

**Say:**
"Let me start by defining what users actually need from a LaTeX OCR system. In real-world applications, users need to recognize complete documents where text and mathematical formulas are interspersed throughout the content.

For example, this slide shows a typical theorem statement from a textbook. Notice how the formula isn't isolated—it's embedded within continuous natural language text. The text provides context, the formula provides precise mathematical content, and together they form a coherent statement.

This is what students see in textbooks, what professors write in lecture notes, and what researchers include in papers. The formulas don't exist in isolation—they're part of the narrative flow.

The challenge I want to address is: current publicly available datasets don't support training models for this kind of recognition."

---

## SLIDE 3: The Dataset Gap (3 minutes)

**Say:**
"This slide illustrates the fundamental gap between what's available and what's needed.

On the left side, you see what we can actually obtain from public sources. Datasets like im2latex-100k and MathBridge provide formula images—but only isolated formulas. You get the mathematical expression with no surrounding context, no document structure, no text flow.

On the right side is what we actually need: mixed text-formula documents. These have formulas embedded within sentences, display equations with preceding and following text, and the realistic document structure that OCR systems must handle.

This is the core problem I'm addressing. It's not about model architecture or training techniques—it's a fundamental data availability problem. The datasets that exist don't match the task we need to solve.

And this matters because a model trained only on isolated formulas won't know how to handle text-formula boundaries, inline formulas within sentences, or the layout patterns of real documents."

**Pause for questions**

---

## SLIDE 4: Why Can't We Use Real Documents? (2.5 minutes)

**Say:**
"A natural question is: why not just collect real mixed text-formula documents? Let me explain why this is impractical, especially at individual researcher scale.

Option 1 is the ArXiv dataset. Yes, it has millions of papers, but consider the costs: weeks of crawling and cleaning, it's English-only—Nougat literally cannot process Chinese characters—and it's all university-level research papers, creating a distribution mismatch when you want to recognize, say, middle school textbooks.

Option 2 is collecting real educational materials ourselves. Our pilot study showed that collecting just 100 samples took 3 hours. Scaling to the thousands of samples typically needed for deep learning means hundreds of hours of manual work. And for Chinese specifically, fewer than 1% of papers in major academic databases provide LaTeX source files. The data simply doesn't exist publicly.

Option 3 is multilingual extension. If you want to support multiple languages with traditional data collection, you need to repeat this arduous process for each language. This scales linearly with the number of languages and is completely infeasible for low-resource languages where LaTeX documents may not exist at all.

So the core problem is: How can we train mixed text-formula OCR without extensive real document collection?"

---

## SECTION 2: PROPOSED SOLUTION

## SLIDE 5: Key Insight - Separate Structure from Semantics (2 minutes)

**Say:**
"My key insight is that we can separate structural patterns from semantic coherence. Let me explain what I mean.

OCR models need to learn visual-structural patterns: what does a document with mixed text and formulas look like? Where do formulas appear? How are they laid out relative to text? These are structural questions.

But semantic coherence—whether the formulas actually relate meaningfully to the surrounding text—is secondary for the recognition task itself. The model needs to recognize that there's a formula here, not understand why this particular formula appears in this particular context.

So my approach is simple: take real natural language text from Wikipedia, take real mathematical formulas from the im2latex dataset, and randomly combine them without preserving semantic relationships.

The example on the slide shows this: 'Climate change affects global temperatures' followed by the Helmholtz equation, followed by 'renewable energy is important.' Completely incoherent semantically, but structurally, it's a valid mixed text-formula document. The model can learn from this that formulas appear in certain positions, have certain visual characteristics, and interface with text in specific ways.

This is the foundation of my method: programmatically assemble training data from real components."

---

## SLIDE 6: Data Generation Process (3 minutes)

**Say:**
"Let me walk through the concrete algorithm.

Step 1: Sample 100 to 300 words from Wikipedia. This gives us real, grammatically correct natural language in whatever language we're targeting.

Step 2: Insert formulas at random positions. Every 2 to 4 sentences, I insert one display formula. Within each sentence, I insert 0 to 4 inline formulas at random positions. 90% of these formulas come from the real im2latex dataset, and 10% are generated by mutating existing formulas—things like substituting variables or changing operators.

Step 3: Inject controlled noise to simulate real-world imperfections. About 5% random word insertions and 5% spelling perturbations. This helps the model be robust to OCR-typical scenarios.

Step 4: Compile the LaTeX with XeLaTeX using randomly selected fonts to create visual diversity.

Step 5: Convert to PNG at 200 DPI for our training images.

The data sources are simple: Wikipedia provides text in over 300 languages, im2latex provides 150,000 mathematical expressions, and we generate an additional 10% through mutation.

The output is remarkable: 1 million training images totaling 120 million tokens—60 million Chinese and 60 million English—generated in just 2 hours on an ordinary consumer CPU with zero manual annotation effort.

The key advantage here is that I can generate training data for any language where Wikipedia exists, without needing a single real LaTeX document in that language. For Chinese, where essentially no public LaTeX data exists, this is transformative."

---

## SLIDE 7: Why This Approach Works (2 minutes)

**Say:**
"I want to address a terminology issue that Professor Weiss raised. This data isn't 'synthetic' and it isn't 'artificial'—those terms are misleading because they suggest the components are fake.

What's real: The text is 100% real Wikipedia content, grammatically correct natural language. The formulas are 90% real from im2latex, validated mathematical expressions. The LaTeX compilation process is the real rendering pipeline.

What's programmatic: The text-formula pairing is random, not semantic. The formula placement follows stochastic rules rather than meaningful relationships. The overall document structure is assembled algorithmically.

So I propose we call this 'programmatically assembled from real components' rather than 'synthetic' or 'artificial.'

My hypothesis is that models can learn the visual and structural patterns they need from this programmatically assembled data, then adapt to real semantic documents with minimal fine-tuning. The semantic relationships are a refinement that can be learned quickly with a small amount of real data, while the structural patterns—which require large-scale data—can be learned from programmatically assembled examples."

**Pause for questions on methodology**

---

## SECTION 3: EXPERIMENTAL DESIGN

## SLIDE 8: Three-Stage Data Pipeline (2 minutes)

**Say:**
"Now let me describe the experimental design, which has three completely independent datasets.

First, the pretraining dataset: 1 million programmatically assembled samples, 120 million tokens, used for initial training. Cost is 2 hours of generation time and zero dollars for manual annotation.

Second, the fine-tuning dataset: 500 real LaTeX documents collected from various sources—textbooks, academic papers, lecture notes. These have authentic text-formula relationships. Cost is about 15 hours of manual collection. This dataset is completely independent from the test set.

Third, the test set: 500 independently collected real educational materials, split into 100 K-through-8 samples and 400 university-level samples. Cost is estimated at over 30 hours of manual collection and annotation. This is completely independent from both the pretraining and fine-tuning sets.

Critical point about independence: The fine-tuning set and test set have zero overlap. The pretraining uses programmatically assembled data, so there's no possibility of overlap there either. And we cannot use ArXiv papers for testing because we can't determine which specific papers were in Nougat's training set—that would risk test set contamination."

---

## SLIDE 9: Test Set Design - Educational Materials (2 minutes)

**Say:**
"Let me explain why I chose educational materials for the test set and how it's structured.

Why educational materials? First, it's a real-world use case—digitizing textbooks and lecture notes is a major application. Second, it lets us test domain adaptation since Nougat was trained on university-level ArXiv papers. Third, it covers multiple difficulty levels, giving us a more comprehensive evaluation.

The table shows the breakdown: 50 samples from elementary school (K-5), 50 from middle school (6-8), and 400 from university level. Each level is split evenly between Chinese and English. Sources include textbooks, worksheets, exams, lecture notes, and presentation slides.

Current status: I've collected about 100 samples so far, which took 3 hours. Based on this rate, I estimate 25 to 30 hours for the remaining 400 samples. This includes manual annotation to verify the LaTeX ground truth.

This structure will let me test both domain adaptation across educational levels and multilingual capability across Chinese and English."

---

## SLIDE 10: Research Questions (2.5 minutes)

**Say:**
"The experimental design addresses three specific research questions.

RQ1: Fine-tuning Necessity. Does fine-tuning on just 500 real samples improve performance over using only the programmatically assembled pretraining data? This is a simple ablation study comparing MixTex-PT—pretrain only—against MixTex-FT—pretrain plus fine-tune. If fine-tuning helps significantly, it validates the two-stage training paradigm.

RQ2: Domain Generalization. How do models generalize to out-of-distribution educational materials across different levels? I hypothesize that Nougat, trained exclusively on university-level ArXiv papers, will show performance degradation on elementary and middle school materials. MixTex's domain-agnostic training should maintain more consistent performance across levels. This is tested by comparing performance on the K-8 subset versus the university subset.

RQ3: Multilingual Capability. This has two parts. First, can programmatically assembled training enable Chinese LaTeX OCR where existing systems provide no support? Nougat literally cannot process Chinese text, so we're demonstrating a capability that doesn't exist in current systems. Second, can we achieve competitive English performance compared to Nougat, which was trained on millions of ArXiv papers? Here we're showing that programmatically assembled data works despite the massive data advantage Nougat has.

These three questions are designed to validate different aspects of the approach: the necessity of real data fine-tuning, domain generalization properties, and multilingual capability."

---

## SLIDE 11: Baseline Comparison (1.5 minutes)

**Say:**
"Our primary baseline is Nougat, the current state-of-the-art for English LaTeX OCR. It was trained on millions of ArXiv papers, all university-level research papers, and it's English-only—it cannot process Chinese characters at all.

Now, why can't we test on ArXiv papers? The simple answer is data contamination risk. We cannot determine which specific ArXiv papers are in Nougat's training set. The authors don't provide that level of detail. If we tested on ArXiv and got poor results, Nougat's authors could rightfully say 'you tested on our training data.' So we need clean, independent evaluation data—hence the educational materials test set.

Our evaluation metrics are standard: Edit Distance for character-level accuracy, BLEU for token-level accuracy, token-level Precision and Recall, and we'll assess statistical significance using paired t-tests with p less than 0.05.

This ensures rigorous, reproducible evaluation that can't be challenged on methodological grounds."

---

## SLIDE 12: Expected Experimental Results - Table 1 (1.5 minutes)

**Say:**
"Let me show you the expected results structure, though I want to emphasize these are hypotheses, not actual results yet.

Table 1 is the ablation study on all 500 test samples. I expect MixTex-PT—pretrain only—to perform worse than the fine-tuned version, demonstrating that the 500 real samples do provide value. MixTex-FT—pretrain plus fine-tune—should perform best, validating our two-stage approach. And Nougat should be competitive, which would show that our method achieves similar performance to a model trained on millions of papers, but with far less data collection effort.

Table 2 shows domain adaptation broken down by educational level. My hypothesis is that MixTex will show consistent performance across K-8 and university levels because it wasn't trained on domain-specific patterns. Nougat, trained on university-level ArXiv papers, should perform worse on K-8 materials and better on university materials, showing overfitting to its training domain.

If this hypothesis is supported, it would suggest that programmatically assembled, domain-agnostic training data actually provides better generalization—though I want to position this as an empirical finding, not a primary claim."

---

## SLIDE 13: Expected Experimental Results - Table 3 (1.5 minutes)

**Say:**
"Table 3 shows multilingual capability broken down by language, and this is where the value proposition becomes very clear.

For Chinese, with 250 samples, Nougat shows 'No support'—literally cannot process these images. MixTex should show functional performance, demonstrating a capability that doesn't exist in current systems. This alone is a significant contribution.

For English, with 250 samples, we compare against Nougat's strong baseline. I'm not claiming we'll beat Nougat on English—Nougat was trained on millions of papers. I'm claiming we'll achieve competitive performance despite training on programmatically assembled data generated in 2 hours.

The key message isn't 'our model is better.' The key message is 'we enable Chinese LaTeX OCR where no solution exists, and we achieve competitive English performance with orders of magnitude less data collection effort.'

This is about practical feasibility and accessibility, not about squeezing out marginal performance gains."

**Pause for questions on experiments**

---

## SECTION 4: EXPECTED CONTRIBUTIONS

## SLIDE 14: Primary Contribution - Practical Data Generation Method (2 minutes)

**Say:**
"Let me clearly state what I believe is the primary contribution of this work.

The problem being solved: Publicly available datasets contain only isolated formulas. Real applications need mixed text-formula documents. Collecting real documents is expensive or impossible for many languages.

Our solution: Programmatically assemble real text plus real formulas in 2 hours on consumer hardware, works for any language with Wikipedia coverage—over 300 languages—and costs zero dollars in manual annotation.

The impact: Individual researchers can now train LaTeX OCR models. Chinese LaTeX OCR becomes feasible when it was previously impossible. The barrier to entry drops from institutional scale to individual scale.

The core claim is this: Programmatically assembled data from real components enables competitive mixed text-formula OCR with minimal real data fine-tuning.

This is not a claim about beating state-of-the-art performance. It's a claim about making the task feasible when it wasn't before. That's the contribution."

---

## SLIDE 15: Secondary Contributions (1.5 minutes)

**Say:**
"Beyond the primary contribution, there are three secondary contributions worth noting.

First, intrinsic multilingual capability. Because formulas are language-agnostic and the text source is substitutable, we can enable a new language by substitution rather than collection. Chinese LaTeX OCR is, to my knowledge, the first working system of its kind.

Second, domain adaptation findings. This is purely empirical—if the programmatically assembled training shows better generalization to K-12 materials, that's an interesting finding worth investigating further. But it's not the primary motivation for this work. It's an observed phenomenon that merits future research.

Third, the two-stage training paradigm itself: large-scale programmatic pretraining followed by small-scale real data fine-tuning. This may apply to other document understanding tasks facing data scarcity.

These are valuable, but they're secondary to the core contribution of making mixed text-formula OCR feasible for researchers who can't collect large-scale real datasets."

---

## SLIDE 16: What We Are NOT Claiming (1.5 minutes)

**Say:**
"Now let me be very explicit about what's out of scope, because I think clarity here is important for honest positioning.

We are NOT claiming our model is better than Nougat on English. We're claiming it's competitive with far less data collection.

We are NOT claiming we solve the bias problem in LaTeX OCR. We're observing that there may be better domain adaptation, which is interesting.

We are NOT claiming our method reduces hallucination. We're noting it as an interesting future research direction, not a validated claim.

We are NOT claiming this works for all 300+ languages. We're validating Chinese and English only, though the method should extend.

The honest positioning is: This is a practical data generation method that enables mixed text-formula OCR for languages and domains where real data is scarce. The approach is simple, reproducible, and effective. That's the story."

**Pause for questions on contributions**

---

## SECTION 5: WORK ASSESSMENT

## SLIDE 17: What's Already Done (1.5 minutes)

**Say:**
"Let me assess the current state of the work and what remains.

What's completed: The data generation pipeline is tested and functional. The model architecture—Swin Transformer plus GPT-2, 78 million parameters—is implemented and training successfully. The training infrastructure works and takes 16 hours on a single 4090. I've completed a pilot data collection of 100 test samples in 3 hours, validating the collection rate. And I've run initial experiments, though these need to be redone with the new framing.

What needs revision: The problem statement in the paper needs to clearly articulate the isolated versus mixed gap. The experimental design needs to be restructured around the new research questions and table structures. The terminology throughout needs to shift to 'programmatically assembled.' And the contribution positioning needs to emphasize the method rather than performance claims.

The key insight here is that most of the implementation work is already done. The primary remaining task is reframing the narrative and re-running experiments with a clearer evaluation protocol. I'm not starting from scratch—I'm refining and clarifying."

---

## SLIDE 18: Remaining Work - Data Collection (1.5 minutes)

**Say:**
"Looking at remaining data collection work, this table breaks down the tasks.

The fine-tuning set of 500 real samples is in progress, estimated at 10 hours remaining. The test set K-8 portion—100 samples—is partially done, about 6 hours remaining. The test set university portion—400 samples—is partially done, about 20 hours remaining. And annotation verification to ensure ground truth accuracy will take about 10 hours.

Total remaining: 46 hours of data collection work.

But this can be parallelized. I can collect data while models are training. My co-author can potentially help with collection. Annotation can be batched and done efficiently.

The timeline estimate is 2 to 3 weeks for complete data collection working part-time, 1 week for running all experiments including multiple training runs, and 1 week for paper revision. Total: 4 to 5 weeks to a submission-ready paper.

This seems feasible within the academic timeline."

---

## SLIDE 19: Remaining Work - Experiments (1.5 minutes)

**Say:**
"For the experimental work, this table shows GPU time and analysis time.

MixTex-PT, pretrain only, takes 16 hours of training plus 2 hours of evaluation. MixTex-FT adds just 30 minutes of fine-tuning. Baseline comparison with Nougat requires zero training time since it's pre-trained, but 4 hours for thorough evaluation. Statistical significance tests add another 4 hours. Optional ablation studies would add 8 hours of training and 4 hours of analysis.

Total: 40 to 48 hours of GPU time, which on a single 4090 means about 2 days of wall-clock time running continuously. 16 hours of analysis time.

I have access to a 4090, so resources aren't a constraint. I can run experiments in parallel with data collection, and I plan to do multiple runs with different random seeds for statistical reliability.

Risk mitigation: What if the results don't support the hypotheses? The key point is that the method contribution stands alone. Even if domain adaptation results are null or negative, the practical data generation method is still valuable. Null results are scientifically valid."

---

## SLIDE 20: Remaining Work - Paper Revision (1 minute)

**Say:**
"For paper revision, the major sections that need rewriting are:

Introduction: Clearly state the isolated versus mixed gap, quantify data collection costs, and position this as a practical method rather than a bias solution.

Related Work: Explicitly discuss why isolated formula datasets exist and why they don't solve our problem.

Experiments: New research questions, clear description of all three datasets, and statistical rigor with t-tests and confidence intervals.

Discussion: Honest limitations and position bias/hallucination as future work rather than current claims.

Estimated time is 1 to 2 weeks for complete revision including iteration based on feedback.

This is substantial work but not unprecedented. It's clarifying and refining rather than starting over."

---

## SLIDE 21: Risk Assessment (1.5 minutes)

**Say:**
"Let me honestly assess the risks in this project.

Low risks: Data generation is already working and reproducible. Model training infrastructure is proven. Chinese capability shows positive preliminary results.

Medium risks: Data collection time might exceed 30 hours—if so, we can reduce the test set to 300 samples if needed. Domain adaptation results might not be statistically significant—if so, we position it as exploratory rather than a primary claim.

High risk: Performance might not be competitive with Nougat on English. However, we already have preliminary positive results suggesting this won't be an issue. And if it is, the fallback is to focus on Chinese capability plus the data efficiency story. Chinese alone is publishable since no current system supports it.

Overall assessment: Low to moderate risk. The core contribution—the data generation method—is solid regardless of what the performance numbers end up being. Even weak performance would be publishable as 'here's a method that makes this feasible with these results,' as long as we're honest about the limitations."

---

## SECTION 6: SUMMARY

## SLIDE 22: Project Summary (2 minutes)

**Say:**
"Let me summarize the entire project in one slide.

The core problem: Publicly available LaTeX datasets contain only isolated formulas, but real-world applications require mixed text-formula document recognition. This is a fundamental data availability gap.

The proposed solution: Programmatically assemble training data from real components—Wikipedia text plus im2latex formulas—without preserving semantic relationships. Generate large-scale training data quickly and cheaply.

The key advantages: 2 hours of generation time versus weeks of manual collection. Works for any language via Wikipedia—over 300 languages. Enables Chinese LaTeX OCR which is currently impossible. Scales to individual researcher level rather than requiring institutional resources.

The expected contributions: First, a practical data generation method for mixed text-formula OCR. Second, intrinsic multilingual capability via component substitution. Third, empirical findings on domain adaptation as exploratory work.

This is a straightforward project with clear practical value. It solves a real problem—data scarcity for mixed text-formula OCR—with a simple, reproducible method."

---

## SLIDE 23: Timeline to Submission (1 minute)

**Say:**
"The timeline to submission is aggressive but feasible.

Phase 1: Data collection over 2 to 3 weeks, delivering 500 test samples and 500 fine-tuning samples.

Phase 2: Experiments over 1 week, filling all tables with real results and computing statistics.

Phase 3: Paper revision over 1 to 2 weeks, producing a submission-ready draft.

Total: 4 to 6 weeks to a complete paper.

Milestones are: weeks 1-2 complete data collection, week 3 run all experiments, week 4 draft the complete paper, weeks 5-6 iterate and polish based on feedback.

The target venue would be WACV 2025 if the timeline permits, or the next available vision conference if we need more time. I'm not rushing to compromise quality."

---

## SLIDE 24: Open Questions for Discussion (2 minutes)

**Say:**
"Before I conclude, I want to raise some open questions where I'd appreciate your guidance.

On experimental design: Is 500 test samples sufficient, or should I target 1000 for stronger statistical power? Should I include more baseline comparisons like LaTeX-OCR and Pix2Text, or is Nougat sufficient? Is the K-8 versus university split the right way to test domain adaptation, or would you suggest different educational levels?

On contribution positioning: Should I de-emphasize domain adaptation even further than I have? Is 'programmatically assembled' the right terminology, or would you suggest something else? Should I mention bias and hallucination at all in this paper, or save those topics entirely for future work?

On scope: Should I validate a third language like French or Spanish to strengthen the multilingual claim? Should I compare different synthetic-to-real ratios in an ablation? Should I ablate the noise injection components to understand their contribution?

I'm open to your suggestions on any of these points."

---

## SLIDE 25: Thank You (30 seconds)

**Say:**
"Thank you for your time and attention. I'm very grateful for your guidance in helping me think through this project more clearly. I recognize that I moved too quickly into implementation before, and this process of stepping back and re-examining the problem has been incredibly valuable.

I'm now ready for your questions, feedback, and suggestions."

**[Open for discussion]**

---

## BACKUP SLIDES

## BACKUP SLIDE 1: Detailed Data Generation Algorithm

**If asked about implementation details:**

"This slide shows the detailed pseudocode for the data generation algorithm. The outer loop generates 1 million samples. For each sample, we sample 100 to 300 words from the Wikipedia corpus, split into sentences, and insert a Poisson-distributed number of inline formulas per sentence with lambda equals 1. Every 2 to 4 sentences we insert one display formula. We inject noise at a 5% rate for both insertions and misspellings. Then we compile with XeLaTeX and convert to PNG at 200 DPI.

The key is parallelization: using 8 CPU cores, we can generate 1 million samples in about 2 hours. This is the practical efficiency that makes the approach feasible."

---

## BACKUP SLIDE 2: Model Architecture Details

**If asked about model architecture:**

"The encoder is Swin Transformer tiny variant, taking 500 by 400 RGB input and outputting embedding vectors. It's pretrained on ImageNet and has about 40 million parameters. The decoder is GPT-2 with hidden size 768, 12 attention heads, and 4 layers. It handles up to 296 tokens and uses random initialization because the tokenizer is specifically designed for multilingual LaTeX including Chinese, English, and mathematical symbols. The decoder has about 38 million parameters.

Training uses AdamW optimizer with cosine learning rate decay from 1.3 times 10 to the minus 4 down to 7 times 10 to the minus 6. Batch size is 24, we use FP16 mixed precision, and we train for 5 epochs during pretraining and 3 epochs during fine-tuning."

---

## Handling Q&A

### If asked: "How do you know this will work?"

**Answer:**
"I have preliminary results from initial experiments that show positive signals. The Chinese capability works—models can recognize Chinese text interspersed with formulas. English performance is competitive with Nougat in early tests. However, I need to re-run these experiments with the clearer protocol we've discussed to make robust claims. The theoretical foundation is sound: models need to learn visual-structural patterns, and those can be learned from programmatically assembled data. The semantic refinement comes from fine-tuning."

### If asked: "Why not just collect more real data?"

**Answer:**
"That's a fair question. For English, you could potentially invest the time to collect thousands of real samples. But for Chinese, the data simply doesn't exist publicly. Less than 1% of papers in CNKI and Wanfang provide LaTeX source. And if you want to extend to 5 or 10 languages, you multiply the data collection effort by 5 or 10. My method inverts this: the effort is constant regardless of language—just substitute the Wikipedia text source. For any single language, maybe collection is feasible. For multilingual coverage, programmatic assembly is the only practical approach."

### If asked: "What if Nougat beats you on all metrics?"

**Answer:**
"If Nougat significantly outperforms us on English, we still have Chinese as a contribution—Nougat can't handle Chinese at all. And we can position the paper as: here's a method that makes training feasible with limited resources, here are the results and limitations, here are the tradeoffs. Not every paper needs to beat state-of-the-art on standard metrics. Enabling a new capability (Chinese) or dramatically reducing resource requirements are valuable contributions even if performance doesn't exceed a model trained on millions of examples."

### If asked: "Is this really novel or just standard data augmentation?"

**Answer:**
"It differs from standard data augmentation in important ways. Standard augmentation applies transformations to existing real data—rotations, noise, distortions. We're synthesizing document structure itself from components. The closest prior work is rendering real LaTeX sources, but that still requires having the LaTeX sources. We're creating the LaTeX sources programmatically. The novelty is in addressing the structural gap: available datasets have wrong structure (isolated), we create correct structure (mixed) without needing real mixed documents. It's simple, but simple doesn't mean non-novel if it solves a real problem that wasn't solved before."

---

## CLOSING NOTES

**After Q&A:**
"Thank you again for this discussion. Your feedback on [mention specific points they raised] is very helpful and I'll incorporate it as I move forward. Should I send you an updated experimental design document before I begin the full data collection, or are you comfortable with me proceeding based on today's discussion?"

**Be prepared to take notes on:**
- Suggested changes to experimental design
- Concerns about specific claims or terminology
- Recommendations for additional baselines or ablations
- Timeline expectations and milestone check-ins
