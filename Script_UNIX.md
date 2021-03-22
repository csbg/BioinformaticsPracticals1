
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

List content again
```
ls *
ls -l *
```

There are many ways to create a file
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



# Navigating the file system
pwd

wget http://ftp.ensembl.org/pub/release-103/fasta/homo_sapiens/cds/Homo_sapiens.GRCh38.cds.all.fa.gz
head Homo_sapiens.GRCh38.cds.all.fa.gz
clear
gunzip -c Homo_sapiens.GRCh38.cds.all.fa.gz | head
zless Homo_sapiens.GRCh38.cds.all.fa.gz

du *
du -sh *



# REGEXP
cat sample
grep ^a sample
grep -E p\{2} sample
grep "a\+t" sample


# Useful links:
- https://bioinformaticsworkbook.org/Appendix/Unix/unix-basics-1.html#gsc.tab=0
- https://bioinformaticsworkbook.org/Appendix/Unix/UnixCheatSheet.html#gsc.tab=0
- https://bioinformatics.uconn.edu/unix-basics/#
- https://decodebiology.github.io/bioinfotutorials/
- https://www.melbournebioinformatics.org.au/tutorials/tutorials/unix/unix/

