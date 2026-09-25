# Essay notes
## 1. Assesses each of the four FAIR principles 
against concrete, specific evidence drawn from the paper and its associated dataset. You should access the paper’s data repository (GEO, ENA, or SRA) directly as part of your assessment.

### FINDABILITY
Finding the dataset: 
- [FAIL] Can you locate the dataset by searching the GEO or ENA database without knowing the accession in advance?
No, when looking for the dataset in SRA in NCBI, I was not able to locate the dataset with combinations of key words from the article's title and author names. When looking at the PRJNA526458 BioProject, it is visible that the title differs completely from the title of the published article:
"soil metagenome - Soil water stress history wheat microbiome"
This contains key words, but out of the various results from the keyword search, it was not at all clear that this is the one. The only info linkind it loosely to the article is the registration date and who it was registered from (just says "National Research Council Canada, Genome Quebec Innovation Centre"). This could have helped for a risky guess, but no stable conclusion.


- [PASS] Is there a persistent, stable identifier (accession number) for the dataset mentioned in the paper? 
Yes, in the Data Availablity statement the BioProject accession PRJNA526458 is specified for the raw sequencing data sets available at SRA, the Sequence Read Archive at NCBI.

### ACCESSIBILITY
- Can Data be downloaded via standardized protocol?
Yes, data can be downloaded via standart URL protocl (NCBI ENTREZ API), or even easier and better with th sam-tools.

**For this I used Claude for help to find the right commands and understand the NCBI data hierarchy.**

Yes, and they can be downloaded either individually from SRA by searching the sequence run with the SRS ID in SRA, or via the command line. Here, the edirect command-line tools can be installed and used. 

1. The most efficient way to download it from the command line it via the prefetch command in SRA's tools, which allows to pull down every run associated to a specific Bioproject:
 
    prefetch PRJNA123456

2. Alternatively, one can get all SRR IDs from a Bioproject with: 

    esearch -db sra -query "PRJNA123456" | efetch -format runinfo > runinfo.csv

The output can then be used in a script to get all/specific FASTQ files:

    efetch -db sra -id <ID1>,<ID2> -format runinfo > runinfo.csv


3. efetch for single specific run -> can't retrieve FASTA sequences directly from SRA database (because it stores raw sequencing reads in specialised compressed SRA format). It can only retrieve metadata via the following command:

    efetch -db sra -id SRR8743463 -format runinfo > runinfo.txt

The NCBI hierarchiy looks as follows: BioProject (PRNJA...) -> Biosamples (SAMN...) -> SRA Experiments (SRX...) -> SRA runs (SRR...).



- Evaluate the mechanism of access: protocol, stability, documentation. Assess quality of restricted-access process.

- Distinguish between raw and processes data availability
There is only raw data available, as the “SRA normalized format” contains the usual raw SRA data, but with full quality score (SRA formats link). 

- Would metadata persist if data where withdrawn?

### INTEROPERABILITY
Data availability: 
- [PASS] Are raw FASTQ files available (not only processed count matrices)? 
Yes!

- [PASS] Are processed results available (count matrix, normalised data)? 
No, there is the "SRA Normalized Format" available, but it contains the raw ouput with full base quality scores - so no processed output. However, the normalisation method is neatly described in the article. 


### REUSABILITY - Q2 & Q3

#### 2. Evaluates whether the metadata is sufficient for reuse 
specifically, whether you would have enough information to reproduce a differential expression (or equivalent) analysis from the deposited data alone, without contacting the authors.

Sample identification: 
- [PASS] Can you tell how many samples are in the dataset? 

Yes, there are 950 samples. 

- [FAIL] Can you assign each sample to an experimental condition (control vs treated, genotype A vs B, etc.) from the metadata alone? 

Not really, there is no field for that in the sample list and no metadata header/description that mentions anything like that. But looking closely at the sample names, some information could be made out. The sample name has the following structure: WORD1_WORD2_WORD3_NR1_NR2_GENE:

    - WORD1: ROOT, LEAF, Soil                  <- don't understand because they are all soil samples (from rhizosphere)
    - WORD2: Bar, STR, ST NAS, Wal, WA         <- probably genotypes: Barrie, Strongfield, Nass, Walton = **inconstistent, repetitive and therefore not Ontology-based!**
    - WORD3: IR, DL                            <- irrigated vs non-irrigated? **Not consistent wit article, there it is IS and NI**

    - NR1: 5, 20, 30, 50                       <- SWHC 

    - NR2: 1-5                                 <- Biological replicate within group? 

    - GENE: 16S or ITS

Things to improve: add info for each sample in the sample list by adding the following fields: irrigated vs non irrigated irrigation history, wheat genotype/"T0", Soil Water Holding Capacity. Moreover, a header / introduction to the metadata should be included, just shortly describing the sample experimental conditions.

- [FAIL] Are biological replicates identifiable?

No, given that clear metadata is missing. By guessing the meaning of the sample names as above, one could deem samples with the same Genotype but differences in the rest of the name biological replicates.


Biological metadata: 
- [PASS] Is the organism identified with a taxonomy ID (not just a name)? 

Yes! In this case, the organism is "soil metagenome" and the according Taxonomy ID is 410658.

- [FAIL] Is the treatment or experimental condition specified? Is it annotated with an ontology term (e.g. EFO, CHEBI)? 

No, I could just guess my way to it (see above) - no ontological term in sight.

- [ ] Are any other relevant variables specified (age, sex, genotype, growth conditions, time point)?

No, although that would be relevant.

Technical metadata: 
- [PASS] Is the sequencing platform specified? 

Yes, it is IlluminaMiSeq.

- [ ] Is the library preparation protocol specified (stranded/unstranded, poly-A/ribo-depleted)? 

Yes, amplicon.

- [PASS] Is read length and paired/single end specified?

It is specified that it is paired and the read length seems to be 250 - it says so on the SRA page.



-> There is no metadata about what the logic behind the samples are on SRA! There is some info, but no metadata on the logic, grouping, experimental condition, etc. of the samples! 


#### 3. Assesses the reproducibility of the computational analysis 
are software versions specified? Are analysis parameters documented? Is code available, and if so, is it versioned and documented?
 
- [FAIL] Is analysis code available? 

No.

- [PARTIAL] Are software versions specified for all tools used? 

SOME

- [FAIL] Is there a workflow definition (Snakemake, Nextflow) or equivalent? 

No.

- [FAIL] Does the paper declare adherence to a community metadata standard (e.g. MINSEQE)?

No.

- [ ] Is the alignment tool and version specified?

**Analysis pipeline**

The following methods are described in supplementary file 'Data Sheet 1.doxc'. For the quality trimming (first 5 lines), informations about the pipeline used were fetched from a referenced article in the supplementary file (Tremblay et al., 2015). The remaining steps seem to differ from this referenced article and are described in detail in the supplementary file. Hence, I took the information from the supplementary file.

| | Step    |   Tool    |   Version?    |   Parameters reported?  |    found where? |
|-|--------|-----------|---------------|-------------------------|-----------------|
| 1.| quality trimming | JGI Itag analysis pipeline - includes tools described below | - | none reported | Tremblay et al. (2015)|
| 2.| paired end read assembly | FLASH software | article | no | Tremblay et al. (2015) |
| 3.| removing common sequence contaminants and PhiX spike-in reads | kmer matching tool DUK | - | no| Tremblay et al. (2015) |
| 4.| trimming assembled amplicons to remove reverse primer sequences | in-house PERL scripts | - | no | Tremblay et al. (2015) |
| 5.| filtering amplicon sequences | ? | - | yes, values for lenient and stringent quality control parameters | Tremblay et al. (2015)|
|--------|-----------|---------------|-------------------------|------------------------|
| 6.| dereplication & clustering | DNAclust | v3 | dereplicated at 100% identity, clustered at 99% | supplementary file|
| 7.| chimera scanning | UCHIME | article | de novo mode, reference mode | reference is missing in supplementary file |
| 8.| clustering | DNAclust | v3 | 97% identity | supplementary file|
| 9.| taxonomy assignment 16S (bacteria) | RDP classifier | article | modified Greengenes training set built from a concatenation of the Greengenes db v13_5, and Silva eukaryotes 18S r128 | supplementary file, details db in references (DeSantis et al., 2006, Quast et al., 2013) |
| 10.| taxonomy assignment ITS (fungi) | RDP classifier | article | training set generated from the Unite database | supplementary file, details db in references (Kõljalg et al., 2013) |

Looking at the table, I see some unclear parts:

The most tricky part would most porbably be step 7, as the article referenced to is missing in the references. Moreover, any steps that don't mention parameters could be hard to replicate (steps 2, 3, 4 and therefore step 1; also step 9 and 10 for the parameters of the RDP classifier itself other than the training sets used). Aditionally, the datasets mentioned in steps 9 and 10 have to be further investigated to see if those steps can be fully replicated. 

Depending on how heavily they rely on other paper, it might not be very fair compliant - also depends on how FAIR-compliant other article is. 

If a tool only references an article and no explicit version, is this ok? Have to check whether it is a versioned tool or not -> if it does not say so, it is ok (because if it gets updated without mentioning it, the problem is with the programmers, not the article authors)

The supplementary file refers articles that point to the web pages / web services used. Only in one out of three I could find the dataset referenced. I am unsure whether there are actual issues with the finability of the datasets or if it is just me not undertanding the webpages and terms used in their file/directory system. What I can say is that they are not easily findable, but not that a professional in the field would not be able to find them after investing some time.

They point to webpages, which is good. On SILVA, finding the release is crucial - if you know what you're doing then you can get on from there. GreenGene: old webpage, good that it even exists; data is there, so if you really wanted to find it you could.

_________________________________________________________________________________________________________________
## 4. Makes specific, concrete, evidenced recommendations 
for what could have been done differently to improve FAIR compliance. These should be actionable and specific — not “better metadata” but “the GEO submission should have included EFO ontology terms for tissue type and treatment, specifically UBERON:0002368 for adrenal medulla and CHEBI:41879 for dexamethasone.”


