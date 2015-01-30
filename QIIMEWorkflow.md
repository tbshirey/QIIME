### QIIME
## Instructions for processing short Ion Torrent 16S amplicon sequences in QIIME

# Create a separate mapping file for each sample (see example mapping file)

# Download the .fastq sequence file and convert your .fastq file into seperate .fasta and .qual files by using this script
convert_fastqual_fastq.py -f Filename.fastq -c fastq_to_fastqual

# For each sample generate a separate split library file from the .fna and .qual files generated in the previous step
split_libraries.py -m MappingFile.txt -f SampleFilename.fna -q SampleFilename.qual -l 150 -L 350 -z truncate_only -o split_library_SampleFilename_output



# Concatenate all the split library files into one fasta file
cat split_library_Filename_output/seqs.fna split_library_Filename2_output/seqs.fna > Combined_seqs.fna

