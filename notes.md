To get new course files added to your repository later, you will need to add the original repository (the one you forked) as a 'remote' [see here for help](https://stackoverflow.com/questions/3903817/pull-new-updates-from-original-github-repository-into-forked-github-repository),[and here](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo)  
To add updates/new files from the gen711-811 repo, copy and paste these lines into terminal on RON:
```
cd $HOME/gen711-811
git remote add upstream https://github.com/jthmiller/gen711-811.git
git fetch upstream
git merge upstream/master master
```
The first line is to get you back to your home directory just in case you switched. 
The second is to go looking for any changes that I might have made in my copy of 'gen711-811'
The third is to get any of those changes
The fourth is to merge my changes 'upstream/master' with your 'master'

Note, git merge is like "git pull" which is fetch + merge. Or, better, you can replay your local work on top of the fetched branch like a "git pull --rebase"
```
git rebase upstream/master
```



1. Ensure all your local changes are committed to your current branch.
- Save all your vscode additions, and commit them. 
2. Fetch the latest updates from the remote:
```git fetch origin```
3. Rebase your changes onto the updated remote branch
```git rebase origin/main```
4. Resolve any conflicts: If conflicts arise during the rebase, Git will pause the process and prompt you to resolve them. After editing the files to resolve the conflicts, use:
```
git add .
git rebase --continue
```
#### Resolving Conflicts
In both scenarios, if the same lines of code were changed in both your local work and the remote updates, Git will indicate a conflict. You must manually edit the conflicted files to choose which changes to keep. Your editor will show markers (like <<<<<<< and >>>>>>>) to help you identify the conflicting sections. 
After resolving the conflicts, add the file(s) and continue the operation as described above. 



Metadata - commas, spaces, tabs, special characters all bad
wget/curl get files from web
(command d) higlights every instance of a thing

> functions as a pipe to a location, will print to the file designated
> can also start a path, such as > ~/ to stick it in your home directory
>> will do it again (called appending), sticks two outputs together
wc -l is line count
piping greps into 'less' lets you scroll through the results
control R -v will show the opposite of your grepped less readout, displaying only those reads without the searched-for sequence
grep ^@ excludes quality lines with @'s, will find each sequence (every four lines)

for-loops 
for name in *.fastq
do
echo ${name}
done 

^ vairable names change based on your files
'for', 'do', and 'done' are the three important bits
braces for showing variable, quotes also work


1. Get practical exam into VS Code
cd ~/gen711
git clone <REPO_URL>
code <REPO_NAME>
2. Make 'analysis' directory from home
mkdir ~/analysis
3. Copy FASTQ files without changing directory
cp ../../Data/WELMw0518231_S614_L002_R1_001.fastq.gz ~/GEN711_811_Practical-Exam/analysis/
cp ../../Data/WELMw0518231_S614_L002_R2_001.fastq.gz ~/GEN711_811_Practical-Exam/analysis/

4. Change directory using absolute path
cd ~/home/users, etc/analysis

5. View top 4 lines of FASTQ
head -n 4 Sample1.fastq
8. Count reads with ≥15 Ns (no new file)
grep -E 'N{15,}' Sample1.fastq | wc -l
grep -E 'N{15,}' Sample2.fastq | wc -l

9. Create 'to_blast' and move FASTA files
mkdir to_blast
mv *.fasta to_blast/

10. Confirm files moved (without changing directory)
ls to_blast

11. Get 100th line of Sample1.fasta
head -n 100 to_blast/Sample1.fasta | tail -n 1

12. Run md5sum + save output
md5sum to_blast/Sample1.fasta
md5sum to_blast/Sample1.fasta > my_md5sums.txt

13. Append Sample2 md5sum
md5sum to_blast/Sample2.fasta >> my_md5sums.txt
14. Add your name
echo "MYNAME" >> my_md5sums.txt
Replace MYNAME with your real name.

Extra Credit (FastQC)
fastqc Sample1.fastq
fastqc to_blast/Sample1.fasta
Answer:
FASTQ works  (contains quality scores)
FASTA may fail or give warnings  (no quality scores)
Why: FastQC is designed for FASTQ format, which includes base quality information.

pwd — Print current directory
ls — List files
ls -l  - detailed view
ls -a  - show hidden files
cd — Change directory
cd ..   go up one level
mkdir — Make directory
mkdir analysis
mkdir -p dir1/dir2   - create nested dirs
cp — Copy files
cp file.txt backup.txt
cp file.txt ~/analysis/
cp *.fastq analysis/
mv — Move or rename
mv file.txt newname.txt
mv *.fasta to_blast/
rm — Delete
rm file.txt
rm -r folder/        # delete directory
Viewing Files - 
head — First lines
head file.txt
head -n 10 file.txt
tail — Last lines
tail file.txt
tail -n 5 file.txt
less — Scrollable view
less file.txt
cat — Print entire file
cat file.txt
Searching & Pattern Matching - 
grep — Search text
grep "ACTG" file.txt
grep -i "actg" file.txt     # ignore case
grep -c "ACTG" file.txt     # count matches
grep -E 'N{15,}' file.txt   # regex (15+ Ns)
Counting & Summaries - 
wc — Word/line count
wc file.txt
wc -l file.txt     # count lines
 Pipes & Redirection - 
Pipe |
Send output of one command into another
cat file.txt | grep ACTG
Redirect output >
ls > output.txt
Append output >>
echo "hello" >> file.txt

 FASTQ / FASTA Processing - 
FASTQ Structure (4 lines per read)
Header (@)
Sequence
+
Quality
Extract FASTA from FASTQ
Using awk (most important)
awk 'NR%4==1 {print ">" substr($0,2)} NR%4==2 {print}' file.fastq
Explanation:
NR%4==1 → header lines
NR%4==2 → sequence lines
substr($0,2) removes @
Preview output with head
awk 'NR%4==1 {print ">" substr($0,2)} NR%4==2 {print}' file.fastq | head
 Advanced Text Processing (AWK)
Print specific columns
awk '{print $1}' file.txt
Conditional filtering
awk '$1 > 10' file.txt
Regular Expressions (Regex Essentials) - 
Pattern
Meaning
.
any character
*
0 or more
+
1 or more
{15,}
15 or more
^
start of line
$
end of line

Example:
grep -E 'N{15,}' file.fastq
Line Selection Tricks - 
Get specific line (e.g., 100th line)
head -n 100 file.txt | tail -n 1
Checksums & File Integrity
md5sum
md5sum file.txt
md5sum file.txt > sums.txt
md5sum file2.txt >> sums.txt
Wildcards 
*.fastq        # all fastq files
Sample*        # files starting with "Sample"
Command History Shortcuts
Previous command
↑  (up arrow)
Repeat last command
!!
Quality Control (FastQC)
fastqc file.fastq
fastqc file.fasta
FASTA may fail (no quality scores)
Absolute vs Relative Paths
Absolute
cd /home/user/analysis
Relative
cd analysis
 Permissions - 
chmod +x script.sh
Combining Commands -
Example pipeline
grep -E 'N{15,}' file.fastq | wc -l
(Find reads with ≥15 Ns and count them)

Examples - 
Copy without changing directory
cp /path/to/file ~/analysis/
Convert + save file
awk 'NR%4==1 {print ">" substr($0,2)} NR%4==2 {print}' file.fastq > file.fasta
Check files in another directory
ls other_directory/

Add name to file
echo "Your Name" >> file.txt
Common Mistakes to Avoid - 
Forgetting / in paths
Using > instead of >> (overwrites file!)
Running commands in wrong directory
Forgetting quotes in awk or grep
Mixing up FASTQ vs FASTA format
The core jawns - 
cd
ls
cp
mv
mkdir
head
tail
grep
wc -l
awk
|   >   >>

(Repo = prac_exam)
1. Change directory using an absolute path
cd ~/prac_exam
2. Create nested directory structure in one command
mkdir -p data/untrimmed_fastq
3. Copy fastq files without changing directory
cp /tmp/Gen711-811_data/*.fastq.gz data/untrimmed_fastq/
4. List all hidden files
ls -a
5. Change directory using a relative path
cd data/untrimmed_fastq
6. View top 4 lines of a compressed FASTQ
zcat SRR2584863_1.fastq.gz | head -n 4

Example output:

@SRR2584863.1 ...
GATCGGAAGAGCACACGTCTGAACTCCAGTCAC
+
FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF
7. File sizes (uncompressed, readable)
du -h *.fastq.gz

(If truly uncompressed .fastq files exist, use du -h *.fastq instead)

Example output:

1.2G SRR2584863_1.fastq.gz
1.3G SRR2584863_2.fastq.gz
8. Count quality lines containing '@'

(FASTQ quality lines are every 4th line)

zcat SRR2584863_1.fastq.gz | awk 'NR%4==0' | grep -c '@'
zcat SRR2584863_2.fastq.gz | awk 'NR%4==0' | grep -c '@'
9. Count reads with ≥15 Ns (no file creation)
zcat SRR2584863_1.fastq.gz | awk 'NR%4==2' | grep -c 'N\{15,\}'
10. Create badreads.fasta
zcat SRR2584863_1.fastq.gz | paste - - - - | awk '$2 ~ /N{15,}/ {print "@"$1"\n"$2}' > badreads.fasta

zcat SRR2584863_2.fastq.gz | paste - - - - | awk '$2 ~ /N{15,}/ {print "@"$1"\n"$2}' >> badreads.fasta
11. Activate conda environment + locate fastqc
conda activate genomics
which fastqc

Example output:

/home/user/miniconda3/envs/genomics/bin/fastqc
12. Run fastqc and organize results (no directory change)
fastqc SRR2584863_1.fastq.gz
mkdir -p ../results/fastqc_untrimmed_reads
mv SRR2584863_1_fastqc.* ../results/fastqc_untrimmed_reads/
13. Get 100th line from another directory (no cd)
sed -n '100p' ../trimmed_fastq/SRR2584863_1.trim.fastq
14. md5sum workflow
md5sum SRR2584863_1.fastq.gz
md5sum SRR2584863_1.fastq.gz > my_md5sums.txt
md5sum SRR2584863_2.fastq.gz >> my_md5sums.txt
echo "Jasper Clement" >> my_md5sums.txt
15. Push to GitHub (VSCode sidebar equivalent CLI)
git add .
git commit -m "Added analysis outputs and badreads"
git push origin main


More stuff  - 
File & Directory
pwd - show current directory
tree - visualize directory structure
cp -r dir1 dir2 - copy directories
mv file newname - rename file
rm -r dir - remove directory
Viewing Files
less file - scroll through file
head -n 10 file
tail -n 10 file
wc -l file - count lines
Compression
gzip file
gunzip file.gz
zcat file.gz  -read compressed file
Searching & Filtering
grep "pattern" file
grep -i "pattern" file - case-insensitive
grep -v "pattern" file - invert match
awk / text processing
awk '{print $1}' file
awk 'NR==10' file
awk 'NR%4==1' fastq - FASTQ headers
Pipes & Redirection
command1 | command2
> file - overwrite
>> file - append
Permissions
chmod +x script.sh
ls -l
Environment & Paths
echo $PATH
which python
Useful One-Liners
cut -f1 file - extract column
sort file
uniq -c file
sort file | uniq -c - count duplicates

His Notes:
To change directories, use 'cd' and then hit tab two times to see directories in my current directory

Complete the questions below when intrstructed. Push the changes to this document to recive credit for attending the lab
1. What are 3 ways to change directories to your home directory from the untrimmed_fastq directory?
cd $HOME
cd ~
../../../
2. How many programs in /bin
Do each of the following tasks from your current directory using a single ls command for each:
List all of the files in /bin that start with the letter ‘c’. ls safdasfsla
List all of the files in /bin that contain the letter ‘a’.
List all of the files in /bin that end with the letter ‘o’.
Bonus: List all of the files in /bin that contain the letter ‘a’ or the letter ‘c’.
Answers here
Start with the letter c ls /bin/c* Start with the letter a ____ Start with the letter o ____ Contain the letter ‘a’ or the letter ‘c’ ____

What command/commands would you use to find the line number in your history for the command that listed all the '.fastq' files using the absolute path. Paste your answer below.
ls gdsfgsd

history | grep "/usr/bin.*\.sh"
!<line_number> 

Print out the contents of the ~/shell_data/untrimmed_fastq/SRR097977.fastq file. What is the last line of the file? : 
cat ~/shell_data/untrimmed_fastq/SRR097977.fastq
tail -n 1 ~/shell_data/untrimmed_fastq/SRR097977.fastq 

What are the next three nucleotides (characters) after the first instance of the sequence quoted above?

less ~/shell_data/untrimmed_fastq/SRR097977.fastq
Search inside less:
/TTTTTT
Answer: TTT

You can view file permissions using ls -l and change permissions using chmod.
The history command and the up arrow on your keyboard can be used to repeat recently used commands.
Conda environments simplify reporducability, dependencies and sharing environments.
::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: review

You can view file contents using less, cat, head or tail.
The commands cp, mv, and mkdir are useful for manipulating existing files and creating new directories.

Starting in the shell_data/untrimmed_fastq directory, do the following:

Make sure that you have deleted your backup directory and all files it contains. Create a backup of each of your FASTQ files using cp. (Note: You’ll need to do this individually for each of the two FASTQ files. We haven’t learned yet how to do this with a wildcard.) Use a wildcard to move all of your backup files to a new backup directory. Paste the code you used to do each step between the ''' below:

rm -Rf backup 

Change the permissions on all of your backup files to be write-protected.


chmod ug+rwx SRR097977.fastq

Use an absolute path to change your current working directory to the 'prac_exam' directory that you just cloned (2 points). $ cd /home/users/jwc1076/prac_exam_2026/

From 'prac_exam' directory, make the following directory structure in a single command: data/untrimmed_fastq (2 points, -1 point if you need to use 2 commands for this. Hint: There is a flag/option that lets you create nested directories all at once.) $ mkdir -p data/untrimmed_fastq

Copy the two fastq files in the /tmp/Gen711-811_data directly into your untrimmed_fastq directory without changing your current directory. (2 points, partial credit if you need to change directories first. Multipl~e correct answers) cp /tmp/Gen711-811_data/SRR2584863_2.fastq.gz ~ /home/users/jwc1076/prac_exam_2026/data/untrimmed_fastq/

List all the hidden files in this repo. Paste the command below (2 points) ls -a

Use a relative path to change your current working directory to the untrimmed_fastq directory. (2 points) cd/data/untrimmed_fastq

These are paired-end FASTQ files from an E. coli long-term evolution experiment. To confirm the files look ok, view one of them and paste the top 4 lines below. (4 points, Hint: These files are gzip-compressed. Multiple correct answers) head -n 4 SRR2584863_2.fastq.gz

@SRR2584863.1 HWI-ST957:244:H73TDADXX:1:1101:4712:2181/2 GGCGACATTACTGACCCGCNNNNNNNNNNNNNNNNNNNCGACNNNNNNNNNNNNNNNNNCCTGATNNNNNNNNNNNNNNNTCAGNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN + <<<??@??@??@@?@@??@###################################################################################################################################

How large (file size) are the two uncompressed fastq files? Use a single command with appropriate options to show the file sizes in a human-readable format (e.g., MB). Paste the command and output below. (2 points) wc -l SRR2584863_2.fastq 6213036 wc -l SRR2584866_2.fastq 11073592

For each fastq, how many quality score lines have the '@' symbol in them? To answer this, use one line of piped bash commands for each fastq, and the output should be a single number.(2 points) grep '@' SRR2584863_2.fastq | wc -l 2939966 grep '@' SRR2584866_2.fastq | wc -l 5094174

How many reads have 15 or more uncalled bases (NNNNNNNNNNNNNNN) in SRR2584863_1.fastq? Count WITHOUT making a new file (4 points) grep -E 'NNNNNNNNNNNNNNN' SRR2584863_2.fastq | wc -l 3015

Make a single fasta file from the two fastqs using the reads found in the question above, and their respective info lines. Name the new file 'badreads.fasta' in the 'untrimmed_fastq' directory. (4 points)

grep -E 'NNNNNNNNNNNNNNN' SRR2584863_2.fastq >> badreads.fasta grep -E 'NNNNNNNNNNNNNNN' SRR2584866_2.fastq >> badreads.fasta

Hint: the first 4 lines of badreads.fasta should look similar (but maybe not exactly) to this:

@SRR2584863.1 HWI-ST957:244:H73TDADXX:1:1101:4712:2181/2
GGCGACATTACTGACCCGCNNNNNNNNNNNNNNNNNNNCGACNNNNNNNNNNNNNNNNNCCTGATNNNNNNNNNNNNNNNTCAGNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN
@SRR2584863.2 HWI-ST957:244:H73TDADXX:1:1101:8571:2191/2
TCCCCGGAGTCAGCAGGGTGNNNNNNNNNNNNNNNNNATACATNNNNNNNNNNNNNNNGTTTTTGNNNNNNNNNNNNNNGCTGTCNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN
Activate the conda 'genomics' environment that contains fastqc and confirm where fastqc is installed. Paste the command and its output below. (2 points)
which fastqc /home/share/anaconda/envs/genomics/bin/fastqc

Run fastqc on SRR2584863_1.fastq. Then, create a results/fastqc_untrimmed_reads directory and move both the .zip and .html output files into it — all without leaving your untrimmed_fastq directory. Paste all commands used. (4 points)
fastqc SRR2584863_2.fastq mkdir -p 'results/fastqc_untrimmed_reads' mv SRR2584863_2_fastqc.zip ~/results/fastqc_untrimmed_reads mv SRR2584863_2_fastqc.html ~/results/fastqc_untrimmed_reads

Without changing directories, what is the 100th line of the file SRR2584863_1.trim.fastq in your trimmed_fastq directory? (2 points)
head -n 100 /home/users/jwc1076/prac_exam_2026/data/untrimmed_fastq/SRR2584863_2.fastq | tail -n 1

@@@FFFFDDFFHHJJIIGIJJGIHIJGGJJGGIIGHHHGEEHGIIGHIGIDGIBHHGHGFFFFFEDEEDD?BBD<A:@A>:3@>?(2989>:(4(45&000::@<A(8(&.09>1>@:::>CAA>>B>><55:(:@8?B###########

Run md5sum on SRR2584863_1.fastq. Then run it again, redirecting the output to a new file called my_md5sums.txt. Next, run md5sum on SRR2584863_2.fastq and append it to my_md5sums.txt. Finally, append your name to the end of my_md5sums.txt. Paste all commands used. (4 points)
md5sum SRR2584863_2.fastq md5sum SRR2584863_2.fastq >> my_md5sums.txt md5sum SRR2584866_2.fastq >> my_md5sums.txt echo 'Jasper' >> my_md5sums.txt

Push all of the new files that you created to your github repo using vscode's github side bar (4 points).

keypoints
grep is a powerful search tool with many options for customization.
>, >>, and | are different ways of redirecting output.
command > file redirects a command's output to a file.
command >> file redirects a command's output to a file without overwriting the existing contents of the file.
command_1 | command_2 redirects the output of the first command as input to the second command.
for loops are used for iteration.
basename gets rid of repetitive parts of names.
for fqname in *.fastq do fastqc $fqname done
echo SRR097977.fastq echo SRR098026.fastq
for filename in *.fastq do head -n 2 ${filename} done >> ~/file.txt
for filename in *.fastq do echo -e "name=$(basename filename.fastq)"echo−e"mv{filename} ${name}_2026.txt" done
for filename in *_2019.txt do name=$(basename filename2019.txt)mv{filename} ${name}.txt done

Search 1: GNATNACCACTTCC in one FASTQ
grep -B1 'GNATNACCACTTCC' SRR098026.fastq
-B1 prints the sequence ID line (line before match)
Search 2: AAGTT in both FASTQ files
grep -B1 'AAGTT' SRR098026.fastq SRR097977.fastq
Difference in results
One file: only matching lines from that file
Two files: output is prefixed with filenames, e.g.:
SRR098026.fastq:SEQUENCE
SRR097977.fastq:SEQUENCE
Keeping original FASTQ format

Problem: grep -B1 breaks the 4-line FASTQ structure

Solution: print full records (4 lines):

awk 'NR%4==1{h=$0} NR%4==2 && /AAGTT/{print h; print $0; getline; print; getline; print}' SRR098026.fastq
Make bad-reads.fastq (≥10 Ns in a row)
grep -B1 'N\{10,\}' SRR098026.fastq > bad-reads.fastq
Exercise 2: Number of sequences
wc -l SRR098026.fastq

Then divide by 4:

echo $(( $(wc -l < SRR098026.fastq) / 4 ))
Exercise 3: Sequences with ≥3 consecutive Ns
grep -c 'N\{3,\}' SRR098026.fastq
Exercise 4: Print file prefix of all .txt files
for f in *.txt; do echo "${f%.txt}"; done
Exercise 5: Remove _2026 from .txt files
for f in *_2026.txt; do mv "$f" "${f/_2026/}"; done
Exercise 6: Modify script

Add this line to bad-reads-script.sh:

echo "Script finished!"

Run the script:

bash bad-reads-script.sh
