# Introduction

## Logistics
- Assignments will be posted on the [course website](https://umd-cmsc423.github.io/f2026/) and submitted on Gradescope. The base Gitub repo can be found [here](https://github.com/umd-cmsc423/f2026).
- Emails should include **[CMSC423-F26]** in the email subject
- Lectures are meant to be self-contained, but papers and external resources will be linked at times, and are worth reading/exploring
- Prerequisite knowledge of some basic command line and related skills will be expected, and primers on these can be found at MIT's ["The Missing Semester"](https://missing.csail.mit.edu/)

## Course topics
- Some content (material, projects, etc.) will be intentionally challenging!
- The best way to prepare for exams is to implement and understand the projects
- The class will revolve around *sequencing data*
- The top level learning goal is for students to recognize the structure by which genetic sequences are analyzed and made meaningful (to humans)
- Side note: the term "Computational Biology" is a semantic distinction from the term "Bioinformatics". Essentially both terms refer to the same area of study, and the difference is largely semantic. No distinction will be made between the two for class purposes

# Material
- Nowadays, sequencing an entire genome can cost only $100
- Limitations of sequencing include short reads, imperfect reads, and biased reads
- Sequencing often involves replicating the genome with PCR, then breaking it into random fragments. The fragments are sequenced, then reassembled
    - Genome reassembly is one of the most basic problems in computational genomics
    - Fragmentation, being a physical process, is not entirely random because some chemical bonds are stronger than others
    - Sequencers will usually output partially reassembled fragments that are correct within a certain confidence threshold
- Sequences are communicated/stored in a variety of formats, with FASTA being one of the most popular
- The NCBI sequence read archive (SRA) has grown ~4.5 million-fold in 17 years
- Obviously some regions of the human genome are variable, so the goal of a "complete genome" is a *pangenome*, or a reference built from many diverse genomes that represents all of humanity