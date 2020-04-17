---
title: "Alignment and mapping methodology influence transcript abundance estimation"
category:
    - Bioinformatics
permalink: /articles/2019-01-01-mappings-matter
venue: "BioXiv"
excerpt: 'We investigate the influence of mapping and alignment on the accuracy of transcript quantification in both simulated and experimental data, as well as the effect on subsequent differential expression analysis.'
date: 2018-08-14
citation: ''
---

Abstract: Background The accuracy of transcript quantification using RNA-seq data depends on many factors, such as the choice of alignment or mapping method and the quantification model being adopted. While the choice of quantification model has been shown to be important, considerably less attention has been given to comparing the effect of various read alignment approaches on quantification accuracy.

Results We investigate the influence of mapping and alignment on the accuracy of transcript quantification in both simulated and experimental data, as well as the effect on subsequent differential expression analysis. We observe that, even when the quantification model itself is held fixed, the effect of choosing a different alignment methodology, or aligning reads using different parameters, on quantification estimates can sometimes be large, and can affect downstream differential expression analyses as well. These effects can go unnoticed when assessment is focused too heavily on simulated data, where the alignment task is often simpler than in experimentally-acquired samples. We also introduce a new alignment methodology, called selective alignment, to overcome the shortcomings of lightweight approaches without incurring the computational cost of traditional alignment.

Conclusion We observe that, on experimental datasets, the performance of lightweight mapping and alignment-based approaches varies significantly and highlight some of the underlying factors. We show this variation both in terms of quantification and downstream differential expression analysis. In all comparisons, we also show the improved performance of our proposed selective alignment method and suggest best practices for performing RNA-seq quantification.