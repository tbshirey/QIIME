# QIIME for Ion Torrent sequencing
## Workflow for processing short 16S rRNA gene amplicon sequences in QIIME

1. Create a separate mapping file for each sample

2. Download each .fastq sequence file and convert the .fastq files into seperate .fasta and .qual files:

   $ convert_fastaqual_fastq.py -f filename.fastq -c fastq_to_fastaqual

3. For each sample generate a separate split library file from the .fna and .qual files generated in step 2:

   $ split_libraries.py -m mappingfile.txt -f filename.fna -q filename.qual -l 150 -L 350 -z truncate_only -o split_library_filename_output

4. Concatenate all the split library files into one fasta file. Enter a single space between each .fna filename:

   $ cat split_library_filename_output/seqs.fna split_library_filename2_output/seqs.fna > Combined_seqs.fna

5. Pick OTUs using the standard method on the combined_seqs.fna file:

   $ pick_de_novo_otus.py -i Combined_seqs.fna -o Combined_otus

6. Summarize taxa through plots using the combined OTU table (otu_table.biom). For this step, use a new mapping file that contains all the sample names, no barcodes:

   $ summarize_taxa_through_plots.py -i Combined_otus/otu_table.biom -o combined_wf_taxa_summary -m Combined_map.txt

7. Compute beta diversity:

   $ beta_diversity_through_plots.py -i Combined_otus/otu_table.biom -m Combined_map.txt -o Combined_wf_bdiv_even146/ -t Combined_otus/rep_set.tre 

   or...

   $ beta_diversity_through_plots.py -i Combined_otus/otu_table.biom -m Combined_map.txt -o Combined_wf_bdiv_even146/ -t Combined_otus/rep_set.tre -p ignore_parameter.txt

8. The ignore_parameter.txt file is a seperate file that is used if you want the program to ignore certain samples and therefore will not be processed. Create this .txt file as follows:

   $ make_emperor:ignore_missing_samples True

9. Compute jacknife beta diversity:

   $ jackknifed_beta_diversity.py -i Combined_otus/otu_table.biom -t Combined_otus/rep_set.tre -m Combined_map.txt -o Combined_wf_jack -e 110 

   or...

   $ jackknifed_beta_diversity.py -i Combined_otus/otu_table.biom -t Combined_otus/rep_set.tre -m Combined_map.txt -o Combined_wf_jack -e 110 -p ignore_parameter.txt -f

   _note: the flag -f is only needed to overwrite an existing file_

10. To include species level taxonomic analysis use the summarize_taxa.py command with the -L 7 flag as follows:

   $ summarize_taxa.py -i Combined_otus2/otu_table.biom -o combined_wf_taxa_summary3 -m Combined_map_1.txt -a -L 7

### The workflow above is one example of performing metagenomic analysis on Ion Torrent sequence data using QIIME. For additional scripts, parameters, etc. please refer to the QIIME website. QIIME can be accessed at [QIIME](http://qiime.org/index.html). 
