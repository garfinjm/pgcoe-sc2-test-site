---
title: "Assembly Uploads"
layout: page
nav_order: 6
---

# Assembly Uploads
{: .no_toc }

- **Table of Contents**
{:toc}

<!---
Sections start here
-->

## How Can I Annotate my Genome?

NCBI has different annotation requirements based on the type of organism being submitted. See the table below to determine whether annotation is required for your organism:

| **Organism** | **Annotation Required?** | **Files Required** | **Note** |
| --- | --- | --- | --- |
| Bacteria | Optional | Genome fasta  OR  Genome fasta + .sqn file | Genomes submitted without an annotation will be annotated with [PGAP](https://www.ncbi.nlm.nih.gov/refseq/annotation_prok/) by NCBI. |
| Eukaryotes | Optional | Genome fasta  OR  Genome fasta + .sqn file | Genomes submitted without an annotation will be stored in NCBI unannotated. |
| SARS-Cov-2 | No | Genome fasta | SARS-CoV-2 genomes will be annotated with [VADR](https://github.com/ncbi/vadr) by NCBI. |
| Influenza | No | Genome fasta | Influenza genomes will be annotated with [FLAN](https://pmc.ncbi.nlm.nih.gov/articles/PMC1933127/) by NCBI. However, they intend to update to VADR in the future and will accept any genomes that pass VADR but fail FLAN if you ask them. |
| Other viruses | Yes | .sqn file |  |

### Annotating and submitting bacterial genomes

Annotation of bacterial genomes before submission to NCBI is optional, and in most cases unnecessary. Bacterial genomes submitted to NCBI without an annotation will be annotated by NCBI's [PGAP](https://www.ncbi.nlm.nih.gov/refseq/annotation_prok/) tool shortly after submission and made available on NCBI with the genome.

In cases where NCBI's automatic annotation is not desired, [Bakta](https://github.com/oschwengers/bakta) is a great tool for bacterial annotation. If run with the `--compliant` argument, the .gff3 files produced by Bakta can be fed directly into NCBI's [table2asn](https://www.ncbi.nlm.nih.gov/genbank/table2asn/) software to create .sqn files needed to submit genome annotations to NCBI. The Bakta documentation has [a quick overview](https://github.com/oschwengers/bakta?tab=readme-ov-file#genome-submission) of the procedure to generate the compatible .gff3 files here. NCBI provides a guide to running table2asn with .gff3 files [here](https://www.ncbi.nlm.nih.gov/genbank/genomes_gff/).

There are many other tools for genome annotation and methods of submitting bacterial genomes to NCBI available, but the options above should cover most public health use cases.

### Annotating and submitting viral genomes

The hardest part of uploading viral assemblies to NCBI is their requirement to have the genomes annotated with genes. You should seek out an automated gene annotation tool appropriate for your organism. A gene annotation tool would generally take as input 1) your "bare" unannotated genome fasta and 2) some external reference data, and would produce as output a "tbl" file that contains the gene names and locations in a coordinate space relative to your provided genome. That tbl file would then be input to [table2asn](https://www.ncbi.nlm.nih.gov/genbank/table2asn/) which is required for submission to GenBank. Instructions for running table2asn to generate a .sqn file are [available on NCBI's website](https://www.ncbi.nlm.nih.gov/genbank/eukaryotic_genome_submission/#Create_your_submission).

If you have a closely-related reference genome already in Genbank that is genetically similar to yours (no major inversions or transpositions), a very simple annotation approach is to take the "tbl" file of the reference genome (the Genbank web interface lets you download any accession as a "Feature Table", which is a tbl file) and just convert the gene coordinates in that reference genome to the corresponding coordinates in your genome, utilizing a pairwise alignment between the two. This is the conceptual approach of many gene annotation tools/scripts out there.

For viruses, [VADR](https://github.com/ncbi/vadr) is a command line tool maintained by NCBI that can annotate bare viral genomes for species that it has a pre-built model for. Although it was originally built to support a handful of viral taxa, the list of viral species with built models is [increasing regularly](https://github.com/broadinstitute/viral-references/blob/main/annotation/vadr/vadr-by-taxid.tsv), and you could follow their documentation to build models for new taxa with some effort.

## Genome Naming and Species Differentiations

There are multiple international bodies that exist to aid in the naming of species and to help create structure around species naming conventions and submission to international repositories. These international bodies are different for the different taxonomic classes of organisms. There are three main taxonomic separations included in NCBI: 
- Prokaryotes fall under the [International Code of Nomenclature of Prokaryotes](https://the-icsp.org/code-of-nomenclatur) ([ICSP](http://www.the-icsp.org/judicial-commission-on-prokaryote-nomenclature))
- Animals fall under the [International Code of Zoological Nomenclature](https://www.iczn.org/the-code/the-code-online/) ([ICZN](https://www.iczn.org/))
- Plants, algae and fungi fall under the [International Code of Nomenclature](https://www.iapt-taxon.org/nomen/main.php) (ICN). 
- Viruses however do not have a code of nomenclature, but scientists follow recommendations from the [International Committee on Taxonomy of Viruses](https://ictv.global/) ([ICTV](http://www.ictvonline.org/)).

When submitting samples to NCBI, there are multiple guidelines on what information should be included in a submission based on specific recommendations for that particular species. Well characterized and popularly submitted viruses, like influenza and SARS-CoV-2, have well established schemas which are recommended by multiple bodies. In the case of SARS-CoV-2, for example, [ICTV released the following naming convention](https://pmc.ncbi.nlm.nih.gov/articles/PMC7095448/pdf/41564_2020_Article_695.pdf) for individual isolates: SARS-CoV-2/host/location/isolate/date. Further recommendations by the CDC proposed simplifying naming conventions to include country/state-lab-sample/year information when submitting to public repositories like GISAID and NCBI. In certain instances, these simplifications are aimed at reducing PII, subsequently reducing the likelihood that a sample will not be uploaded in order to avoid being identifiable. It is generally suggested that submitters look to ICTV to see if there is a naming schema available for their virus of interest. If none is available, then looking to other closely related viruses may be advisable.

> **🔗Links**
> - [How are organisms in the NCBI Taxonomy database formally named? - NLM Customer Support Center](https://support.nlm.nih.gov/kbArticle/?pn=KA-03381)
> - [The species Severe acute respiratory syndrome-related coronavirus: classifying 2019-nCoV and naming it SARS-CoV-2](https://pmc.ncbi.nlm.nih.gov/articles/PMC7095448/pdf/41564_2020_Article_695.pdf)
> - [CDCgov/SARS-CoV-2\_Sequencing: A collection of sequencing protocols and bioinformatic resources for SARS-CoV-2 sequencing.](https://github.com/CDCgov/SARS-CoV-2_Sequencing)

## Submission Mechanics

### Submission Mechanics: bacteria

The primary method of submitting bacterial genomes to NCBI is through the [NCBI Submission Portal "Genome" page](https://submit.ncbi.nlm.nih.gov/subs/genome/). This user-friendly web interface allows submission single genomes or batches of up to 400 genomes. Sample metadata is provided either by filling in a series of forms (for single sample submissions) or by filling out and uploading structured spreadsheet/csv files with sample metadata.

{: .warning }
All the samples in a batch submission must share a common set of features: BioProject, release date, assembly type, file type (FASTA or SQN), gap/Ns details, publication information, and PGAP request status.

Genome and annotation data files can be uploaded through several mechanisms:

1. An "upload" button in the submission processes (Best option for low volumes or beginners)
2. File Transfer Protocol (FTP) (Good for large datasets or many files)
   1. This method can be used with GUI FTP clients (ex. WinSCP, Filezilla) or via the ftp command line tool.
3. Aspera protocol (Good for large datasets or many files)
   1. This method can be used via the Aspera browser plugin, or the Aspera command line tool.

{: .note }
Options 2 and 3 above require configuring your FTP/Aspera client with [credentials provided](https://submit.ncbi.nlm.nih.gov/subs/genome/) on the NCBI Genome website.

### Submission Mechanics: most viruses

Email a friendly message with an attached zipped sqn file (the output of table2asn) to [gb-sub@ncbi.nlm.nih.gov](mailto:gb-sub@ncbi.nlm.nih.gov). There is no FTP/XML submission option currently. You cannot use the normal NCBI submission web portal (there is a special but different web portal called [BankIt](https://www.ncbi.nlm.nih.gov/WebSub/) instead, but email is easier honestly). You will hear back from them usually within a business day with any curation back and forth.

{: .note}
Viral genome submissions cannot be linked to NCBI submission groups currently - they will be only associated with your personal NCBI account.

### Submission Mechanics: special viruses (SARS-CoV-2, Influenza A/B/C, Norovirus, Dengue Virus)
*Writing in progress*