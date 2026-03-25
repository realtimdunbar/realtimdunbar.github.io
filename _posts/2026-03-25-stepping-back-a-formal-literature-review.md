---
title: "Stepping Back: A Literature Review of LLMs for Automated Theorem Proving"
author: "Tim Dunbar"
date: 2026-03-25 08:00:00 -0400
layout: post
excerpt: "After hitting a wall with dataset construction, I'm doing what I should have done first — a formal literature review of the intersection of large language models and automated theorem proving."
---

## Why I'm Doing This Now

Four posts into the Symbolic series, I've built a pipeline, scraped GitHub, validated specs through SANY and TLC, and arrived at a humbling number: **zero** TLA+ specifications in my dataset survive end-to-end model checking. Eight unique specs passed syntax validation. That's it.

The honest thing to do here is not to grind harder on the same approach. It's to stop and ask: **what has everyone else already figured out?**

I should have done a literature review before writing a single line of code. That's not hindsight — that's methodology. In graduate school, a literature review is the first chapter of your thesis for a reason. It prevents you from reinventing wheels, reveals approaches you'd never consider, and — crucially — tells you where the open problems actually are.

So this post isn't about Symbolic's architecture or my dataset pipeline. It's about the process of conducting a formal literature review at the intersection of large language models and automated theorem proving — what tools I'm using, how I'm organizing the search, and how I plan to synthesize what I find.

The actual findings will come in the next post. This one is about the method.

## What Is a Formal Literature Review?

A literature review is not a Google search. It's not skimming abstracts and citing whatever supports your argument. A formal literature review is a **systematic, reproducible process** for surveying existing research in a problem space.

The key properties:

- **Defined scope.** You state exactly what you're looking for and what you're not.
- **Reproducible search.** Someone else could follow your search strategy and find the same papers.
- **Inclusion/exclusion criteria.** You decide up front what counts, before you see results.
- **Synthesis, not summary.** You identify themes, contradictions, and gaps — not just restate each paper.

There are different levels of rigor. A full **systematic literature review** (SLR) follows protocols like PRISMA, pre-registers the search strategy, and may involve multiple reviewers for bias reduction. That's what you'd do for a journal publication.

What I'm doing is closer to a **scoping review** — a structured but less rigid survey meant to map the landscape of a research area and identify key themes and gaps. It's the right tool when you're asking "what has been done?" rather than "what is the effect size of X?"

## Defining the Scope

The first step is to define the research questions. Not the questions I want to *answer* — the questions that guide what I *search for*.

**Primary question:** What approaches have been explored for using large language models to generate, assist with, or verify formal proofs in automated theorem proving systems?

**Secondary questions:**
- What formal languages and proof assistants are being targeted (Lean, Coq, Isabelle, TLA+, others)?
- What LLM architectures and training strategies have shown promise?
- How are training datasets constructed for low-resource formal languages?
- What evaluation metrics and benchmarks are used?
- Where are the open problems and failure modes?

Notice how much broader this is than "can I fine-tune Llama to write TLA+?" That's deliberate. Symbolic targets TLA+ specifically, but the techniques for training LLMs on Lean proofs or Coq tactics may transfer. The dataset construction challenges for Isabelle are almost certainly relevant to mine. By widening the aperture, I avoid tunnel vision.

## The Search Strategy

### Choosing Databases

Academic search is fragmented. No single database covers everything. Here's what I'm using and why:

**Semantic Scholar** ([semanticscholar.org](https://www.semanticscholar.org))
My primary search engine. Semantic Scholar indexes over 200 million papers, provides excellent API access, and has features specifically designed for literature reviews — citation graphs, TLDR summaries, and influence scores. Its AI-powered relevance ranking tends to surface highly-cited foundational papers alongside recent work, which is exactly what I need.

**arXiv** ([arxiv.org](https://arxiv.org))
The preprint server where most ML and formal methods research lands first. Papers here are often months ahead of journal publication. I'll search arXiv directly for the most recent work that Semantic Scholar may not have indexed yet.

**Google Scholar** ([scholar.google.com](https://scholar.google.com))
Broader coverage than Semantic Scholar, especially for older work and conference proceedings. I use it as a secondary source and for "cited by" chains — finding newer papers that cite a foundational one.

**ACM Digital Library and IEEE Xplore**
For conference papers from venues like ICML, NeurIPS, ICLR, CAV, POPL, and ITP that may not be freely available on arXiv.

### Constructing Search Queries

The query design matters enormously. Too narrow and you miss relevant work. Too broad and you drown in noise. I'm using a structured approach with Boolean operators:

**Core query:**
```
("large language model" OR "LLM" OR "transformer" OR "neural theorem proving")
AND
("theorem proving" OR "formal verification" OR "proof assistant" OR "proof generation")
```

**Variant queries for specific aspects:**
```
# Dataset construction
("training data" OR "dataset" OR "benchmark")
AND ("formal proof" OR "theorem proving" OR "proof assistant")

# Specific proof assistants
("Lean" OR "Coq" OR "Isabelle" OR "TLA+" OR "HOL")
AND ("language model" OR "machine learning" OR "neural")

# Evaluation and metrics
("evaluation" OR "benchmark" OR "accuracy")
AND ("automated theorem proving" OR "proof generation")
AND ("language model" OR "neural" OR "transformer")
```

I'll run each query across each database, tracking what I searched, when, and how many results I got. This is the "reproducible" part — if someone wanted to verify my survey, they could rerun these queries.

### Snowball Sampling

Queries only get you so far. Some of the most relevant papers will be found through **snowball sampling**:

- **Backward snowballing:** For each key paper I find, I check its references. If a paper on LLM-based Lean proving cites a foundational paper on neural theorem proving, I add that to my review.
- **Forward snowballing:** For foundational papers, I check who has cited them since. Semantic Scholar's "cited by" feature and Google Scholar's citation tracking are essential here.

This is how you find the papers that don't match your keywords but are deeply relevant. A 2020 paper on "neural guided proof search" might not contain the phrase "large language model" but could be foundational to the entire field.

## Organizing with Zotero

Raw search results are useless without organization. I'm using **Zotero** ([zotero.org](https://www.zotero.org)) — a free, open-source reference manager — as my central hub.

### Why Zotero?

- **Free and open source.** No subscription paywalls.
- **Browser extension.** One click to save a paper from Semantic Scholar, arXiv, or any journal site. It automatically extracts title, authors, date, abstract, and DOI.
- **PDF management.** Zotero stores and indexes PDFs. I can annotate directly in the reader and those annotations become searchable.
- **Tagging and collections.** I create nested collections that mirror my research questions and tag papers by theme.
- **Citation export.** When I write the synthesis post, Zotero generates citations in any format.
- **Zotero Connector + Better BibTeX plugin.** If I later want to write in LaTeX, the integration is seamless.

### My Collection Structure

```
LLM-ATP Literature Review/
├── 01 - Foundational Papers/
│   ├── Neural Theorem Proving (pre-LLM)
│   └── Transformer Architecture for Math
├── 02 - LLM Proof Generation/
│   ├── Lean
│   ├── Coq
│   ├── Isabelle
│   ├── TLA+
│   └── Other/Multi-system
├── 03 - Dataset Construction/
│   ├── Synthetic Generation
│   ├── Corpus Extraction
│   └── Benchmarks
├── 04 - Training Strategies/
│   ├── Fine-tuning
│   ├── Prompt Engineering
│   ├── Reinforcement Learning
│   └── Retrieval-Augmented
├── 05 - Evaluation & Metrics/
└── 06 - Surveys & Meta-analyses/
```

Each paper gets tagged with relevant themes. A single paper might live in "02 - LLM Proof Generation / Lean" but also be tagged `dataset-construction` and `reinforcement-learning` if it covers those aspects.

### Annotation Strategy

When I read each paper, I annotate with a consistent structure:

- **Yellow highlight:** Key claims and findings
- **Blue highlight:** Methodology details I might adopt
- **Red highlight:** Limitations, failure modes, open problems
- **Green highlight:** Dataset details (size, source, construction method)
- **Notes:** My own thoughts on relevance to Symbolic

This isn't busywork — it's what makes synthesis possible later. When I sit down to write about dataset construction approaches across the field, I can filter by green highlights and get every relevant data point without re-reading 40 papers.

## Inclusion and Exclusion Criteria

Before I start reading, I define what's in scope and what's not. This prevents the review from expanding infinitely.

**Inclusion criteria:**
- Published 2019 or later (transformer era — pre-transformer neural theorem proving is foundational context only)
- Addresses the use of neural language models for theorem proving, proof generation, or formal verification
- Targets at least one established proof assistant or formal system
- Available in English
- Peer-reviewed publication, accepted preprint, or technical report from a recognized research group

**Exclusion criteria:**
- Pure code generation without formal verification (e.g., Copilot-style code completion)
- Natural language reasoning or informal mathematical problem solving (e.g., GSM8K, MATH benchmark)
- Papers focused solely on symbolic AI or traditional ATP without neural components
- Blog posts, tutorials, or documentation (useful for context but not for the review itself)

The boundary between "code generation" and "proof generation" is blurry. A paper about using LLMs to generate Dafny code with verification conditions is relevant. A paper about generating Python with unit tests is not. I'll make judgment calls at the margin and document them.

## The Reading Process

I don't read every paper cover to cover. That's not feasible, and it's not necessary. I use a three-pass approach adapted from S. Keshav's "[How to Read a Paper](http://ccr.sigcomm.org/online/files/p83-keshavA.pdf)":

**Pass 1: Survey (5 minutes per paper)**
Read the title, abstract, introduction, section headings, and conclusion. Decide: is this relevant enough for Pass 2? This is where the inclusion/exclusion criteria do their work.

**Pass 2: Comprehension (30 minutes per paper)**
Read the full paper, but don't get stuck on dense proofs or implementation details. Understand the approach, the key results, and the limitations. Annotate in Zotero. Add tags.

**Pass 3: Deep read (1-2 hours per paper)**
Only for the most important papers — the ones I'll discuss in detail in the synthesis. Understand the methodology well enough to evaluate it critically. Could I reproduce this? Where does it break? How does it relate to Symbolic?

I expect roughly:
- 100-150 papers from initial search results
- 40-60 papers after Pass 1 filtering
- 15-25 papers given a deep read in Pass 3

## Tracking the Process

I'm maintaining a simple spreadsheet alongside Zotero to track the review process itself:

For each paper, I track:

- **Title and source** — where I found it (Semantic Scholar, arXiv, etc.)
- **Pass 1 date** — when I surveyed it
- **Include?** — Yes / No / Maybe
- **Pass 2 and Pass 3 dates** — if applicable
- **Key theme** — mapped to my collection categories
- **Relevance to Symbolic** — High / Medium / Low

This serves two purposes. First, it keeps me honest — I can see if I'm spending too long in the weeds or skipping important categories. Second, it makes the review auditable. If someone questions whether I considered a particular line of research, I can point to the spreadsheet.

## What I Expect to Find

I'm going in with hypotheses, not conclusions. But based on what I've already encountered tangentially, I expect the landscape to include:

**Well-explored territory:**
- LLM-based tactic prediction for Lean (LeanDojo, ReProver, and related work)
- GPT-4 and similar frontier models on mathematical reasoning benchmarks
- Autoformalization — translating informal math to formal statements

**Less-explored territory:**
- Fine-tuning smaller, open-source models for specific proof assistants
- Dataset construction for low-resource formal languages (this is my problem)
- TLA+ specifically (I suspect very little exists here)
- Reliability and failure mode analysis of LLM-generated proofs

**Open questions I'm watching for:**
- How much training data do you actually need for useful proof generation?
- Does reinforcement learning from proof checker feedback outperform supervised fine-tuning?
- Can techniques that work for Lean (rich type theory, large mathlib corpus) transfer to TLA+ (temporal logic, sparse data)?

## Why This Matters for Symbolic

I could skip all of this and go back to grinding on dataset construction. But that's how you end up building something that already exists, or worse, building something that the research community has already shown doesn't work.

The literature review will inform Symbolic in specific ways:

1. **Dataset strategy.** If the field has converged on synthetic generation over web scraping, I should know that before spending another month scraping GitHub.
2. **Model selection.** If fine-tuning 8B parameter models is a dead end for this task and the research points to other approaches, I need to know.
3. **Evaluation framework.** I'm currently measuring syntax validity and semantic validity. The field may have better metrics.
4. **Positioning.** If nobody has done this for TLA+ specifically, that's a contribution. If someone has, I need to know what they found.

## What Comes Next

The next post will be the synthesis — what I actually found. I'll organize it by theme rather than by paper, identify the key technical approaches, map the gaps, and explain how it reshapes my plan for Symbolic.

For now, the work is the unglamorous part: running queries, reading abstracts, filling out spreadsheets, and annotating PDFs. It's not as exciting as writing code, but it's how you build something that matters instead of something that already failed somewhere else.

The formal literature review starts now.
