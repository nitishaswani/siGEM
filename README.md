# siGEM
_Software for automating and optimising siRNA purchase from IDT_

Integrated DNA Technologies (IDT) is a popular commercial resource for shortinterfering
RNAs (siRNAs), which are short double-stranded oligonucleotides used in
functional genomics studies to perturb gene expression. There is currently
a significant bottleneck in collating and computationally validating the various predesigned
siRNA molecules sold on IDT. Given that siRNA-gene transcript complementarity
is a crucial determinant of successful gene silencing, it is vital this condition is
met before purchasing the siRNA molecule. This also minimizes wasteful expenditure,
which is yet another objective in basic laboratories.

The process of siRNA purchase from IDT is complicated by the variety of
transcripts, and hence protein, isoforms a gene is capable of expressing, which significantly
amplifies the possible one-to-one interactions between an siRNA and its target.
This gives rise to IDT’s first limitation - the lack of comprehensive pre-designed siRNAs
currently displayed on their website. Therefore, the researcher has no choice but to
sift through the molecules and their sequences until a match is found with the specific
transcript isoform they wish to target. Its second limitation lies in the poor construction
of the website user-interface, leading the user down a rabbit hole of web traversals
until they can access siRNA sequence-level information. Currently, to our best knowledge,
this validation process is done manually, which is markedly laborious. 

## Program high-level objectives

&nbsp; 1. Automate siRNA purchase from IDT (without compromising customization
features that the software offers) to increase success rates of functional
genomics studies

&nbsp; 2. Enhance data integration between NCBI and IDT for transcript and
siRNA sequence-level information, respectively

&nbsp; 3. Optimize efficiency and financial costs for genomics studies as well as
downstream proteomics experiments

## High level class functionalities:
- ```NCBI``` class used to access key features using Entrez API: Provides functionality for retrieving accession codes, transcript sequences, downloading FASTA files
- ```IDT``` class that performs IDT scraping operations: Provides functionality for retrieving siRNA sequence information
- ```siGEM``` class that performs siRNA-transcript matching operations: Provides functionality for creating SQLite3 database and PDF of matched information


# SQLite database structure
_Note: the data dictionary is added in the repository documents folder_

<img src="https://github.com/nitishaswani/siGEM/assets/147107913/68ce8dff-9ea2-459f-834a-b8f230c40dda" width="790">


## Example usage:
``` python
# Creating a NCBI query object
ncbi_query = NCBI("brca2", "human")

# Getting a dictionary summarising all key information related to query
# Gene ID; Species name; Gene name; RefSeq gene summary
gene_summary = ncbi_query.ncbi_summary()
print(gene_summary)

# Getting list of accession codes
accession_codes = ncbi_query.accession_codes()
print(accession_codes)

# Getting a list of tuples of mRNA transcript sequences for returned accession codes
transcript_summary = ncbi_query.ncbi_transcript(accession_codes)

# Storing FASTA formatted files to local subdirectory for returned accession codes
fasta_file = ncbi_query.fasta(accession_codes)

# Getting siRNA sequence information by creating IDT object
idt = IDT("brca2", "human")
source_code = idt.source_code()
siRNA_sequences = idt.siRNA_sequence(source_code)

# Performing matching operation by creating siGEM object. Instantiating the class automatically creates SQLite3 database.
match = siGEM(gene_summary, transcript_summary, siRNA_sequences)
match_pdf = match.create_pdf()
```





