---
title: "Biotech Patent Knowledge Graph with Sequence Retrieval"
description: "knowledge graph for biotech patents: POS-tagging NLP extraction, ETL, BLAST+ taxonomy validation, and internal retrieval system over structured sequences."
date: 2021-02-01T00:00:00Z
lastmod: 2026-07-05
featured: false
cover:
    image: "/assets/images/projects/ipwe.jpeg"
    style: "logo"
    logoWide: true
    alt: "Biotech patent knowledge graph project"
    relative: false
---

As team lead, I spearheaded the development of a knowledge graph for biotech patent literature—defining the research direction, aligning delivery milestones, and overseeing data pipeline and schema architectures. The project's core goal was to transform unstructured patent text into structured genetic sequence records to power downstream IP analytics and interactive exploration.

{{< figure-block src="/assets/images/projects/ipwe_highlight.png" alt="POS tagging genetic extraction pipeline" caption="POS tagging and domain rules to locate sequence-bearing passages in biotech patent documents" >}}

The team built an automated ETL pipeline to extract gene names, DNA/protein sequences, and taxonomic mentions from patent documents. These extracted entities were integrated into a unified knowledge graph schema and validated against BLAST+ databases for taxonomic classification—filtering out noise caused by OCR errors and inconsistent biological nomenclature.

On the text-mining side, the solution combined part-of-speech (POS) tagging with domain-specific rules to isolate sequence-bearing passages within patent claims and descriptions—a strategy tailored to the complex, semi-structured nature of legal-biotech prose compared to standard biomedical abstracts.

To facilitate search and exploration, the project implemented an internal retrieval system allowing queries over extracted sequences and associated entities to surface matching or similar graph nodes. This structured search capability serves as a vital complement to traditional keyword search, accelerating prior-art exploration and sequence-overlap workflows.

This project validated the pipeline end-to-end, paving the way for data-driven product decisions in life-sciences IP analytics.
