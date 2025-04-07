# Intership
# Download the RawData from NCBI with these following Steps:
Download and install SRA Toolkit for Windows
Download from: https://github.com/ncbi/sra-tools/wiki/02.-Installing-SRA-Toolkit

# Add the SRA Toolkit bin directory to your PATH or navigate to the bin directory
cd C:\Program Files\sratoolkit.3.0.7-win64\bin 
./prefetch.exe --version
 ~/desktop/sratoolkit.3.2.1-win64/bin/fasterq-dump.exe --split-files SRR30230878
# And now the Raw file (SRR) are splitted to fastq files
