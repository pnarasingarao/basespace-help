---
layout: article
title: Metagenomics Analysis
---

The Metagenomics workflow enables the analysis of 16S ribosomal RNA, a component of the 30S subunit of prokaryotic ribosomes. The 16S ribosomal sequences from an environmental sample can be analyzed to determine which organisms are present. In MiSeq Reporter, a naïve Bayesian classifier (based on [Wang et al., Appl. Environ. Microbiol. (2007) Aug; 73(16):5261-7](http://www.ncbi.nlm.nih.gov/pubmed/17586664?dopt=AbstractPlus)) has been implemented that has been optimized for Illumina paired-end reads. Our 16S rRNA data store contains sequences from the May 2011 release of the [Greengenes 16S rRNA database](http://greengenes.lbl.gov/cgi-bin/nph-index.cgi). The main output of this workflow is a classification of reads at several taxonomic levels (kingdom, phylum, class, order, family, genus).
The Metagenomics workflow can only be used to analyze MiSeq sequencing results

The metagenomics App Results page provides one graph.

- [Clusters Graph](/articles/descriptive/workflow-graphs/#headings_10)

The Metagenomics Sample Details Page provides two panes:

- [Samples Table](/articles/descriptive/sample-details-page-components/#headings_0)
- [Metagenomics Pie Chart](/articles/descriptive/sample-details-page-components/#headings_8)
