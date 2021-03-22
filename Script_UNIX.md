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
touch day1/touch.txt
echo "test"  > day1/echo.txt
```

List files again
```
ls *
ls -l *
head day1/*
```


# Files and file systems


Download file and look at it.

Copy file of profiles from /home/bioinfo1

mv 

vs

cp

pwd

scp?

wc -l

https://www.thegeekstuff.com/2010/11/50-linux-commands/

nano

wget http://ftp.ensembl.org/pub/release-103/fasta/homo_sapiens/cds/Homo_sapiens.GRCh38.cds.all.fa.gz

head Homo_sapiens.GRCh38.cds.all.fa.gz

clear

gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head

zless Homo_sapiens.GRCh38.cds.all.fa.gz

du *
du -sh *

Pipe command	| Function
--- | ---
cmd < file | use file as input
cmd > file | write output to file
cmd >> file | append output to file
cmd 2> stderr | error output to file
cmd 1>&2 file | send output and error to file
cmd1 \| cmd2 | send output of cmd1 to cmd2




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

