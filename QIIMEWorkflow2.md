# QIIME
## Instructions for processing short Ion Torrent 16S amplicon sequences in QIIME

* Create a separate mapping file for each sample (see example mapping file)

* Download the .fastq sequence file and convert your .fastq file into seperate .fasta and .qual files by using this script
$ convert_fastaqual_fastq.py -f Filename.fastq -c fastq_to_fastaqual

* For each sample generate a separate split library file from the .fna and .qual files generated in the previous step
$ split_libraries.py -m MappingFile.txt -f SampleFilename.fna -q SampleFilename.qual -l 150 -L 350 -z truncate_only -o $ split_library_SampleFilename_output



* Concatenate all the split library files into one fasta file.  Make sure you enter a space between each .fna filename
$ cat split_library_Filename_output/seqs.fna split_library_Filename2_output/seqs.fna > Combined_seqs.fna

* Pick OTUs using the standard method on your combined seqs file
$ pick_de_novo_otus.py -i Combined_seqs.fna -o Combined_otus

* Summarize taxa through plots using the combined OTU table, but this time use a new mapping file that contains all the sample names, no barcodes
$ summarize_taxa_through_plots.py -i Combined_otus/otu_table.biom -o combined_wf_taxa_summary -m Combined_map.txt

* Compute beta diversity
$ beta_diversity_through_plots.py -i Combined_otus/otu_table.biom -m Combined_map.txt -o Combined_wf_bdiv_even146/ -t Combined_otus/rep_set.tre 

or use...
$ beta_diversity_through_plots.py -i Combined_otus/otu_table.biom -m Combined_map.txt -o Combined_wf_bdiv_even146/ -t Combined_otus/rep_set.tre -p ignore_parameter.txt

The ignore_parameter.txt file is a seperate file that is used if you want the program to ignore certain samples that will not be process. To make this file, create a .txt file as follows
$ make_emperor:ignore_missing_samples True

* Compute jacknife beta diversity
$ jackknifed_beta_diversity.py -i Combined_otus/otu_table.biom -t Combined_otus/rep_set.tre -m Combined_map.txt -o Combined_wf_jack -e 110 

or

$ jackknifed_beta_diversity.py -i Combined_otus/otu_table.biom -t Combined_otus/rep_set.tre -m Combined_map.txt -o Combined_wf_jack -e 110 -p ignore_parameter.txt -f

the flag -f is only needed to overwrite an existing file

* If you want to add species level info run:
$ summarize_taxa.py -i Combined_otus2/otu_table.biom -o combined_wf_taxa_summary3 -m Combined_map_1.txt -a -L 7

* Copy data into excel

