---
layout: article
title: Sample Details Page Components
---

The Sample Details Page shows metrics for a sample that the app that ran the analysis generated. Different panes are displayed on this page depending on the app.

---
##Samples table
The samples table contains general analysis information for the sample. Depending on the workflow, the following metrics can be shown:

<table class="table table-bordered">
	<tr>
		<th>Column</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>Sample name</td>
		<td>The sample name from the sample sheet</td>
	</tr>
	<tr>
		<td>Sample ID</td>
		<td>The sample ID from the sample sheet. Sample ID must always be a unique value</td>
	</tr>
	<tr>
		<td>Genome</td>
		<td>The name of the reference genome</td>
	</tr>
	<tr>
		<td>Chr</td>
		<td>The reference target or chromosome name</td>
	</tr>
	<tr>
		<td>Cluster PF</td>
		<td>The number of clusters passing filter for the sample that aligned to the reference genome</td>
	</tr>
	<tr>
		<td>Mismatch</td>
		<td>The percentage mismatch to reference averaged over cycles per read (Read 1/Read 2)</td>
	</tr>
	<tr>
		<td>No Call</td>
		<td>The percentage of bases that could not be called (no-call) for the sample averaged over cycles per read (Read 1/Read 2)</td>
	</tr>
	<tr>
		<td>Coverage</td>
		<td>Median coverage (number of bases aligned to a given reference position) averaged over all positions</td>
	</tr>
	<tr>
		<td>Het SNPs</td>
		<td>The number of heterozygous SNPs detected for the sample</td>
	</tr>
	<tr>
		<td>Hom SNPs</td>
		<td>The number of homozygous SNPs detected for the sample</td>
	</tr>
	<tr>
		<td>Insertions</td>
		<td>The number of insertions detected for the sample</td>
	</tr>
	<tr>
		<td>Deletions</td>
		<td>The number of deletions detected for the sample</td>
	</tr>
</table>

<br/>

The workflows apps Small RNA and De Novo Assembly have custom samples tables:

- [Small RNA Samples Table](/articles/descriptive/small-rna-samples-table/)
- [De Novo Assembly Samples Table](/articles/descriptive/de-novo-assembly-samples-table/)

---

##Amplicons Table

<table class="table table-bordered">
	<tr>
		<th>Column</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>#</td>
		<td>An ordinal identification number in the table</td>
	</tr>
	<tr>
		<td>Amplicons</td>
		<td>The amplicon name</td>
	</tr>
	<tr>
		<td>Location</td>
		<td>The position at which the variant was found</td>
	</tr>
	<tr>
		<td>Variants #</td>
		<td>The number of variants for this amplicon</td>
	</tr>
</table>

---

##Coverage Graph

<table class="table table-bordered">
	<tr>
		<th>Y axis</th>
		<th>X axis</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>Coverage</td>
		<td>Position</td>
		<td>The green curve is the number of aligned reads that cover each position in the reference.
		The red curve is the number of aligned reads that have a miscall at this position in the reference. SNPs and other variants show up as spikes in the red curve</td>
	</tr>
</table>

---

##Q-Score Graph

<table class="table table-bordered">
	<tr>
		<th>Y axis</th>
		<th>X axis</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>Q-Score</td>
		<td>Position</td>
		<td>The average quality score of bases at the given position of the reference</td>
	</tr>
</table>

---

##Variant Score Graph

<table class="table table-bordered">
	<tr>
		<th>Y axis</th>
		<th>X axis</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>Score</td>
		<td>Position</td>
		<td>Graphically depicts quality score and the position of SNPs and indels</td>
	</tr>
</table>

---

##Variants Table

The variants table shows variants for your sample per chromosome or amplicon.

<table class="table table-bordered">
	<tr>
		<th>Column</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>#</td>
		<td>An ordinal identification number in the table</td>
	</tr>
	<tr>
		<td>Location</td>
		<td>The position at which the variant was found</td>
	</tr>
	<tr>
		<td>Score</td>
		<td>The quality score for this variant</td>
	</tr>
	<tr>
		<td>Type</td>
		<td>The variant type, which can be either SNP or indel</td>
	</tr>
	<tr>
		<td>Call</td>
		<td>A string representing how the base or bases changed at this location in the reference</td>
	</tr>
	<tr>
		<td>dbSNP</td>
		<td>The dbSNP name of the variant, if applicable</td>
	</tr>
	<tr>
		<td>RefGene</td>
		<td>The gene according to RefGene in which this variant appears</td>
	</tr>
	<tr>
		<td>Frequency</td>
		<td>The fraction of reads for the sample that includes the variant. For example, if the reference base is A, and sample 1 has 60 A reads and 40 T reads, then the SNP has a variant frequency of 0.4</td>
	</tr>
	<tr>
		<td>Depth</td>
		<td>The number of reads for a sample covering a particular position. The GATK variant caller subsamples data in regions of high coverage</td>
	</tr>
	<tr>
		<td>Filter</td>
		<td>The criteria for a filtered variant</td>
	</tr>
</table>

---

##Small RNA Pie Chart

The Small RNA pie chart provides a visualization of clusters identified as mature miRNA, other forms of RNA, genomic sequence, or contaminants.
Common categories for the Small RNA pie chart are as follows:

- Unaligned	clusters that did not align against any reference
- Genome	clusters that aligned to the reference genome
- miRNA clusters that aligned to the mature miRNA database

Hits to the mature miRNA database are counted only if the cluster aligned to the correct strand and position for the mature miRNA.

The remaining category names in the Small RNA pie chart are taken from the FASTA file names in the databases. For example, if the RNA database contains a file named rRNA.fa, then matches to this file are reported as the category **rRNA**.

---

##Small RNA Graph

The Small RNA graph provides a plot of the common mature miRNA sequences for a sample and their abundances. The most common miRNA sequences for the selected sample (up to 10 records) are shown in proportion to the number of clusters matched.

---

##Metagenomics Pie Chart

The Metagenomics pie chart provides a visualization of how many clusters from each sample were assigned to a category in each taxonomic level.
Click another row in the taxonomy table to change the pie chart to that sample or taxonomic level.

---

##Assembly Samples Graph

Contigs are arranged end-to-end along the X axis and the reference chromosomes are arranged bottom-to-top along the Y axis. Each pixel of the plot is colored according to how many short sequences of the corresponding contig have a match in the corresponding portion of the reference genome.

An identical assembly results in a diagonal line. A vertical gap in the plot might indicate a portion of the reference that is absent in the assembly, such as a plasmid, which is found in some bacteria populations.

<table class="table table-bordered">
	<tr>
		<th>Y axis</th>
		<th>X axis</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>Reference</td>
		<td>Assembly Position</td>
		<td>A syntenic plot of assembled contigs compared to a reference. A reference genome must be specified in the sample sheet</td>
	</tr>
</table>

---

##Sample QC Table

<table class="table table-bordered">
	<tr>
		<th>Column</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>Sample Name</td>
		<td>The sample name from the sample sheet</td>
	</tr>
	<tr>
		<td>Clusters Count</td>
		<td>The number of clusters sequenced for this sample</td>
	</tr>
	<tr>
		<td>Clusters Percentage</td>
		<td>The percentage of the total cluster number matching the index for this sample</td>
	</tr>
	<tr>
		<td>Pass Filter</td>
		<td>The percentage of clusters passing filter for this sample</td>
	</tr>
	<tr>
		<td>Alignment R1/R2</td>
		<td>The percentage of clusters successfully aligned in Read 1/ Read 2</td>
	</tr>
	<tr>
		<td>Length Median</td>
		<td>The median fragment length for the sample</td>
	</tr>
	<tr>
		<td>Length Min</td>
		<td>The low percentile of fragment lengths for this sample as they correspond to three standard deviations from the median</td>
	</tr>
	<tr>
		<td>Length Max</td>
		<td>The high percentile of fragment lengths for this sample as they correspond to three standard deviations from the median</td>
	</tr>
	<tr>
		<td>Mismatch R1/R2</td>
		<td>The percentage mismatch to reference averaged over cycles per read (Read 1/Read 2)</td>
	</tr>
	<tr>
		<td>Estimated Diversity</td>
		<td>An estimate of the total library diversity derived from the observed diversity and the number of apparent PCR duplicates. This calculation is available for paired-end runs unless PCR duplicate flagging was disabled in the sample sheet</td>
	</tr>
	<tr>
		<td>Observed Diversity</td>
		<td>Number of distinct aligned positions. Reads with the same aligned positions are assumed to be PCR duplicates. PCR duplicates are defined as sequences with identical Read 1 and Read 2 start sites</td>
	</tr>
</table>