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
mkdir -p day2/test/folder
cd day2/test/folder
pwd
```

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
mv /home/bioinfo1/profiles.txt ~/
cp /home/bioinfo1/profiles.txt ~/

# Why does cp not work? Look at file permissions:
ls -l /home/bioinfo1/profiles.txt
ls -l ~/profiles.txt
```

Now  explore the copied file
```
head profiles.txt
head -5 profiles.txt
tail -5 profiles.txt
less profiles.txt
more profiles.txt

# Count the number of lines
wc -l profiles.txt
```

<!-- TODO: Split here? -->

### Editing in nano

Now add a line at the end of this file. Add the profiles "TODO". To quit the nano-editor you need too press Ctrl+X. Then type Y+Enter to save the changes.
```
nano profiles.txt
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
wget http://ftp.ensembl.org/pub/release-103/fasta/homo_sapiens/cds/Homo_sapiens.GRCh38.cds.all.fa.gz
```

Have a look at this file
```
head Homo_sapiens.GRCh38.cds.all.fa.gz
```

How large are those files?
```
du *
du -sh *
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
cmd 1>&2 file | send output and error to file
cmd1 \| cmd2 | send output of cmd1 to cmd2


Let's have a look at the first few lines
```
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head
```

Some programs let you look at decompressed output, for example
```
zless Homo_sapiens.GRCh38.cds.all.fa.gz
```

Or count the number of lines in this file
```
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | wc -l
```

# REGEXP
cat sample
grep ^a sample
grep -E p\{2} sample
grep "a\+t" sample

gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -500 | grep -v ">" | tr -d '\n' | grep -o . | sort | uniq

gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -30 | sed -z 's/\n[^>]/ /g' | sed -E 's/([ACTG]*)$/seq:\1/g'

gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head -30 | sed -z 's/\n[^>]/ /g' | sed -E 's/([ACTG]*)$/seq:\1/g' | gzip > Data.gz

gunzip -c Data.gz | sed 's/.*seq://g'

gunzip -c Data.gz | cut -d$" " -f7,14

x=TAC
x2="seq:[ACTG]*$x"
echo $x2
<!-- gunzip -c Data.gz | grep "seq:[TAC]*CGAC" | grep "gene_symbol" | sed 's/^.*gene_symbol://g' | sed 's/ .*$//g' -->
gunzip -c Data.gz | grep $x2 | grep "gene_symbol" | sed 's/^.*gene_symbol://g' | sed 's/ .*$//g' | sort | uniq > "result_${x}.txt"

x=TT
x2="seq:[ACTG]*$x"
echo $x2
<!-- gunzip -c Data.gz | grep "seq:[TAC]*CGAC" | grep "gene_symbol" | sed 's/^.*gene_symbol://g' | sed 's/ .*$//g' -->
gunzip -c Data.gz | grep $x2 | grep "gene_symbol" | sed 's/^.*gene_symbol://g' | sed 's/ .*$//g' | sort | uniq > "result_${x}.txt"

diff result_TAC.txt result_TT.txt 




comm result_TAC.txt result_TT.txt 
comm -13 result_TAC.txt result_TT.txt 

xargs

# Loops and conditions

Write your attempts in a separate file

# Useful links:
- https://bioinformaticsworkbook.org/Appendix/Unix/unix-basics-1.html#gsc.tab=0
- https://bioinformaticsworkbook.org/Appendix/Unix/UnixCheatSheet.html#gsc.tab=0
- https://bioinformatics.uconn.edu/unix-basics/#
- https://decodebiology.github.io/bioinfotutorials/
- https://www.melbournebioinformatics.org.au/tutorials/tutorials/unix/unix/


https://www.thegeekstuff.com/2010/11/50-linux-commands/

nano
scp?