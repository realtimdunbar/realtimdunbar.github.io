---
title: "What I Have Been Up To"
author: "Tim Dunbar"
date: "February 3, 2026"
layout: post
excerpt: "A retrospective on eight years of professional and personal growth: completing a Master's in Computer Science at Georgia Tech, advancing to Director of Data Engineering, and pivoting toward AI architecture, formal methods, and quantum computing applications."
---

## Introduction

It's been over eight years since my last post here in August 2017. During that time, the world changed dramatically—we lived through a global pandemic, witnessed fundamental shifts in how we work and communicate, and saw artificial intelligence move from research labs into everyday tools. On a personal level, these years brought significant transitions: completing graduate school, advancing in my career, becoming an empty-nester, and relocating to Florida.

This post serves as a retrospective on the professional and personal growth that occurred during this period, and more importantly, sets the stage for where I'm heading next. After years of building production data systems and completing formal training in computer science, I'm now focusing on the intersection of artificial intelligence, formal methods, and quantum computing—areas that bridge theoretical computer science with practical systems engineering.

---

## Background

The past eight years encompassed major life transitions. My youngest child moved out, marking the transition to being empty-nesters. We relocated from Virginia to Clermont, Florida, seeking a change of pace and climate. There were the usual challenges—a car accident with a drunk driver, family moving in, the various emergencies and complexities that come with homeownership. Through it all, I maintained focus on professional development and continued exploring the mathematical and computational ideas that have fascinated me since my undergraduate studies.

Outside of work and study, I've remained active in Toastmasters International since 2018, developing communication and leadership skills. I continue to find creative expression through blues music, playing both guitar and harmonica—a reminder that not everything needs to be about logic and computation.

---

## Education

In 2021, I began the Master of Science in Computer Science program at Georgia Institute of Technology, completing it in 2025 with a 3.81 GPA. This was a rigorous program that allowed me to formalize knowledge I'd gained through years of practical experience while diving deep into areas I'd only explored superficially before.

**Key Areas of Focus:**

- **Artificial Intelligence**: Advanced coursework in machine learning, natural language processing, and knowledge representation
- **Quantum Computing**: Specialized study in quantum algorithms and their applications
- **Formal Methods**: Training in formal verification, model checking, and correctness proofs

**Research Highlights:**

My most significant academic work involved simulating molecular systems using quantum computers, specifically extending the CAFQA (Clifford Ansatz For Quantum Accuracy) framework. This research sits at the intersection of quantum chemistry, quantum computing, and computational physics.

**The Problem:** Classical computers struggle to simulate quantum mechanical systems accurately due to exponential scaling—simulating n quantum particles requires computational resources that grow as 2^n. Quantum computers can simulate these systems more naturally, but current NISQ devices are limited by noise and gate errors.

**My Contribution:** The CAFQA approach uses Clifford gates (a restricted set of quantum gates) to build quantum circuits for molecular simulation. While Clifford circuits are easier to implement and more noise-resilient, they have limited expressiveness. My research focused on augmenting the traditional Clifford gate set with T gates to determine if this could achieve additional accuracy not realized by CAFQA alone.

**Why This Matters:** T gates are non-Clifford gates that add computational power to quantum circuits, allowing them to represent more complex quantum states. However, they're also more difficult to implement on real quantum hardware and more susceptible to noise. The research question: does the increased expressiveness of Clifford+T circuits outweigh the additional error introduced by T gates for molecular simulation tasks?

The work involved:
- Implementing quantum circuits with hybrid Clifford+T gate sets
- Comparing simulation accuracy against pure Clifford approaches (baseline CAFQA)
- Analyzing the trade-off between circuit expressiveness and noise resilience
- Benchmarking on small molecular systems (H₂, LiH, BeH₂)
- Evaluating performance on NISQ hardware with realistic error rates

This research reinforced a key insight: the most interesting problems exist at the boundaries between disciplines. Quantum chemistry isn't just physics—it's a computational problem that requires expertise in algorithms, hardware limitations, and careful trade-off analysis between theoretical capability and practical implementation constraints.

**Broader Training:**

Beyond the graduate program, I maintained continuous learning through various certifications and courses:
- Data Science at Scale specialization (Coursera, 2017)
- Practical Predictive Analytics (Coursera, 2017)
- Build a Modern Computer from First Principles (Coursera, 2016)
- Multiple certifications in R, Python, and data manipulation

---

## Movement

In 2024, we relocated from Virginia to Clermont, Florida. The move represented both a lifestyle change and a practical decision—lower cost of living, better weather, and proximity to growing tech communities in Orlando and Tampa. Working remotely as Director of Data Engineering made the geographic transition seamless professionally, while personally it offered a fresh start after years of intense focus on graduate school and career advancement.

Florida's emerging tech scene has been a pleasant surprise. While not Silicon Valley or Austin, the state has been attracting significant tech investment, particularly in aerospace (Cape Canaveral's private space industry), defense contractors, and enterprise software companies. The cost-of-living arbitrage allows for a better quality of life while maintaining the same professional standards and compensation.

---

## Work

My professional trajectory over the past eight years has been one of increasing scope and technical depth. I currently serve as **Director of Data Engineering at Trader Interactive**, where I lead initiatives at the intersection of data architecture, systems design, and intelligent automation.

**Professional Evolution:**

When I last posted in 2017, I was deep in data science and analytics—building predictive models, running statistical analyses, and working primarily with structured datasets. The field has evolved dramatically since then:

- **Infrastructure as Code**: Data engineering now resembles software engineering more than statistics. We build pipelines using modern tooling—Airflow, dbt, Terraform—treating data infrastructure with the same rigor as application code.

- **Real-Time Systems**: Batch processing has given way to streaming architectures. We've built systems that process millions of events per day using Kafka, Spark Streaming, and Lambda architectures.

- **ML Operations**: Machine learning moved from Jupyter notebooks to production systems. This required building deployment pipelines, monitoring systems, and governance frameworks—bridging the gap between data science and platform engineering.

- **Cloud-Native Architecture**: Migration from on-premise data centers to cloud infrastructure (primarily AWS) changed how we think about scalability, cost optimization, and system design.

**Key Accomplishments:**

- **D.R.I.V.E. Award (2021)**: Led the AVBT project team, recognized for innovation in data-driven decision making
- **Data Platform Modernization**: Architected and led the migration from legacy ETL systems to modern ELT patterns using cloud-native tools
- **Team Building**: Grew and mentored a team of data engineers, establishing best practices for code review, testing, and documentation
- **Cross-Functional Leadership**: Bridged gaps between data science, analytics, software engineering, and business stakeholders

**Technical Philosophy:**

Over these years, I've developed a perspective on data engineering that emphasizes:

1. **Correctness over Speed**: Data pipelines should be provably correct. Late data is annoying; wrong data is catastrophic.
2. **Simplicity over Cleverness**: Complex systems fail in complex ways. Simple, well-documented systems are easier to debug, maintain, and extend.
3. **End-to-End Ownership**: Data engineers should understand both the source systems generating data and the downstream use cases consuming it.
4. **Automation with Guardrails**: Automate everything, but build validation and monitoring into every step.

This philosophy is increasingly influenced by formal methods and correctness proofs—concepts I encountered in graduate school that have direct applications to production data systems.

---

## What's Next

After years of building data infrastructure and completing formal computer science training, I'm pivoting toward three interconnected areas that represent the future of reliable, intelligent systems:

### 1. AI Architecture and LLM Systems

Large language models have moved from research curiosities to production tools in just a few years. However, most organizations are still figuring out how to deploy them reliably. I'm particularly interested in:

- **Fine-tuning and specialization**: Adapting open-source models (Llama, Mistral) for domain-specific tasks where GPT-4 falls short
- **Retrieval-Augmented Generation (RAG)**: Building systems that ground LLM outputs in verifiable data sources
- **LLM reliability**: Developing validation frameworks that catch hallucinations, ensure consistency, and provide confidence scores
- **Cost optimization**: Balancing model capability against inference costs—when to use 70B models vs. 7B models vs. prompt engineering

**Current Project**: I'm building Symbolic, a system that uses fine-tuned LLMs to generate formally verified specifications. This combines practical ML engineering with theoretical computer science, addressing the fundamental problem of AI reliability.

### 2. Formal Methods and Verification

The software industry has largely relied on testing to ensure correctness: write code, write tests, hope you covered the important cases. Formal methods offer a different approach: mathematically prove that systems behave correctly under all possible conditions.

**Why This Matters Now:**

As systems become more complex—distributed databases, consensus algorithms, concurrent systems—the state space becomes too large to test exhaustively. A mutex with two processes has dozens of possible interleavings. With ten processes, it's millions. Testing samples the state space; formal verification proves properties across the entire space.

Companies like AWS, Microsoft, and MongoDB are already using formal methods (primarily TLA+) to verify critical systems. I believe this will become standard practice, not just for infrastructure companies, but for any organization building safety-critical or financially significant systems.

**Areas of Focus:**

- **TLA+ and model checking**: Specifying and verifying distributed systems, consensus protocols, and concurrent algorithms
- **Theorem provers**: Exploring Coq, Lean, and other proof assistants for software verification
- **Accessibility**: Making formal methods approachable for working engineers (hence the Symbolic project)

### 3. Quantum Computing Applications

My graduate research in quantum simulation of molecular systems opened my eyes to both the promise and the current limitations of quantum computing. We're in the NISQ (Noisy Intermediate-Scale Quantum) era—quantum computers exist and work, but they're noisy, have limited qubits, and can't yet outperform classical computers for most problems.

**Realistic Near-Term Applications:**

- **Quantum chemistry**: Simulating molecular systems for drug discovery and materials science
- **Optimization problems**: Exploring quantum annealing and variational algorithms for combinatorial optimization
- **Quantum machine learning**: Investigating whether quantum computers can accelerate specific ML workloads

**What I'm Watching:**

- Error correction progress (we need ~1000 physical qubits per logical qubit currently)
- Algorithm development for NISQ devices
- Hybrid quantum-classical approaches that leverage the strengths of both

I don't expect quantum computers to replace classical systems broadly, but there are specific domains—particularly in simulation and optimization—where they may provide exponential advantages.

---

## Conclusion

The past eight years have been transformative both personally and professionally. Graduate school provided formal training in areas I'd explored informally for years. My career evolved from data science and analytics to data engineering and systems architecture. I've moved from building models to building the platforms that enable others to build models.

The next phase focuses on reliability and correctness in AI systems—combining practical experience in production engineering with theoretical foundations in formal methods and quantum computing. The goal: build systems that aren't just intelligent, but provably correct.

This blog will return to active use, documenting this journey. Expect deep dives into:
- LLM fine-tuning and productionization
- Formal verification techniques for software systems
- Quantum algorithms and their practical applications
- The intersection of AI and formal methods

The world has indeed moved on since 2017. But the fundamental questions remain: How do we build systems that work correctly? How do we make AI reliable? How do we bridge theory and practice? These questions will guide the next chapter.

---

*If you're working on similar problems—AI reliability, formal methods, quantum applications—I'd love to connect. Reach out at [timothy.c.dunbar@me.com](mailto:timothy.c.dunbar@me.com).*