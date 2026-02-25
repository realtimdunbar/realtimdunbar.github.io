---
layout: post
title: "Running the Real TLA+ Toolchain: What Survives SANY and TLC"
date: 2026-02-16 22:00:00 -0800
categories: [formal-methods, tla-plus, machine-learning]
tags: [tla+, sany, tlc, validation, dataset, symbolic]
---

In my [last post](/validating-tlaplus-dataset/), I collected 449 TLA+ files from GitHub and validated them down to 79 using basic structural checks — balanced brackets, module headers and footers. I reported that 52 of those 79 passed "TLC validation."

Tonight I ran the *actual* TLA+ toolchain — the SANY parser and TLC model checker — on all 79 files. The results were humbling.

> **New to this series?** Start with [From Napkin Sketch to Mathematical Proof: Introducing Symbolic](/From-Napkin-Sketch-to-Mathematical-Proof/) for the full context.

## The Gap Between Structural and Semantic Validation

My previous validation checked for things like matching `---- MODULE ----` headers and balanced parentheses. That's like checking that a Python file has proper indentation — necessary but nowhere near sufficient.

SANY (the official TLA+ parser) does full semantic analysis: operator resolution, type consistency, module dependency resolution. TLC goes further and actually model-checks the specification against its properties.

The difference matters.

## Pre-Analysis: The Dependency Problem

Before running SANY, I analyzed what each file actually needs:

```
Dependency Analysis (79 files):
  Standard-only:  12  — only uses Naturals, Integers, Sequences, etc.
  Custom deps:    55  — INSTANCE/EXTENDS modules we don't have
  No deps:        12  — fully self-contained
```

**55 out of 79 files depend on custom modules from their source repository that we never scraped.** These are mostly `MC*.tla` files — model-checking configurations that reference a main specification. For example, `MC_n4_f1.tla` from the CometBFT repo needs `TendermintAccDebug_004_draft.tla`, which we don't have.

This was predictable in hindsight. GitHub's code search returned individual files, not complete projects. We grabbed the configuration files without their dependencies.

## Results: SANY Validation

```
SANY Results (79 files):
  Passed:  18  (22.8%)
  Failed:  61  (77.2%)
```

I expected losses from the dependency problem. I did not expect nearly 80% of the dataset to fall away in a single step.

### Failure Categories

| Category | Count | What it means |
|----------|-------|---------------|
| **missing_module** | 57 | Can't find a module the file depends on |
| **semantic_error** | 2 | Valid syntax but semantic issues (duplicate definitions) |
| **sany_error** | 2 | Internal SANY errors (malformed recursive declarations) |
| **tlc_error** | 1 | Passed SANY, failed TLC |
| **success** | 17 | Passed SANY (no Spec operator, so TLC can't run) |

The dominant failure mode is clear: **72% of files fail because they reference modules we don't have.** Not because the TLA+ is wrong — because we only scraped half the project. The remaining failures — semantic errors, malformed declarations — are genuine bugs in the specs, but they're rounding errors next to the dependency problem.

## A Filename Gotcha

One lesson from tonight: SANY requires the `.tla` filename to exactly match the `MODULE` declaration inside the file. Our GitHub scraper renamed files with repo prefixes — `Aqua-218_NyxNet_Gateway.tla` contains `MODULE Gateway`.

Every single file initially failed SANY with:

```
File name 'Aqua-218_NyxNet_Gateway' does not match the name 'Gateway'
of the top level module it contains.
```

The fix: copy each file to a temp directory with the correct name before validation. A small thing, but it would have been easy to misinterpret 0/79 passing as "all our specs are broken" rather than "our filenames are wrong."

## TLC: Almost Nothing to Check

Only **1 file** in the entire dataset defines a `Spec` operator — the entry point TLC needs for model checking. That file (`MySpec.tla`) passed SANY but failed TLC.

The other 17 SANY-passing files are specifications without a runnable `Spec`. They define operators, theorems, and constants, but nothing TLC can execute. SANY-pass is the best validation we can achieve for them.

## What This Means for the Training Dataset

Let me be direct: this is a setback.

The previous post reported "52/79 passed TLC (65.8%)." That number came from the basic structural validator — my own regex-based checks — not from running actual SANY and TLC. I was measuring the wrong thing. The real numbers tell a different story:

| Validation Level | Files | Rate |
|-----------------|-------|------|
| GitHub scrape | 449 | 100% |
| Structural syntax | 79 | 17.6% |
| SANY (semantic) | 18 | 4.0% |
| TLC (model checking) | 0 | 0% |

**Zero files pass TLC.** Not one specification in the entire scraped dataset can be model-checked end to end.

And even the 18 that pass SANY aren't what they seem. Deduplication wipes out most of them — 7 are from the same NyxNet project (related config and policy modules), 3 are copies of the same Cantor diagonal proof floating around different repos, and 3 are copies of a trivial `Foo1` test spec. The truly distinct, high-quality specifications number around **8**.

To fine-tune a model you need hundreds to thousands of training examples at minimum. 8 unique specs is not a dataset — it's a handful. Training on this would produce a model that can recite Cantor's diagonal proof and NyxNet gateway configs, and nothing else.

The scraping approach that felt like it was working two weeks ago has hit a wall.

## What I'm Taking Away

This is the kind of result that makes you question an assumption you didn't realize you were making. I assumed that if a file exists on GitHub with a `.tla` extension, it's probably a usable TLA+ specification. That assumption was wrong three different ways.

### 1. Scraping individual files doesn't work for TLA+

TLA+ specifications are multi-file projects. An `MC.tla` file without its companion modules is like a `test_app.py` without the app. GitHub's code search returns individual files, and I treated each one as a self-contained example. 57 of 79 files punished me for that assumption. Future scraping needs to pull entire repository directories, not individual files.

### 2. Structural validation gave me false confidence

My regex-based validator said 52 files were good. SANY said 18. That's not a minor discrepancy — I was overestimating my dataset by nearly 3x. Balanced brackets tell you almost nothing about whether TLA+ is valid. The gap between "looks right" and "SANY accepts it" is enormous. Any future validation pipeline needs to run the real toolchain from the start, not as an afterthought.

### 3. The dataset needs a fundamentally different approach

8 unique specs is not a starting point for fine-tuning. It's a dead end. The options I'm considering:

- **Scrape complete projects** — clone full repos, resolve dependencies, validate entire project trees
- **Target the tlaplus/Examples repository** — curated, self-contained specs that are known to work
- **Generate synthetic specs** — use an LLM to produce specs, validate with SANY/TLC, keep what passes
- **Manual curation** — write specs by hand for common patterns (mutex, leader election, consensus)

Each has tradeoffs. Scraping complete projects fixes the dependency problem but adds complexity. The Examples repo is high quality but may not be large enough on its own. Synthetic generation is scalable but risks teaching a model to imitate its own mistakes. Manual curation produces the best training pairs but doesn't scale.

## Technical Notes

The validation script (`symbolic/utils/tlc_validate.py`) runs in four phases:

1. **Pre-analysis** — extracts EXTENDS, INSTANCE, Spec operators, dependency classification
2. **SANY validation** — runs `tla2sany.SANY` with 30s timeout, handles filename renaming
3. **TLC validation** — generates `.cfg`, runs `tlc2.TLC` with 60s timeout (only for Spec-having files)
4. **Results** — JSON, summary, pass/fail lists to `validation_output/tlc_validation/`

```bash
python tlc_validate.py \
  --java /opt/homebrew/opt/openjdk@17/bin/java \
  --tlc-jar ~/tla-tools/tla2tools.jar
```

Full results are in the [Symbolic repository](https://github.com/realtimdunbar/symbolic).

## What's Next

The honest answer is I need to step back and rebuild the data pipeline before anything else moves forward. The immediate plan:

1. **Clone the tlaplus/Examples repo** and run the full SANY/TLC validation pipeline on it — this should give me a baseline of known-good specs to work with
2. **Build a repo-level scraper** that clones entire TLA+ projects from GitHub instead of pulling individual files, so dependencies stay intact
3. **Re-evaluate the training strategy** — depending on how many validated specs I can collect, fine-tuning may not be the right first step. Prompt engineering with a strong base model might get further, faster, while I build up the dataset in parallel

Two weeks ago I thought I had 52 validated specs and a clear path to fine-tuning. Tonight I have 8 and a list of hard questions. That's progress — just not the kind that feels good.

---

*This is part of my ongoing work on [Symbolic](https://github.com/realtimdunbar/symbolic), an LLM-based system for generating TLA+ specifications from natural language. Previous posts: [Introducing Symbolic](/From-Napkin-Sketch-to-Mathematical-Proof/) | [Building the Dataset](/validating-tlaplus-dataset/)*

## Resources

- [TLA+ SANY Parser](https://github.com/tlaplus/tlaplus) — the official syntax/semantic analyzer
- [TLA+ Examples Repository](https://github.com/tlaplus/Examples) — curated, complete specifications
- [Symbolic Project](https://github.com/realtimdunbar/symbolic)
