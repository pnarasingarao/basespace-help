---
layout: article
title: Analysis VCF entries
---

The VCF files for Analysis can have the following entries in the FILTER, FORMAT, and INFO fields:

---

**Table**: VCF FILTER Entries
<table class="table table-bordered">
	<tr>
		<th>Entry</th>
		<th>DEscription</th>
	</tr>
	<tr>
		<td>IndelConflict</td>
		<td>Locus is in region with conflicting indel calls</td>
	</tr>
	<tr>
		<td>SiteConflict</td>
		<td>Site genotype conflicts with proximal indel call, typically a heterozygous SNV call made inside of a heterozygous deletion</td>
	</tr>
	<tr>
		<td>LowGQX</td>
		<td>Locus GQX is less than 30 or not present</td>
	</tr>
	<tr>
		<td>HighDPFRatio</td>
		<td>The fraction of base calls filtered out at a site is greater than 0.4</td>
	</tr>
	<tr>
		<td>HighSNVSB</td>
		<td>SNV strand bias value (SNVSB) exceeds 10</td>
	</tr>
	<tr>
		<td>HighDepth</td>
		<td>Locus depth is greater than 3x the mean chromosome depth</td>
	</tr>
	<tr>
		<td>OffTarget</td>
		<td>Variant is not on target</td>
	</tr>
</table>

---

**Table**: VCF FORMAT Entries
<table class="table table-bordered">
	<tr>
		<th>Entry</th>
		<th>DEscription</th>
	</tr>
	<tr>
		<td>GQX</td>
		<td>Minimum of {Genotype quality assuming variant position,Genotype quality assuming non-variant position}</td>
	</tr>
	<tr>
		<td>GT</td>
		<td>Genotype</td>
	</tr>
	<tr>
		<td>GQ</td>
		<td>Genotype Quality</td>
	</tr>
	<tr>
		<td>DP</td>
		<td>Filtered base call depth used for site genotyping</td>
	</tr>
	<tr>
		<td>DPF</td>
		<td>Base calls filtered from input before site genotyping</td>
	</tr>
	<tr>
		<td>AD</td>
		<td>Allelic depths for the ref and alt alleles in the order listed. For indels, this value only includes reads that confidently support each allele (posterior probability 0.999 or higher that read contains indicated allele vs all other intersecting indel alleles)</td>
	</tr>
	<tr>
		<td>DPI</td>
		<td>Read depth associated with indel, taken from the position preceding the indel.</td>
	</tr>
</table>

---

**Table**: VCF INFO Entries
<table class="table table-bordered">
	<tr>
		<th>Entry</th>
		<th>DEscription</th>
	</tr>
	<tr>
		<td>SNVSB</td>
		<td>SNV site strand bias</td>
	</tr>
	<tr>
		<td>SNVHPOL</td>
		<td>SNV contextual homopolymer length</td>
	</tr>
	<tr>
		<td>CIGAR</td>
		<td>CIGAR alignment for each alternate indel allele</td>
	</tr>
	<tr>
		<td>RU</td>
		<td>Smallest repeating sequence unit extended or contracted in the indel allele relative to the reference. RUs longer than 20 bases are not reported.</td>
	</tr>
	<tr>
		<td>REFREP</td>
		<td>Number of times RU is repeated in reference.</td>
	</tr>
	<tr>
		<td>IDREP</td>
		<td>Number of times RU is repeated in indel allele.</td>
	</tr>
	<tr>
		<td>END</td>
		<td>End position of the region described in this record</td>
	</tr>
	<tr>
		<td>BLOCKAVG_min30p3a</td>
		<td>Non-variant site block. All sites in a block are constrained to be non-variant, have the same filter value, and have all sample values in range [x,y], y <= max(x+3,(x*1.3)). All printed site block sample values are the minimum observed in the region spanned by the block</td>
	</tr>
</table>