---
title: "Accurate, fast and lightweight clustering of de novo transcriptomes using fragment equivalence classes"
category:
    - Bioinformatics
permalink: /articles/2016-04-12-rapclust-recombseq
venue: "RECOMB-Seq"
excerpt: 'We present a method for clustering contigs from de novo transcriptome assemblies based upon the relationships exposed by multi-mapping sequencing fragments.'
date: 2015-11-01
citation: 'Accurate, fast and lightweight clustering of de novo transcriptomes using fragment equivalence classes A Srivastava*, <u>H Sarkar</u>*, L Malik, R Patro - arXiv preprint arXiv:1604.03250, 2016'
---

<a href='https://arxiv.org/pdf/1604.03250'>Download PDF here</a>

Motivation: De novo transcriptome assembly of non-model organisms is the first major step for many RNA-seq analysis tasks. Current methods for de novo assembly often report a large number of contiguous sequences (contigs), which may be fractured and incomplete sequences instead of full-length transcripts. Dealing with a large number of such contigs can slow and complicate downstream analysis.

Results: We present a method for clustering contigs from de novo transcriptome assemblies based upon the relationships exposed by multi-mapping sequencing fragments. Specifically, we cast the problem of clustering contigs as one of clustering a sparse graph that is induced by equivalence classes of fragments that map to subsets of the transcriptome. Leveraging recent developments in efficient read mapping and transcript quantification, we have developed RapClust, a tool implementing this approach that is capable of accurately clustering most large de novo transcriptomes in a matter of minutes, while simultaneously providing accurate estimates of expression for the resulting clusters. We compare RapClust against a number of tools commonly used for de novo transcriptome clustering. Using de novo assemblies of organisms for which reference genomes are available, we assess the accuracy of these different methods in terms of the quality of the resulting clusterings, and the concordance of differential expression tests with those based on ground truth clusters. We find that RapClust produces clusters of comparable or better quality than existing state-of-the-art approaches, and does so substantially faster.

Comments: Co-first author