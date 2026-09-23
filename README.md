# Essay notes
## 1. Assesses each of the four FAIR principles 
    against concrete, specific evidence drawn from the paper and its associated dataset. You should access the paper’s data repository (GEO, ENA, or SRA) directly as part of your assessment.

### FINDABILITY
Finding the dataset: 
- [ ] Can you locate the dataset by searching the GEO or ENA database without knowing the accession in advance? 

- [ ] Is there a persistent, stable identifier (accession number) for the dataset? 

- [ ] Is the identifier cited in the paper?

- [ ] Is the reference genome/transcriptome version specified? 

### ACCESSIBILITY


### INTEROPERABILITY
Data availability: 
- [ ] Are raw FASTQ files available (not only processed count matrices)? 

- [ ] Are processed results available (count matrix, normalised data)? 


### REUSABILITY - Q2 & Q3

#### 2. Evaluates whether the metadata is sufficient for reuse 
    specifically, whether you would have enough information to reproduce a differential expression (or equivalent) analysis from the deposited data alone, without contacting the authors.

Sample identification: 
- [ ] Can you tell how many samples are in the dataset? 

- [ ] Can you assign each sample to an experimental condition (control vs treated, genotype A vs B, etc.) from the metadata alone? 

- [ ] Are biological replicates identifiable?


Biological metadata: 
- [ ] Is the organism identified with a taxonomy ID (not just a name)? 

- [ ] Is the tissue or cell type specified? Is it annotated with an ontology term (e.g. UBERON, EFO, CL)? 

- [ ] Is the treatment or experimental condition specified? Is it annotated with an ontology term (e.g. EFO, CHEBI)? 

- [ ] Are any other relevant variables specified (age, sex, genotype, growth conditions, time point)?


Technical metadata: 
- [ ] Is the sequencing platform specified? 

- [ ] Is the library preparation protocol specified (stranded/unstranded, poly-A/ribo-depleted)? 

- [ ] Is read length and paired/single end specified?



#### 3. Assesses the reproducibility of the computational analysis 
    are software versions specified? Are analysis parameters documented? Is code available, and if so, is it versioned and documented?
 
- [ ] Is analysis code available? 

- [ ] Are software versions specified for all tools used? 

- [ ] Is there a workflow definition (Snakemake, Nextflow) or equivalent? 

- [ ] Does the paper declare adherence to a community metadata standard (e.g. MINSEQE)?

- [ ] Is the alignment tool and version specified?

_________________________________________________________________________________________________________________
## 4. Makes specific, concrete, evidenced recommendations 
    for what could have been done differently to improve FAIR compliance. These should be actionable and specific — not “better metadata” but “the GEO submission should have included EFO ontology terms for tissue type and treatment, specifically UBERON:0002368 for adrenal medulla and CHEBI:41879 for dexamethasone.”


