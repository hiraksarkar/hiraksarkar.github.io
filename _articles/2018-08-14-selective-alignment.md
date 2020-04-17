---
title: "Towards selective-alignment: Bridging the accuracy gap between alignment-based and alignment-free transcript quantification"
category: 
    - Bioinformatics
permalink: /articles/2018-08-14-selective-alignment
venue: "ACM-BCB"
excerpt: 'We introduce an algorithm for selectively aligning high-throughput sequencing reads to a transcriptome, with the goal of improving transcript-level quantification in difficult or adversarial scenarios.'
date: 2018-08-14
citation: '<u>Sarkar, Hirak</u>*, Mohsen Zakeri*, Laraib Malik, Rob Patro Proceedings of the 2018 ACM International Conference on Bioinformatics, Computational Biology, and Health Informatics. 2018.'
---

<a href='https://dl.acm.org/doi/pdf/10.1145/3233547.3233589'>Download PDF here</a>

Abstract: We introduce an algorithm for selectively aligning high-throughput
sequencing reads to a transcriptome, with the goal of improving transcript-level quantification in difficult or adversarial scenarios. This algorithm attempts to bridge the gap between fast
non-alignment-based algorithms and more traditional alignment
procedures. We adopt a hybrid approach that is able to produce
accurate alignments while still retaining much of the efficiency of
non-alignment-based algorithms. To achieve this, we combine edit distance-based verification with a highly-sensitive read mapping
procedure. Additionally, unlike the strategies adopted in most aligners which first align the ends of paired-end reads independently,
we introduce a notion of co-mapping. This procedure exploits relevant information between the “hits” from the left and right ends
of paired-end reads before full mappings for each are generated,
improving the efficiency of filtering likely-spurious alignments. Finally, we demonstrate the utility of selective alignment in improving
the accuracy of efficient transcript-level quantification from RNA-seq reads. Specifically, we show that selective-alignment is able
to resolve certain complex mapping scenarios that can confound
existing non-alignment-based procedures, while simultaneously
eliminating spurious alignments that fast mapping approaches can
produce. Selective-alignment is implemented in C++11 as a part
of Salmon, and is available as open source software, under GPL v3, at:
https://github.com/COMBINE-lab/salmon/tree/selective-alignment

Comments: This leads to major improvement of Salmon