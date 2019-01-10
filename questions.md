# Questions

## HPC good citizenship

1. On the UCI cluster, the resource request "-pe openmp 64" refers to the number of processors requested.  Does that
   request mean that your commands will use multiple processors?
   
This request "-pe openmp 64" is specified to try to request all 64 cores in a parallel environment to run a program/job  on the same node. It will use multiple processors only if the software you are using is capable of doing so and those are available on the HPC.
  
2. In general, how do you know how many processors, how much RAM, how many files would be required/needed/written by the
   jobs you are running on the cluster?
   
The time command with the verbose option will list out this information for a command that is run

```
/usr/bin/time -v commandgoeshere
```

Which outputs:

```
* User time (seconds): 72.76
      * System time (seconds): 3.28
      * Percent of CPU this job got: 93%
      * Elapsed (wall clock) time (h:mm:ss or m:ss): 1:21.48
        Average shared text size (kbytes): 0
        Average unshared data size (kbytes): 0
        Average stack size (kbytes): 0
        Average total size (kbytes): 0
      * Maximum resident set size (kbytes): 3745120
        Average resident set size (kbytes): 0
      * Major (requiring I/O) page faults: 0
      * Minor (reclaiming a frame) page faults: 233595
        Voluntary context switches: 17852
        Involuntary context switches: 24019
      * Swaps: 0
      * File system inputs:   496560
      * File system outputs: 2878576
      * Socket messages sent: 0
      * Socket messages received: 0
        Signals delivered: 0
        Page size (bytes): 4096
      * Exit status: 0
```
  
This lists out information on the run, including the user & system time; the maximum resident set size which shows how much RAM is used; and output size which relates to how much data it is writing.

3. In order to be a "good citizen", you need to have some idea of much RAM your job requires.  In particular, you need
   to know the "peak" (i.e., maximum) RAM required at any point during execution.  Show an example of the shell command
   that you would use on a Linux machine to measure run time and peak ram usage of an arbitrary command, where the time/peak            RAM values are written to a file.

You could run your application/command on a small set of input data/subset of input data and scale up from there, prefixed by:

```
/usr/bin/time -v commandhere 2>&1 outfile.txt
```

The output is given through STDERR so we need to redirect STDERR to a file with the 2>&1. Based on the values given and the initial size of your subsetted input file vs the total size of the original file you could estimate the time and RAM required for the actual job.

   
4. What are the units of your answer for number 3?

User and system time is reported in seconds

Elapsed wall clock time is hour:min:sec

Maximum resident set size values are reported in kbytes

5. What are the bash commands for the following operations:

    * Checking that a file exists
    
```
if [ -e txt.md ]
then
    echo "Exists"
else
    echo "Does not exist"
fi
```
    
   * Checking that a file exists and is not empty
    
```
if [ -e test.md ]
then
  if [ -s test.md ]
  then
   echo "Exists and is full"
   else
    echo "Exists but is empty"
fi
else
  echo "Does not exist"
fi
```

6. How would you use the commands from your answer to 5 to write a work flow for HPC that only runs a job if the
   expected output file is **not** present.

```
if [ -e test.txt ]
then
   echo "test.txt is here"
   else
   executecommandhere.py
fi
```

## Trickier questions

7. Would your answer to number 3 work on Apple's OS X operating system?  If no, do you have any idea why not? 

No, the exact answer provided in number 3 does not work for mac. It only gives the time elapsed in seconds but not the  additional information about the memory used/size of written files or peak RAM usage. The verbose option is not available in mac. As an alternative, one could install gnu-time and use the gtime command to get a similar verbose output as question 3.

8. Most of the HPC nodes have 512Gb (gigabytes) of RAM. Let's say you have a job that will require **no more** than 24Gb
   of RAM.  How would you request resources so that you can run more than one job on a node **and** not cause nodes to
   crash?  Show an example of a skeleton HPC script as part of your answer.  **Knowing how to do this is super important
   and will save you loads of frustration and prevent you from taking out your colleagues' jobs on the cluster,
   preventing you from getting nasty emails from Harry!!!!!!!!!!!**

```
#!/bin/bash
#$ -q bio
#$ -N TEST
#$ -l mem_free=24GB
```
