# CSE185 Final Project
We are making a tool to find peaks given transcription factor data. We will take an input of a tag directory of a certain input genome and a control genome for comparison, and then utilize an approach to estimate where the peaks are.   
Our output will be a .BED file written to the directory of the user's choice.  
Tag directories MUST be generated prior to running our program, with tag files in the TSV format. Tag directories can be created using HOMER's makeTagDirectory command. http://homer.ucsd.edu/homer/ngs/tagDir.html
Tag directories MUST share file names for respective chromosomes and end with .tags.tsv (i.e tag file for chromosome 17 must be 17.tags.tsv in both directories). 

# Packages to Install
Our tool relies on scipy. To install scipy, run
```bash
pip install scipy
```
# Preprocessing:
We will have .bam files in the example-files/ folder. Please utilize those to make tag directories before running our program
Run tag directories with the makeTagDirectory command. 
```bash
makeTagDirectory <output directory> <input BAM file> [options]
 ```
 Example:
 ```bash
 makeTagDirectory ./tagdirs/Oct4 ./Oct4.sorted.bam
```


 
# Calling the Function:
IMPORTANT: Before running our program, make sure you have created tag directories for both your transcription factor read data and the corresponding control. See the **Preprocessing** step.
```bash
python peakFinder.py <tag_directory_path> -c <optional_control_tag_directory_path> -O [optional_output_path] -t [threads]
```


# How to Test Our Function:
run the command on the example files. There are two folders, one is our lab5-benchmark, and one is from ENCODE-benchmark. 
Example for running based on Sox2 in the lab5:
```bash
python peakFinder.py lab5-benchmark/tagdirs/Sox2/ -O example.bed
```
For the ENCODE-benchmark, ENCFF356 is the experimental data, and ENCFF171OVT is the control input.
Compare the BED file output by loading it in IGV to visualize peak locations. See # Benchmarking for more details.
NOTE: The tag directory that we have in the benchmark only has two files, this is because our github cannot support files that are too big, bigger than 25 MB. If you want full example files, go to example-files and follow the text file's instructions there.

# File Format:
The output file format is going to be a BED file. For each peak, it will give the chromosome number, a start and end position, as well as their respective p-values. 

# To download:
To download our repository into your system, run the command:
```bash
git clone https://github.com/g1cole/CSE185.git
```

# Benchmarking 
To benchmark the code to see if peaks match, download HOMER or any other preferred peak calling program and run on the test data (example is using HOMER):
```bash
findPeaks /lab5-benchmark/tagdirs/Sox2 -style factor -o auto
python peakFinder.py /lab5-benchmark/tagdirs/Sox2 -t 6 -O peaks185.bed
```
You can visualize both these bed files in IGV, or if you prefer you may utilize a command such as HOMER's MakeUCSC to convert the bed file to a bedgraph and visualize. 
From here, you can see how the identified peaks compare.
