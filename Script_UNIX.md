- [Connecting to the cluster](#connecting-to-the-cluster)
- [Files and file systems](#files-and-file-systems)
    + [Getting files](#getting-files)
    + [Editing in nano](#editing-in-nano)
    + [Zipped files](#zipped-files)
    + [Piping](#piping)
- [REGEXP](#regexp)
- [Loops and conditions](#loops-and-conditions)
- [Useful links:](#useful-links-)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

# Connecting to the cluster
The command ssh lets you connect to the cluster
```
ssh [username]@corso.came.sbg.ac.at
# For example:
ssh fortelnyb1075184@corso.came.sbg.ac.at
```

Next you will change your password. Make sure to remember this password which you will you throughout this course
```
passwd
```

Now we are connected to the cluster. In the following we will explore this file system. Details will be explored in the next lecture of this course. For now, just execute the commands and see if you can make sense of the results.

The cluster has a file system like any computer. Currently you are in your home directory. The current "path" is shown by the following command
```
pwd
```

You can list the content of your home directory using
```
ls *
ls -l *
```

You can clear the terminal by typing
```
clear
```

Now, use the up- and down-arrows on your keyboard to look through the history of commands.

Now create a directory
```
mkdir day1
```

List content of the directory again
```
ls *
ls -l *
```

What does mkdir do? Bring up the manual (press "q" to exit the manual).
```
man mkdir
```

Now create some files. There are many ways to create a file
```
# Create an empty file in directory "day1"
touch day1/touch.txt

# Write the text "abcdefgh" in a file.
echo "abcdefgh"  > day1/echo.txt
```

List files again
```
ls *
ls -l *
```

Take a look at the files with content
```
head day1/*
```

# Files and file systems

### Navigate the file system
First, let's clean up
```
clear
```

Go to the home directory (absolute path)
```
cd ~/
ls -l
pwd
```

Go to the folder with all home directories (absolute path)
```
cd /home
ls -l
pwd
```

Go back to your home directory (absolute path)
```
cd ~/
ls -l
pwd
```

Go into the directory created yesterday (relative path - from your home directory)
```
cd day1/
ls -l
pwd
```

Go back one level (relative path - from the "day1" folder)
```
cd ../
ls -l
pwd
```

Let's create another folder (relative path - from your home directory). What does the "-p" stand for?
```
pwd
mkdir -p day2/test/folder
pwd
```

Change permissions in a file created in the above folder.
```
touch day2/test/folder/test.txt
ls -l day2/test/folder

chmod 711 day2/test/folder/test.txt
ls -l day2/test/folder

chmod 722 day2/test/folder/test.txt
ls -l day2/test/folder

chmod 733 day2/test/folder/test.txt
ls -l day2/test/folder

chmod 744 day2/test/folder/test.txt
ls -l day2/test/folder

chmod 755 day2/test/folder/test.txt
ls -l day2/test/folder

chmod 766 day2/test/folder/test.txt
ls -l day2/test/folder

chmod 777 day2/test/folder/test.txt
ls -l day2/test/folder

chmod 700 day2/test/folder/test.txt
ls -l day2/test/folder

man chmod
```

The following table outlines permissions in chmod (https://en.wikipedia.org/wiki/Chmod)

x | Permission | rwx | Binary
--- | --- | --- | ---
7 | read, write and execute | rwx | 111
6 | read and write | rw- | 110
5 | read and execute | r-x | 101
4 | read only | r-- | 100
3 | write and execute | -wx | 011
2 | write only | -w- | 010
1 | execute only | --x | 001
0 | none | --- | 000

#### Exercises
- Create a new folder called "test2" within in the folder ~/day2.
- Enable full access to this folder for the owner of the folder, read access for the group, and no access for others

### Getting files

Make sure you are in your home directory
```
pwd
cd ~/
pwd
```

Obtain the list of profiles.
```
# move or copy?
man mv
man cp

# now try it:
mv /home/bioinfo1/gRNAs.txt ~/
cp /home/bioinfo1/gRNAs.txt ~/

# Why does cp not work? Look at file permissions:
ls -l /home/bioinfo1/gRNAs.txt
ls -l ~/gRNAs.txt
```

Now  explore the copied file
```
head gRNAs.txt
head -5 gRNAs.txt
tail -5 gRNAs.txt
less gRNAs.txt
more gRNAs.txt

# Count the number of lines
wc -l gRNAs.txt
```

#### Exercises

Copy the file gRNAs.txt from your home directory into the folder "day2". Then rename the copied file in this folder to "profiles_exercise.txt".

### Editing in nano

Now add a line at the end of the file "gRNAs.txt" that is located in your home directory. Add the profiles "TODO". To quit the nano-editor you need too press Ctrl+X. Then type Y+Enter to save the changes.
```
nano gRNAs.txt
```

Command | Function
 --- | --- 
ctrl+r | read/insert file
ctrl+o | save file
ctrl+x | close file
alt+a | start selecting text
ctrl+k | cut selection
ctrl+u | uncut (paste) selection
alt+/ | go to end of the file
ctrl+a | go to start of the line
ctrl+e | go to end of the line
ctrl+c | show line number
ctrl+_ | go to line number
ctrl+w | find matching word
alt+w | find next match
ctrl+\ | find and replace

Now let's modify our shell to make things prettier
```
nano ~/.bashrc
```

Add the following lines in nano, then exit the file (and save it).
```
TODO
```

Now run the commands stored in this file
```
source ~/.bashrc
```

From now on, every time you login, these changes will be in place.


### Zipped files

Now let's download all human gene sequences from Ensembl.
```
man wget
wget http://ftp.ensembl.org/pub/release-103/fasta/homo_sapiens/cds/Homo_sapiens.GRCh38.cds.all.fa.gz
```

How large is this file?
```
du *
du -sh *
```

Have a look at this file
```
head Homo_sapiens.GRCh38.cds.all.fa.gz
```

This doesn't look great. Remember to clean up your terminal
```
clear
```

The above file is zipped. Let unzip it.
```
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz
# This command will run through the entire file which is very long. Press Ctrl+C to stop the command.

man gunzip
#        -c --stdout --to-stdout
#              Write  output on standard output; keep original files unchanged.  If there are several input files, the output consists of a sequence of independently compressed members. To obtain better compression, concatenate all input files before compressing them.
```

Again, remember to clean up your terminal
```
clear
```

Can we use "head" on the unzipped output? Yes - this is done by "pipeing"

### Piping

Pipeing enables you to pass the output of one command to another command.

Pipe command | Function
--- | ---
cmd < file | use file as input
cmd > file | write output to file
cmd >> file | append output to file
cmd 2> stderr | error output to file
cmd &> file | send output and error to file
cmd1 \| cmd2 | send output of cmd1 to cmd2


Let's have a look at the first few lines
```
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head
```

Some programs let you look at decompressed output, for example
```
zless Homo_sapiens.GRCh38.cds.all.fa.gz
# very similar to:
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | zless
```

Or count the number of lines in this file
```
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | wc -l
```

#### Exercises
- Create a folder called "day3" in your home
- Store the number of lines of Homo_sapiens.GRCh38.cds.all.fa.gz into the file "lineNumber.txt" in the folder "day3"
- Write the first 15 lines of Homo_sapiens.GRCh38.cds.all.fa.gz into the file "lines.txt" in the folder "day3"
- Write the 31th to 35th line of Homo_sapiens.GRCh38.cds.all.fa.gz into the file "lines2.txt" in the folder "day3"
- Store the size of Homo_sapiens.GRCh38.cds.all.fa.gz in Mb into the file "size.txt" in the folder "day3"

# Patterns and regular expressions

### File pattern matches

Commands can be executed on multiple files at the same time using pattern matches.

```
ls -l
ls -l *
ls -l day*
wc -l *
wc -l *.gz
```

Pattern | Description
 --- | ---
* | Match zero or more characters
? | Match any single character
[...] | Match any of the characters in a set
?(patterns) | Match zero or one occurrences of the patterns (extglob)
*(patterns) | Match zero or more occurrences of the patterns (extglob)
+(patterns) | Match one or more occurrences of the patterns (extglob)
@(patterns) | Match one occurrence of the patterns (extglob)
!(patterns) | Match anything that doesn't match one of the patterns (extglob)

### Simple regular expressions

Regular expressions can be executed on file names but also content within files. Below is the example from the PDF presented during the lecture. The file "sample" needs to be copied over from the home directory of user "bioinfo".
```
cat sample
grep ^a sample
grep -E p\{2} sample
grep "a\+t" sample
```

### Checking the nucleotides in the Ensembl fasta file

Have a look at the first few lines
```
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -50
```

We expect all DNA sequences to be made up of A, C, T, and Gs. Next, we will verify this. First, we will get all DNA-sequences from this file
```
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | grep -v ">"

# from "man grep": -v Invert the sense of matching, to select non-matching lines.
```

Next we will use "tr" to delete newline characters, to place all characters in one (very long) line
```
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | grep -v ">" | tr -d '\n'
```

Next we place every character on a different line
```
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | grep -v ">" | tr -d '\n' | grep -o .
```

Finally, we want to only see unique letters. To do so, we have to first sort the lines and then make them unique.
```
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | grep -v ">" | tr -d '\n' | grep -o . | sort | uniq

# what happens if we only make them unique?
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | grep -v ">" | tr -d '\n' | grep -o . | head -20
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | grep -v ">" | tr -d '\n' | grep -o . | head -20 | uniq
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | grep -v ">" | tr -d '\n' | grep -o . | wc -l
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | grep -v ">" | tr -d '\n' | grep -o . | uniq | wc -l
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | grep -v ">" | tr -d '\n' | grep -o . | sort | uniq | wc -l
```

#### Exercises
- Create a folder "day4" in your home directory
- Count the number of A, C, T, and G of the DNA sequences of the first 500 lines of file Homo_sapiens.GRCh38.cds.all.fa.gz
- Store each number in the  file called nt_A.txt, nt_C.txt,... in the folder day4

### Reformatting the Ensembl fasta file

Remind ourselves of how this file looks like:
```
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500
```

Next we will remove all newline character matches. This is done by "sed".
```
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | sed -z 's/\n[^>]/ /g'
```

We will place "seq:" in front of the DNA-sequence of each line.
```
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | sed -z 's/\n[^>]/ /g' | sed -E 's/([ACTG]*)$/seq:\1/g'
```

We developed the above approach on the first 30 lines. Now run it an the full file.
```
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | sed -z 's/\n[^>]/ /g' | sed -E 's/([ACTG]*)$/seq:\1/g' | gzip > GRCh38_reformatted.gz
```

Have a look at the generated file.
```
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | wc -l
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | grep ">" | wc -l
gunzip -c GRCh38_reformatted.gz | wc -l
zless GRCh38_reformatted.gz
```


gunzip -c Data.gz | cut -d$" " -f7,14

### Identifying nucleotide profile matches

Now we will identify genes in the database that match a specific DNA pattern. 
```
gunzip -c GRCh38_reformatted.gz | head -30
```

We check that all entries have "gene symbols"
```
gunzip -c GRCh38_reformatted.gz | grep "gene_symbol" | head -30 
gunzip -c GRCh38_reformatted.gz | grep -v "gene_symbol" | head -30 
```

Next we extract those sequences matching the profile "CGAC"
```
gunzip -c GRCh38_reformatted.gz | grep "seq:[ACTG]*CGAC" | head -30 
```

Now we extract the gene symbols of these matches. First we match all text before the text "gene_symbol"
```
gunzip -c GRCh38_reformatted.gz | grep "seq:[ACTG]*CGAC" | sed 's/^.*gene_symbol://g' | head -30
```

Then we also remove all text after the gene symbol
```
gunzip -c GRCh38_reformatted.gz | grep "seq:[ACTG]*CGAC" | sed 's/^.*gene_symbol://g' | sed 's/ .*$//g'  | head -30
```

Finally, we get unique gene IDs.
```
gunzip -c GRCh38_reformatted.gz | grep "seq:[ACTG]*CGAC" | sed 's/^.*gene_symbol://g' | sed 's/ .*$//g'  | sort | uniq
```


#### Exercises

Create the following files in the folder "day4":

- Extract all entries with sequences matching the pattern "ATTAGC" in the file patternMatch_ATTAGC.txt
- Write the count of entries with sequences starting with "TGC" into the file count_TGC.txt
- Write the count of entries of gene "MMP2" into the file count_MMP2_.txt. Note: Do not count genes MMP20, MMP21,...
- Write the count of unique genes whose symbol starts with "RPL" into the file count_RPL.txt
- Write the count of all unique protein coding genes into the file count_protein_coding.txt

# Loops and variables

### Variables

Variables can hold information which can be passed to different programs
```
x="test variable"
echo $x
echo $x | sed 's/a//g'
```

Now we will write the profiles to test and the patterns into variables
```
guide=TT
echo $guide
pattern="seq:[ACTG]*${guide}"
echo $pattern
gunzip -c GRCh38_reformatted.gz | grep $pattern | head -30
gunzip -c GRCh38_reformatted.gz | grep $pattern | sed 's/^.*gene_symbol://g' | sed 's/ .*$//g' | head -30
```


### Loops

The following loop starts with x = 1. It then repeats printing "Welcome ... times" and adding +1 to x, until x is greater than 5 (while x is lower or equal to 5).
```
x=1
while [ $x -le 5 ]
do
  echo "Welcome $x times"
  x=$(( $x + 1 )) # syntax to add a number to x
done
```

We can also loop through the content of a file.
```
while read p; do
  echo "$p"
done <gRNAs.txt
```

#### Exercise
Now we will test all profiles in file gRNAs.txt against all DNA sequences in GRCh38_reformatted.gz. To do so:

- Use the function that uses profiles stored in variables to extract relevant gene symbols. 
- Place this function into the loop that iterates through the file gRNAs.txt
- Store all results (not the top 30 coming from head -30) of each profile into a file that is named like this: `results_${profile}.txt`. Place all files into a new folder "day4"
- Check a few examples by hand. Do you get the right genes? Do you get the correct number of genes?

### Comparing files

Have a look at the files we generated.
```
cd ~/day5
ls results_*.txt
ls -l results_*.txt
head results_*.txt
wc -l results_*.txt
```

Now let's compare individual files.
```
cd ~/day5
comm result_TAC.txt result_TT.txt
comm -13 result_TAC.txt result_TT.txt
```

This overlap can be stored in a variable
```
guide1=TAC
guide2=TT
overlap=$(comm -13 result_TAC.txt result_TT.txt)
echo "$guide1 $guide2 $overlap"
```

#### Exercise

Now we will compare each pair of guides to test the overlap of genes.

- Use one loop within another loop to check pairs of guides
- Use the "comm" command as shown above to extract the overlap, counting the number of genes, and writing the result into a file named `overlap_${guide1}_${guide2}.txt`.
- "Manually" check results, for example comparing to the example above ("TAC" vs "TT")


# Useful links:
- https://bioinformaticsworkbook.org/Appendix/Unix/unix-basics-1.html#gsc.tab=0
- https://bioinformaticsworkbook.org/Appendix/Unix/UnixCheatSheet.html#gsc.tab=0
- https://bioinformatics.uconn.edu/unix-basics/#
- https://decodebiology.github.io/bioinfotutorials/
- https://www.melbournebioinformatics.org.au/tutorials/tutorials/unix/unix/
- https://www.thegeekstuff.com/2010/11/50-linux-commands/