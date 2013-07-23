**************************************
*      Build the UTE with BLAST      *
**************************************
By: Paul Bilinski, UC Davis, 2012

This is the series of steps to take to generate a unique transposable element database (UTE).  The goal is to use BLAST to identify the shared regions and then use this series of scripts to mask out the common regions with ---.  Once NNNs and common regions are turned into ---, we can then use the scripts to cycle through and extract all tags of a given length into a UTE.

Step 1.  Have the FASTA of the system you are looking at, and the blast results from the blast against itself.

(blast query)
blastn -query ~/BlastFiles/BF002.txt -evalue 1E-1 -outfmt 7 -db TEdb_nobreak.fasta -num_threads 4 -out ~/BlastOutputs/BF002-R.csv

TEdb_nobreak-R.csv (Results from blast)
TEdb_nobreak_namechange.fasta (Fasta with the names changed so no spaces)

Step 2. Use the blast to mask out the regions within the TE reference with the perl script Mask_Blasthits.pl

perl Mask_Blasthits.pl TEdb_nobreak-R.csv TEdb_nobreak_namechange.fasta dashes.txt Masked_output.txt

Step 3.  Use textwrangler to get rid of any of the NN's after the masking is complete.  To do so, search for NN and replace with --.  Be careful, as 5 TEs with SINE have an N, so if you replace that N with a dash you replace that as well.

Masked_output.txt (file you have to edit)
Masked_output_no_N.txt

Step 4.  Use perl script Sep_TE_tags.pl to parse the file and generate your UTE!

First, you have to replace all --- so that they are just a single - char.  Then, run the script as such

perl Sep_TE_tags.pl 