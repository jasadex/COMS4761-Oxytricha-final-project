# Demonstration_on_3_candidate_loci.ipynb:

- Jupyter notebook that contains iterative de Bruijn graph helper functions, implementation, and compressed visualization repeated sequentially for the three candidate loci. Comments within the code largely walk you through what each item is doing and how to modify it if necessary. Also creates a Pyro Bayesian model to score the validity of an edge in a de Bruijn graph. It converts the loci data needed for priors into Pytorch tensors, uses Autoguide to update the predicted posterior distributions while training the model, and displays the final predicted posterior distributions as both slope and intercept graphs. The last part of the notebook calculates and compares the sum of the scores of every edge in each path in a de Bruijn graph. 
- Undefined terms:
  - rc = reverse complement
  - DBG = de Bruijn graph
  - Hk = one iteration of the de Bruijn graph construction at a given k
  - MDSs = gene segments (retained in the product)
  - Fastq_36 = FASTA of sequencing reads from cells harvested 36 hours into development
  - Fastq_48 = FASTA of sequencing reads from cells harvested 48 hours into development
  - MIC = precursor form of the genome (ie: MIC_reference_seq is reference sequence of the precursor form of the gene of a gene of interest, whereas locus_reference_seq is the reference sequence of the product form of that respective gene)
  - MAC = product form of the genome
  - IES = internally eliminated sequence, or sequence intervening retained gene segments in the precursor locus that must get removed to create the product form of the gene
 
- How to test run: You need to first download and upload the .ipynb file as well as the data-containing files to a Google Drive folder, with the latter files being in a subfolder that matches the paths detailed in the code (“/oxytricha computational genomics project/sw”). You must then run every cell in order in Google Colab. Doing so will give you all three de Bruijn graphs and the Bayesian scoring model for the Serine Threonine loci. In order to run the Bayesian model for one of the other two loci you must run the cells for the de Bruijn graph of that specific locus first before running the code in the Bayesian model and Path summation cells.
- No parameters are needed to run as the data for each contig is loaded and processed in the notebook. The notebook also does not require any particularly powerful machine or cloud, as it can run on the CPU in Google Colab. We used Python 3.12 which is the current version that Google Colab uses by default. All required add-on libraries are installed and loaded in the code itself. 

# Iterative_de_Bruijn_graph_+_visualization_cod.ipynb:
- This is a generalized version of the code that can be used in the future to create a de Bruijn graph and Bayesian model of any additional loci. Contains a Jupyter notebook with de Bruijn graph helper functions, implementation code, and visualization code. Also creates a Pyro Bayesian model to score the validity of an edge in a de Bruijn graph. It converts the loci data needed for priors into Pytorch tensors, uses Autoguide to update the predicted posterior distributions while training the model, and displays the final predicted posterior distributions as both slope and intercept graphs. User parameters can be put into the placeholder variables, then blocks can be run in order.
- Inputs needed:
  - Fastq_files = .fastq file containing sample reads
  - Reference_fasta = .fa file of target locus reference sequence (mature product)
  - precursor_reference_fasta = .fa file of target locus reference precursor sequence
  - Genesegment_fasta = .fa with sequences from annotated gene segments
- Does not require any particularly powerful machine or cloud, as it can run on the CPU in Google Colab. We used Python 3.12 which is the current version that Google Colab uses by default. All required add-on libraries are installed and loaded in the code itself. 

# Example Data: 
- FASTQ files that contain, per timepoint, intermediate reads that have at least one segment of the mature gene product of the target locus aligned to them.
- Note: the raw dataset containing all intermediate reads where these example inputs are extracted from is not publicly available. If you’re interested in getting inputs for different loci, feel free to reach out and discuss with us.
- FASTA files of sequences of the segments of mature gene product (MDSs)

# Validation:
## Path_validation.ipynb:
- Validates candidate rearrangement intermediate paths from “Demonstration_on_3_candidate_loci” notebook by comparing against an independent set of candidate intermediate reads, for TEBP-beta locus
- Computes per-path pooled k-mer coverage, which is the fraction of each path’s k-mers that are present in the independent read pool
- Computes per-read best-path coverage, which is how well the best-matching path explains each independent read
- Validates Bayesian score, whether they positively predict read support for scored paths
- Generates the figures shown in the report
- Inputs needed:
  - PATH_SCORES_TSV: TVS of sorted scored paths exported from the assembly notebook, containing the sequence and score of the paths, here provided in the “test data” folder
  - TRAINING_FASTQS: FASTQ files used to build the assembly, to exclude training reads from validation stats
  - TEST_BAMS: BAM files of independent test reads, here provided in the “test data” folder
- This notebook can be run locally with the right input file described above, and these input files can be found in the “test data” folder

## Test data:
- this folder contains input files for the validation notebook that generated the validation result
- TEBP_beta_sorted_paths.tsv: tsv of sorted scored paths exported from the assembly notebook
- candidate_tebp_beta_intermediates_36/48/60h.bam: independent set of test reads to be used for validation
- tebp_beta_36/48h_candidate_reads.fastq: FASTQ files of reads used to build the de Bruijn assembly
