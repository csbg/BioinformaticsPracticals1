# CONTENTS
- [Connecting to the cluster](#connecting-to-the-cluster)
- [Files and file systems](#files-and-file-systems)
    + [Navigate the file system](#navigate-the-file-system)
      - [Exercises](#exercises)
    + [Getting files](#getting-files)
      - [Exercises](#exercises-1)
    + [Editing in nano](#editing-in-nano)
    + [Zipped files](#zipped-files)
    + [Piping](#piping)
      - [Exercises](#exercises-2)
- [Patterns and regular expressions](#patterns-and-regular-expressions)
    + [File pattern matches](#file-pattern-matches)
    + [Simple regular expressions](#simple-regular-expressions)
    + [Checking the nucleotides in the Ensembl fasta file](#checking-the-nucleotides-in-the-ensembl-fasta-file)
      - [Exercises](#exercises-3)
    + [Reformatting the Ensembl fasta file](#reformatting-the-ensembl-fasta-file)
    + [Identifying gRNA matches](#identifying-grna-matches)
      - [Exercises](#exercises-4)
- [Loops and variables](#loops-and-variables)
    + [Variables](#variables)
    + [Loops](#loops)
      - [Exercise](#exercise)
    + [Comparing files](#comparing-files)
      - [Exercise](#exercise-1)
- [Useful links](#useful-links)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>


# Connecting to the cluster
(Day 1)
The command ssh lets you connect to the cluster. Note: you will need VPN active to be able to connect.
```bash
ssh [username]@corso.came.sbg.ac.at
# For example:
ssh nfortelny@corso.came.sbg.ac.at
```

Next you will change your password. Make sure to remember this password which you will you throughout this course
```bash
passwd
```

Now we are connected to the cluster. In the following we will explore this file system. Details will be explored in the next lecture of this course. For now, just execute the commands and see if you can make sense of the results.

The cluster has a file system like any computer. Currently you are in your home directory. The current "path" is shown by the following command
```bash
pwd
```

You can list the content of your home directory using
```bash
ls
ls -l
```

You can clear the terminal by typing
```bash
clear
```

Now, use the up- and down-arrows on your keyboard to look through the history of commands.

Now create a directory
```bash
mkdir day1
```

List content of the directory again
```bash
ls
ls -l
```

What does mkdir do? Bring up the manual (press "q" to exit the manual).
```bash
man mkdir
```

Now create some files. There are many ways to create a file
```bash
# Create an empty file in directory "day1"
touch day1/touch.txt

# Write the text "abcdefgh" in a file.
echo "abcdefgh"  > day1/echo.txt
```

List files again
```bash
ls
ls -l
```

Take a look at the files with content
```bash
head day1/*
```

---------------
END OF DAY 1
---------------

# Files and file systems
(Day 2)

### Navigate the file system
First, let's clean up
```bash
clear
```

Go to the home directory (absolute path)
```bash
cd ~/
ls -l
pwd
```

Go to the folder with all home directories (absolute path)
```bash
cd /home
ls -l
pwd
```

Go back to your home directory (absolute path)
```bash
cd ~/
ls -l
pwd
```

Go into the directory created yesterday (relative path - from your home directory)
```bash
cd day1/
ls -l
pwd
```

Go back one level (relative path - from the "day1" folder)
```bash
cd ../
ls -l
pwd
```

Let's create another folder (relative path - from your home directory). What does the "-p" stand for?
```bash
pwd
mkdir -p day2/test/folder
pwd
```

Change permissions in a file created in the above folder.
```bash
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
```bash
pwd
cd ~/
# same as "cd " or "cd ~" --> default value for the dir argument is HOME (see "man cd")
pwd
```

Obtain the list of gRNAs.
```bash
# move or copy?
man mv
man cp

# now try it:
mv /home/bioinfo1/gRNAs.txt ~/
cp /home/bioinfo1/gRNAs.txt ~/

# Why does mv not work? Look at file permissions:
ls -l /home/bioinfo1/gRNAs.txt
ls -l ~/gRNAs.txt
```

Now  explore the copied file
```bash
head gRNAs.txt
head -5 gRNAs.txt
tail -5 gRNAs.txt
less gRNAs.txt
more gRNAs.txt

# Count the number of lines
wc -l gRNAs.txt
```

#### Exercises

Copy the file gRNAs.txt from your home directory into the folder "day2". Then rename the copied file in this folder to "gRNAs_exercise.txt".

### Editing in nano

Now add a line at the end of the file "gRNAs.txt" that is located in your home directory, adding the gRNA sequence "ACTGACTG". To quit the nano-editor you need too press Ctrl+X. Then type Y+Enter to save the changes. Nano commands are shown in the editor and can be found on the internet. A list is provided below.
```bash
nano gRNAs.txt
```

Nano commands:

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

Now use nano to modify the shell to make things prettier. To do so change the file bash_profile. This file contains settings for each user (the naming is just by convention). It starts with a ".", which for Linux means the file is hidden. 
```bash
nano ~/.bash_profile
```

Add the following lines to bash_profile in nano, then exit the file and save it - see the commands above.
```bash
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    alias grep='grep --color=auto'
fi
```

The changes we added to bash_profile will come into effect next time you log in. To also activate them for your current login, you can "source" the file, executing the commands stored within.
```bash
ls -l *
source ~/.bash_profile
ls -l *
```

Also, notice the difference in ls commands to show hidden files (like bash_profile).
```bash
ls -l
ls -al
```



### Zipped files

Now let's download all human gene sequences from Ensembl.
```bash
man wget
wget http://ftp.ensembl.org/pub/release-103/fasta/homo_sapiens/cds/Homo_sapiens.GRCh38.cds.all.fa.gz
```

How large is this file?
```bash
du *
du -sh *
```

Have a look at this file
```bash
head Homo_sapiens.GRCh38.cds.all.fa.gz
```

This doesn't look great. Remember to clean up your terminal
```bash
clear
```

The above file is zipped. Now unzip it.
```bash
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz
# This command will run through the entire file which is very long. Press Ctrl+C to stop the command.

man gunzip
#        -c --stdout --to-stdout
#              Write  output on standard output; keep original files unchanged.  If there are several input files, the output consists of a sequence of independently compressed members. To obtain better compression, concatenate all input files before compressing them.
```

Again, remember to clean up your terminal
```bash
clear
```

Can we use "head" on the unzipped output? Yes - this is done using a "pipe"

### Pipes

Linux pipes enables you to pass the output of one command to another command.

Pipe command | Function
--- | ---
cmd < file | use file as input
cmd > file | write output to file
cmd >> file | append output to file
cmd 2> stderr | error output to file
cmd &> file | send output and error to file
cmd1 \| cmd2 | send output of cmd1 to cmd2


Let's have a look at the first few lines of this file.
```bash
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head
```

Some programs let you look at decompressed output, for example
```bash
zless Homo_sapiens.GRCh38.cds.all.fa.gz
# very similar to:
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | zless
```

Now we can also count the number of lines in this file
```bash
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | wc -l
```

#### Exercises
Create a folder called "day2" in your home. Next place the following files into the folder "day2", using pipes:
- Store the number of lines of Homo_sapiens.GRCh38.cds.all.fa.gz into the file "lineNumber.txt".
- Write the first 15 lines of Homo_sapiens.GRCh38.cds.all.fa.gz into the file "lines1.txt".
- Write the 31th to 35th line of Homo_sapiens.GRCh38.cds.all.fa.gz into the file "lines2.txt".
- Store the size of Homo_sapiens.GRCh38.cds.all.fa.gz in Megabytes into the file "size.txt".

---------------
END OF DAY 2
---------------

# Patterns and regular expressions
(Day 3)

### File pattern matches

Commands can be executed on multiple files at the same time using pattern matches (we have used this already above).

```bash
ls -l
ls -l *
ls -l day*
wc -l *
wc -l *.gz
```

Description | Pattern
--- |  ---
Match zero or more characters | *
Match any single character | ?
Match any of the characters in a set | [...]
Match zero or one occurrences of the patterns (extglob) | ?(patterns)
Match zero or more occurrences of the patterns (extglob) | *(patterns)
Match one or more occurrences of the patterns (extglob) | +(patterns)
Match one occurrence of the patterns (extglob) | @(patterns)
Match anything that doesn't match one of the patterns (extglob) --> | !(patterns)

### Simple regular expressions

Regular expressions can be executed on file names but also content within files. Below is the example from the PDF presented during the lecture. Note: The file "sample" needs to be copied over from the home directory of user "bioinfo".
```bash
cat sample
grep ^a sample
grep -E p\{2} sample
grep "a\+t" sample
sed "s/a/XXX/g" sample
```

Descriptions | Symbol
--- |  ---
replaces any character | .
matches start of string | ^
matches end of string | $
matches up zero or more times the preceding character | *
Represent special characters | \
Groups regular expressions | ()
Matches up exactly one character | ?

### Checking the nucleotides in the Ensembl fasta file

Now let's use regulare expressions to explore the fasta file. Have a look at the first few lines
```bash
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -50
```

We expect all DNA sequences to be made up of A, C, T, and Gs. Now, we will verify this. First, we will get all DNA-sequences from this file, excluding lines starting with a ">" (in a fasta files, these lines are the lines describing the sequence: https://en.wikipedia.org/wiki/FASTA_format).
```bash
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | grep -v ">"

# from "man grep": -v Invert the sense of matching, to select non-matching lines.
```

Next we will use "tr" to delete newline characters. This will place all sequences in one (very long) line.
```bash
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | grep -v ">" | tr -d '\n'
```

Next we place every character on a different line.
```bash
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | grep -v ">" | tr -d '\n' | grep -o .
```

Finally, we want to remove repetition, to only see the unique nucleotides. To do so, we have to first sort the lines and then make them unique.
```bash
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | grep -v ">" | tr -d '\n' | grep -o . | sort | uniq

# what happens if we only make them unique?
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | grep -v ">" | tr -d '\n' | grep -o . | head -20
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | grep -v ">" | tr -d '\n' | grep -o . | head -20 | uniq
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | grep -v ">" | tr -d '\n' | grep -o . | wc -l
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | grep -v ">" | tr -d '\n' | grep -o . | uniq | wc -l
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | grep -v ">" | tr -d '\n' | grep -o . | sort | uniq | wc -l
```

#### Exercises
- Create a folder "day3" in your home directory
- Count the number of A, C, T, and G of the DNA sequences of the first 1000 lines of file Homo_sapiens.GRCh38.cds.all.fa.gz
- Store each number in a file called nt_A.txt, nt_C.txt,... in the folder day3

### Reformatting the Ensembl fasta file

Next, we will reformat the fasta file, such that every entry is on one line and the DNA sequence is at the end of the line. 

Remind ourselves of how this file looks like:
```bash
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500
```

The ">" separates different entries and entries should remain each on one line, see here: https://en.wikipedia.org/wiki/FASTA_format. Next we will remove all newline characters, except for the newline characters followed by ">". To remove the newline characters, we will use the "sed" command.
```bash
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | sed -z 's/\n[^>]/ /g'
```

Next, we will place "seq:" in front of the DNA-sequence of each line. 
```bash
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | sed -z 's/\n[^>]/ /g' | sed -E 's/([ACTG]*)$/seq:\1/g'
```

The argument `s/([ACTG]*)$/seq:\1/g` for sed is quite complicated, let's break it up:

- `s/x/y/g` means we substitute `x` by `y`
- `s///` tells sed to substitute
- `s///g` tells sed to substitue globally - replacing each occurance of `x`
- `([ACTG]*)` matches any number of the four letters A, C, T, and G. The brackets `()` tell the regex to *store* the match for later (see `\1` below)
- `$` matches the end of the line, telling the regex that the `([ACTG]*)` has to be at the end of the line (our DNA sequences)
- `seq:` is simply the text we want to insert
- `\1` is going to be replaced by the match to `([ACTG]*)` stored. Therefore, here it will be replaced by the DNA sequence
- in summary, the expression matches any number of the letters A, C, T, and G at the end of the line, stores them, and then write "seq:" and places the sequence behind it


We developed the above approach on the first 30 lines. Now run it an the full file. This may take a couple of minutes. The "gzip" command will compress the results. The `>` will store it in a new file "GRCh38_reformatted.gz".
```bash
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | sed -z 's/\n[^>]/ /g' | sed -E 's/([ACTG]*)$/seq:\1/g' | gzip > GRCh38_reformatted.gz
```

Explore the generated file.
```bash
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | wc -l
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | grep ">" | wc -l
gunzip -c GRCh38_reformatted.gz | wc -l
zless GRCh38_reformatted.gz
```


### Identifying gRNA matches

Now, with the newly formatted file, we can easily identify genes in the database that match a specific gRNA. 
```bash
gunzip -c GRCh38_reformatted.gz | head -30
```

We check that all entries have "gene symbols"
```bash
gunzip -c GRCh38_reformatted.gz | grep "gene_symbol" | head -30 
gunzip -c GRCh38_reformatted.gz | grep -v "gene_symbol" | head -30 

# remember: grep -v means to *not* match
```

Next we extract those sequences matching the guide "TTAAGACA"
```bash
gunzip -c GRCh38_reformatted.gz | grep "seq:[ACTG]*TTAAGACA" | head -30 
```

Now we extract the gene symbols of these matches. First we match all text (`.*`) from the beginning of the line (`^`) before the text "gene_symbol" and delete it.
```bash
gunzip -c GRCh38_reformatted.gz | grep "seq:[ACTG]*TTAAGACA" | sed 's/^.*gene_symbol://g' | head -30
```

Then we also remove all text after the gene symbol, in this case the space and everything (`.*`) after that until the end of the line (`$`).
```bash
gunzip -c GRCh38_reformatted.gz | grep "seq:[ACTG]*TTAAGACA" | sed 's/^.*gene_symbol://g' | sed 's/ .*$//g'  | head -30
```

Finally, we get unique gene IDs.
```bash
gunzip -c GRCh38_reformatted.gz | grep "seq:[ACTG]*TTAAGACA" | sed 's/^.*gene_symbol://g' | sed 's/ .*$//g'  | sort | uniq
```


#### Exercises

Create the following files in the folder "day3":

- Extract all entries with sequences matching the guide "GCGGTTTC" in the file guideMatch_GCGGTTTC.txt
- Write the count of entries with sequences starting with "TGC" into the file count_TGC.txt
- Write the count of entries of gene "MMP2" into the file count_MMP2.txt. Note: Do not count genes MMP20, MMP21,...
- Write the count of unique genes whose symbol starts with "RPL" into the file count_RPL.txt
- Write the count of all unique protein coding genes into the file count_protein_coding.txt

---------------
END OF DAY 3
---------------

# Loops and variables
(Day 4)

### Variables

Variables can hold information which can be passed to different programs
```bash
x="test variable"
echo $x
echo $x | sed 's/a//g'
```

Now we will write the gRNAs to test and the regexp patterns into variables
```bash
guide=TTAAGACA
echo $guide
pattern="seq:[ACTG]*${guide}"
echo $pattern
gunzip -c GRCh38_reformatted.gz | grep $pattern | head -30
gunzip -c GRCh38_reformatted.gz | grep $pattern | sed 's/^.*gene_symbol://g' | sed 's/ .*$//g' | head -30
```


### Loops

We will use `while` loops. They have the following syntax: `while [condition] do [something] done`. Different types of loops exist: https://ryanstutorials.net/bash-scripting-tutorial/bash-loops.php but are not important for this course.

The following loop starts with x = 1. It then repeats printing "Welcome ... times" and adding +1 to x, until x is greater than 5 (while x is lower or equal to 5 `$x -le 5`).
```bash
x=1
while [ $x -le 5 ]
do
  echo "Welcome $x times"
  x=$(( $x + 1 )) # syntax to add a number to x
done
```

Importantly, the loop is ONE construct. Thus when entering `while [ $x -le 5 ]`, the terminal expects additional information, in particular `do [...] done`. While waiting for additional input, the terminal will start lines with `>`. Keep entering the `do`, etc in those lines. The final results will look like this:
```
[user]@corso:~$ while [ $x -le 5 ]
> do
> echo "Welcome $x times"
> x=$(( $x + 1 ))
> done
Welcome 1 times
Welcome 2 times
Welcome 3 times
Welcome 4 times
Welcome 5 times
```

Try to make the above loop count to 10. Or from 10 to 5. 

Here are different ways to compare numbers. Assume variable a holds 10 and variable b holds 20 then:

Operator | Description | Example
 --- | --- | ---
-eq | Checks if the value of two operands are equal or not; if yes, then the condition becomes true. | [ $a -eq $b ] is not true.
-ne | Checks if the value of two operands are equal or not; if values are not equal, then the condition becomes true. | [ $a -ne $b ] is true.
-gt | Checks if the value of left operand is greater than the value of right operand; if yes, then the condition becomes true. | [ $a -gt $b ] is not true.
-lt | Checks if the value of left operand is less than the value of right operand; if yes, then the condition becomes true. | [ $a -lt $b ] is true.
-ge | Checks if the value of left operand is greater than or equal to the value of right operand; if yes, then the condition becomes true. | [ $a -ge $b ] is not true.
-le | Checks if the value of left operand is less than or equal to the value of right operand; if yes, then the condition becomes true. | [ $a -le $b ] is true.


We can also loop through the content of a file.
```bash
while read p; do
  echo "$p"
done <gRNAs.txt
```

#### Exercise

Now we will combine the above variables and loopes to test all guides in file gRNAs.txt against all DNA sequences in GRCh38_reformatted.gz. To do so:

- Use the function above (under "variables") that matches guides against DNA sequences and extracts gene symbols for matched sequences. 
- Place this function into the loop that iterates through the file gRNAs.txt, using the while loop iterating through the file.
- Store all results (not just the top 30 coming from head -30) of each guide into a file that is named: `results_${guide}.txt`. Place all files into a new folder "day4" - ideally already within the loop.
- Check a few examples by hand. Do you get the right genes? Do you get the correct number of genes for the guides?

UPDATE:
The above exercise does not produce the desired results (it produces 0 matches for almost all guides as they are too long). To fix this, use an updated file of gRNAs instead in the following exercises. You can copy the corrected file to your home directory using this command:
```bash
cp /home/nfortelny/gRNAs_CORRECTED.txt ~/
```

To complete the exercise, you can use the following code to extract genes for one guide. Note: The guide needs to be defined first using `guide=[...]`
```bash
echo $guide
pattern="seq:[ACTG]*${guide}"
echo $pattern
gunzip -c GRCh38_reformatted.gz | grep $pattern | sed 's/^.*gene_symbol://g' | sed 's/ .*$//g' | sort | uniq
```

Place the above in the loop that iterates throught the gRNA file. 
```bash
while read guide; do
  echo $guide
done <gRNAs_CORRECTED.txt
```
Remember: you need to save the result of the last line (`gunzip ... | uniq`) to a file `results_${guide}.txt`.


### Comparing files

Have a look at the files we generated.
```bash
cd ~/day4
ls results_*.txt
ls -l results_*.txt
head results_*.txt
wc -l results_*.txt
```

Now let's compare results in a pairwise manner. The command `comm` will provide the gene symbols unique to one or the other file, plus the intersection of both.
```bash
cd ~/day4
comm results_AAGTTGGC.txt results_GCCATACA.txt
comm -12 results_AAGTTGGC.txt results_GCCATACA.txt
```

The count of genes in the overlap (intersection) can be stored in a new variable. To store results of an expression in a new variable, place the expression into parenthesis `x=$()`.
```bash
cd ~/
guide1=AAGTTGGC
guide2=GCCATACA
overlap=$(comm -12 day4/results_${guide1}.txt day4/results_${guide2}.txt | wc -l)
echo "$guide1 $guide2 $overlap"
```

#### Exercise

Now we will compare each pair of guides to test the overlap of genes. Place the result in the folder "day4":

- Use one loop (iterating through all guides) within another loop (also iterating through all guides) to compare the overlap between all pairs of guides
- Use the "comm" command as shown above to extract the overlap, counting the number of genes, and writing the result into a file named `overlap_${guide1}_${guide2}.txt`.
- "Manually" check results, for example comparing to the example above ("AAGTTGGC" vs "GCCATACA")

---------------
END OF DAY 4
---------------

# Useful links
Linux for bioinformatics

- https://bioinformaticsworkbook.org/Appendix/Unix/unix-basics-1.html#gsc.tab=0
- https://bioinformaticsworkbook.org/Appendix/Unix/UnixCheatSheet.html#gsc.tab=0
- https://bioinformatics.uconn.edu/unix-basics/#
- https://decodebiology.github.io/bioinfotutorials/
- https://www.melbournebioinformatics.org.au/tutorials/tutorials/unix/unix/

General linux commands

- https://www.thegeekstuff.com/2010/11/50-linux-commands/

Regular expressions:

- https://www.guru99.com/linux-regular-expressions.html