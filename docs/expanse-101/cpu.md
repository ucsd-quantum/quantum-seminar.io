---
layout: default
title: Compiling and Running CPU Jobs
parent: Hands-on Examples
grand_parent: Expanse 101
nav_order: 6
---

## Compiling and Running CPU Jobs: <a name="comp-and-run-cpu-jobs"></a>
<b>Sections:</b>
* [Hello World (MPI)](#hello-world-mpi)
* [Hello World (OpenMP)](#hello-world-omp)
* [Running Hybrid (MPI + OpenMP) Jobs](#hybrid-mpi-omp)


### <a name="hello-world-mpi"></a>Hello World (MPI)
<b>Subsections:</b>
* [Hello World (MPI): Source Code](#hello-world-mpi-source)
* [Hello World (MPI): Compiling](#hello-world-mpi-compile)
* [Hello World (MPI): Interactive Jobs](#hello-world-mpi-interactive)
* [Hello World (MPI): Batch Script Submission](#hello-world-mpi-batch-submit)
* [Hello World (MPI): Batch Script Output](#hello-world-mpi-batch-output)


#### CPU Hello World: Source code: <#hello-world-mpi-source>
Change to the MPI examples directory (assuming you already copied the ):
```
[mthomas@expanse-ln3 expanse101]$ cd MPI
[mthomas@expanse-ln3 MPI]$ ll
[mthomas@expanse-ln3:~/expanse101/MPI] ll
total 498
drwxr-xr-x 4 mthomas use300      7 Apr 16 01:11 .
drwxr-xr-x 6 mthomas use300      6 Apr 15 20:10 ..
-rw-r--r-- 1 mthomas use300    336 Apr 15 15:47 hello_mpi.f90
drwxr-xr-x 2 mthomas use300      3 Apr 16 01:02 IBRUN
drwxr-xr-x 2 mthomas use300      3 Apr 16 00:57 MPIRUN_RSH
``
[mthomas@expanse-ln3 OPENMP]$cat hello_mpi.f90
!  Fortran example  
program hello
include 'mpif.h'
integer rank, size, ierror, tag, status(MPI_STATUS_SIZE)

call MPI_INIT(ierror)
call MPI_COMM_SIZE(MPI_COMM_WORLD, size, ierror)
call MPI_COMM_RANK(MPI_COMM_WORLD, rank, ierror)
print*, 'node', rank, ': Hello and Welcome to Webinar Participants!'
call MPI_FINALIZE(ierror)
end
```

Compile the code:
```
[mthomas@expanse-ln3 MPI]$ mpif90 -o hello_mpi hello_mpi.f90
[mthomas@expanse-ln3:~/expanse101/MPI] ll
total 498
drwxr-xr-x 4 mthomas use300      7 Apr 16 01:11 .
drwxr-xr-x 6 mthomas use300      6 Apr 15 20:10 ..
-rw-r--r-- 1 mthomas use300     77 Apr 16 01:08 compile.txt
-rwxr-xr-x 1 mthomas use300 750288 Apr 16 01:11 hello_mpi
-rw-r--r-- 1 mthomas use300    336 Apr 15 15:47 hello_mpi.f90
drwxr-xr-x 2 mthomas use300      3 Apr 16 01:02 IBRUN
drwxr-xr-x 2 mthomas use300      3 Apr 16 00:57 MPIRUN_RSH
```
Note: The two directories that contain batch scripts needed to run the jobs using the parallel/slurm environment.

* First, we should verify that the user environment is correct for running the examples we will work with in this tutorial.
```
[mthomas@expanse-ln3 MPI]$ module list
Currently Loaded Modulefiles:
1) intel/2018.1.163    2) mvapich2_ib/2.3.2
```
* If you have trouble with your modules, you can remove the existing environment (purge) and then reload them. After purging, the PATH variable has fewer path directories available:
```
[mthomas@expanse-ln3:~] module purge
[mthomas@expanse-ln3:~] echo $PATH
/home/mthomas/miniconda3/bin:/home/mthomas/miniconda3/condabin:/usr/lib64/qt-3.3/bin:/usr/local/bin:/bin:/usr/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/opt/sdsc/bin:/opt/sdsc/sbin:/opt/ibutils/bin:/opt/pdsh/bin:/opt/rocks/bin:/opt/rocks/sbin:/home/mthomas/bin
```
* Next, you reload the modules that you need:
```
[mthomas@expanse-ln3 ~]$ module load intel
[mthomas@expanse-ln3 ~]$ module load mvapich2_ib
```
* You will see that there are more binaries in the PATH:
```
[mthomas@expanse-ln3:~] echo $PATH
/opt/mvapich2/intel/ib/bin:/opt/intel/2018.1.163/compilers_and_libraries_2018.1.163/linux/bin/intel64:/home/mthomas/miniconda3/bin:/home/mthomas/miniconda3/condabin:/usr/lib64/qt-3.3/bin:/usr/local/bin:/bin:/usr/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/opt/sdsc/bin:/opt/sdsc/sbin:/opt/ibutils/bin:/opt/pdsh/bin:/opt/rocks/bin:/opt/rocks/sbin:/home/mthomas/bin
```

[Back to CPU Jobs](#comp-and-run-cpu-jobs) <br>
[Back to Top](#top)
<hr>

#### Hello World (MPI): Compiling: <a name="hello-world-mpi-compile"></a>

* Compile the MPI hello world code.
* For this, we use the command `mpif90`, which is loaded into your environment when you loaded the intel module above.
* To see where the command is located, use the `which` command:
```
[mthomas@expanse-ln3 MPI]$ which mpif90
/opt/mvapich2/intel/ib/bin/mpif90
```
* Compile the code:
```
mpif90 -o hello_mpi hello_mpi.f90
```

* Verify that the executable has been created:

```
[mthomas@expanse-ln3:~/expanse101/MPI] ll
total 498
drwxr-xr-x 4 mthomas use300      7 Apr 16 01:11 .
drwxr-xr-x 6 mthomas use300      6 Apr 15 20:10 ..
-rwxr-xr-x 1 mthomas use300 750288 Apr 16 01:11 hello_mpi
-rw-r--r-- 1 mthomas use300    336 Apr 15 15:47 hello_mpi.f90
drwxr-xr-x 2 mthomas use300      3 Apr 16 01:02 IBRUN
drwxr-xr-x 2 mthomas use300      3 Apr 16 00:57 MPIRUN_RSH
```

* In the next sections, we will see how to run parallel code using two environments:
* Running a parallel job on an _Interactive_ compute node
* Running parallel code using the batch queue system

[Back to CPU Jobs](#comp-and-run-cpu-jobs) <br>
[Back to Top](#top)
<hr>

#### Hello World (MPI): Interactive Jobs: <a name="hello-world-mpi-interactive"></a>

* To run MPI (or other executables) from the command line, you need to use the "Interactive" nodes.
* To launch the nodes (to get allocated a set of nodes), use the `srun` command. This example will request one node, all 24 cores, in the debug partition for 30 minutes:
```
[mthomas@expanse-ln3:~/expanse101/MPI] date
Thu Apr 16 01:21:48 PDT 2020
[mthomas@expanse-ln3:~/expanse101/MPI] srun --pty --nodes=1 --ntasks-per-node=24 -p debug -t 00:30:00 --wait 0 /bin/bash
[mthomas@expanse-14-01:~/expanse101/MPI] date
Thu Apr 16 01:22:42 PDT 2020
[mthomas@expanse-14-01:~/expanse101/MPI] hostname
expanse-14-01.sdsc.edu
```
* Note:
* You will know when you have an interactive node because the srun command
will return and you will be on a different host.
* Note:  If the cluster is very busy, it may take some time to obtain the nodes.  
* Once you have the interactive session, your MPI code will be allowed to execute on the command line.
```
[mthomas@expanse-14-01 MPI]$ mpirun -np 4 ./hello_mpi
node           0 : Hello and Welcome to Webinar Participants!
node           1 : Hello and Welcome to Webinar Participants!
node           2 : Hello and Welcome to Webinar Participants!
node           3 : Hello and Welcome to Webinar Participants!
[mthomas@expanse-14-01 MPI]$
```

When you are done testing code, exit the Interactive session.

[Back to CPU Jobs](#comp-and-run-cpu-jobs) <br>
[Back to Top](#top)
<hr>

#### Hello World (MPI): Batch Script Submission:  <a name="hello-world-mpi-batch-submit"></a>
To submit jobs to the Slurm queuing system, you need to create a slurm batch job script and
submit it to the queuing system.

* Change directories to the IBRUN directory using the `hellompi-slurm.sb` batch script:
```
[mthomas@expanse-ln3 MPI]$ cd IBRUN/
[mthomas@expanse-ln3 IBRUN]$ cat hellompi-slurm.sb
#!/bin/bash
#SBATCH --job-name="hellompi"
#SBATCH --output="hellompi.%j.%N.out"
#SBATCH --partition=compute
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=24
#SBATCH --export=ALL
#SBATCH -t 00:30:00

# load the user environment
source /etc/profile.d/modules.sh
module purge
module load intel
module load mvapich2_ib

#This job runs with 2 nodes, 24 cores per node for a total of 48 cores.
#ibrun in verbose mode will give binding detail

ibrun -v ../hello_mpi

```
* to run the job, use the command below:
```
[mthomas@expanse-ln3 IBRUN]$ sbatch hellompi.sb
Submitted batch job 32662205
```
* In some cases, you may have access to a reservation queue, use the command below:
```    
sbatch --res=SI2018DAY1 hellompi-slurm.sb
```

[Back to CPU Jobs](#comp-and-run-cpu-jobs) <br>
[Back to Top](#top)
<hr>

#### Hello World (MPI): Batch Script Output: <a name="hello-world-mpi-batch-output"></a>

* Check job status using the `squeue` command.
```
[mthomas@expanse-ln3 IBRUN]$ sbatch hellompi-slurm.sb; squeue -u username
Submitted batch job 18345138
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
32662205   compute hellompi  username PD       0:00      2 (None)
....

[mthomas@expanse-ln3 IBRUN]$ squeue -u username
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
32662205   compute hellompi  username  R       0:07      2 expanse-21-[47,57]
[mthomas@expanse-ln3 IBRUN]$ squeue -u username
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
32662205   compute hellompi  username CG       0:08      2 expanse-21-[47,57]
```
* Note: You will see the `ST` column information change when the job status changes: new jobs go into  `SP` (pending); after some time it moves to  `R` (running): when completed, the state changes to `CG` (completed)
* the JOBID is the job identifer and can be used to track or cancel the job. It is also used as part of the output file name.

* Look at the directory for and output file with the job id as part of the name:
```
[mthomas@expanse-ln3 IBRUN]$
total 48
drwxr-xr-x 2 mthomas use300    4 Apr 16 01:31 .
drwxr-xr-x 4 mthomas use300    7 Apr 16 01:11 ..
-rw-r--r-- 1 mthomas use300 2873 Apr 16 01:31 hellompi.32662205.expanse-20-03.out
-rw-r--r-- 1 mthomas use300  341 Apr 16 01:30 hellompi-slurm.sb
```

* To see the contents of the output file, use the `cat` command:
```
[mthomas@expanse-ln3 IBRUN]$ cat hellompi.32662205.expanse-20-03.out
IBRUN: Command is ../hello_mpi
IBRUN: Command is /home/username/expanse-examples/expanse101/MPI/hello_mpi
IBRUN: no hostfile mod needed
IBRUN: Nodefile is /tmp/0p4Nbx12u1

IBRUN: MPI binding policy: compact/core for 1 threads per rank (12 cores per socket)
IBRUN: Adding MV2_USE_OLD_BCAST=1 to the environment
IBRUN: Adding MV2_CPU_BINDING_LEVEL=core to the environment
IBRUN: Adding MV2_ENABLE_AFFINITY=1 to the environment
IBRUN: Adding MV2_DEFAULT_TIME_OUT=23 to the environment
IBRUN: Adding MV2_CPU_BINDING_POLICY=bunch to the environment
IBRUN: Adding MV2_USE_HUGEPAGES=0 to the environment
IBRUN: Adding MV2_HOMOGENEOUS_CLUSTER=0 to the environment
IBRUN: Adding MV2_USE_UD_HYBRID=0 to the environment
IBRUN: Added 8 new environment variables to the execution environment
IBRUN: Command string is [mpirun_rsh -np 48 -hostfile /tmp/0p4Nbx12u1 -export-all /home/username/expanse-examples/expanse101/MPI/hello_mpi]
node          18 : Hello and Welcome to Webinar Participants!
node          17 : Hello and Welcome to Webinar Participants!
node          20 : Hello and Welcome to Webinar Participants!
node          21 : Hello and Welcome to Webinar Participants!
node          22 : Hello and Welcome to Webinar Participants!
node           5 : Hello and Welcome to Webinar Participants!
node           3 : Hello and Welcome to Webinar Participants!
node           6 : Hello and Welcome to Webinar Participants!
node          16 : Hello and Welcome to Webinar Participants!
node          19 : Hello and Welcome to Webinar Participants!
node          14 : Hello and Welcome to Webinar Participants!
node          10 : Hello and Welcome to Webinar Participants!
node          13 : Hello and Welcome to Webinar Participants!
node          15 : Hello and Welcome to Webinar Participants!
node           9 : Hello and Welcome to Webinar Participants!
node          12 : Hello and Welcome to Webinar Participants!
node           4 : Hello and Welcome to Webinar Participants!
node          23 : Hello and Welcome to Webinar Participants!
node           7 : Hello and Welcome to Webinar Participants!
node          11 : Hello and Welcome to Webinar Participants!
node           8 : Hello and Welcome to Webinar Participants!
node           1 : Hello and Welcome to Webinar Participants!
node           2 : Hello and Welcome to Webinar Participants!
node           0 : Hello and Welcome to Webinar Participants!
node          39 : Hello and Welcome to Webinar Participants!
node          38 : Hello and Welcome to Webinar Participants!
node          47 : Hello and Welcome to Webinar Participants!
node          45 : Hello and Welcome to Webinar Participants!
node          42 : Hello and Welcome to Webinar Participants!
node          35 : Hello and Welcome to Webinar Participants!
node          28 : Hello and Welcome to Webinar Participants!
node          32 : Hello and Welcome to Webinar Participants!
node          40 : Hello and Welcome to Webinar Participants!
node          44 : Hello and Welcome to Webinar Participants!
node          41 : Hello and Welcome to Webinar Participants!
node          30 : Hello and Welcome to Webinar Participants!
node          31 : Hello and Welcome to Webinar Participants!
node          29 : Hello and Welcome to Webinar Participants!
node          37 : Hello and Welcome to Webinar Participants!
node          43 : Hello and Welcome to Webinar Participants!
node          46 : Hello and Welcome to Webinar Participants!
node          34 : Hello and Welcome to Webinar Participants!
node          26 : Hello and Welcome to Webinar Participants!
node          24 : Hello and Welcome to Webinar Participants!
node          27 : Hello and Welcome to Webinar Participants!
node          25 : Hello and Welcome to Webinar Participants!
node          33 : Hello and Welcome to Webinar Participants!
node          36 : Hello and Welcome to Webinar Participants!
IBRUN: Job ended with value 0
[mthomas@expanse-ln3 IBRUN]$
```
* Note the order in which the output was written into the output file. There is an entry for each of the 48 cores (2 nodes, 24 cores/node), but the output is not ordered. This is typical because the time for each core to start and finish its work is asynchronous.

[Back to CPU Jobs](#comp-and-run-cpu-jobs) <br>
[Back to Top](#top)
<hr>

### Hello World (OpenMP): <a name="hello-world-omp"></a>
<b>Subsections:</b>
* [Hello World (OpenMP): Source Code](#hello-world-omp-source)
* [Hello World (OpenMP): Compiling](#hello-world-omp-compile)
* [Hello World (OpenMP): Batch Script Submission](#hello-world-omp-batch-submit)
* [Hello World (OpenMP): Batch Script Output](#hello-world-omp-batch-output)


#### Hello World (OpenMP): Source Code <a name="hello-world-omp-source"></a>

Change to the OPENMP examples directory:
```
[mthomas@expanse-ln3 expanse101]$ cd OPENMP/
[mthomas@expanse-ln3 OPENMP]$ ls -al
total 479
drwxr-xr-x  2 username use300      6 Aug  5 22:19 .
drwxr-xr-x 16 username use300     16 Aug  5 19:02 ..
-rwxr-xr-x  1 username use300 728112 Aug  5 19:02 hello_openmp
-rw-r--r--  1 username use300    267 Aug  5 22:19 hello_openmp.f90
-rw-r--r--  1 username use300    310 Aug  5 19:02 openmp-slurm.sb
-rw-r--r--  1 username use300    347 Aug  5 19:02 openmp-slurm-shared.sb

[mthomas@expanse-ln3 OPENMP]$ cat hello_openmp.f90
PROGRAM OMPHELLO
INTEGER TNUMBER
INTEGER OMP_GET_THREAD_NUM

!$OMP PARALLEL DEFAULT(PRIVATE)
TNUMBER = OMP_GET_THREAD_NUM()
PRINT *, 'Hello from Thread Number[',TNUMBER,'] and Welcome Webinar!'
!$OMP END PARALLEL

STOP
END
```
[Back to CPU Jobs](#comp-and-run-cpu-jobs) <br>
[Back to Top](#top)
<hr>

#### Hello World (OpenMP): Compiling:  <a name="hello-world-omp-compile"></a>

Note that there is already a compiled version of the `hello_openmp.f90` code. You can save or delete this version.

* In this example, we compile the source code using the `ifort` command, and verify that it was created:
```
[mthomas@expanse-ln3 OPENMP]$ ifort -o hello_openmp -qopenmp hello_openmp.f90
[mthomas@expanse-ln3 OPENMP]$ ls -al
[mthomas@expanse-ln3:~/expanse101/OPENMP] ll
total 77
drwxr-xr-x 2 mthomas use300      7 Apr 16 00:35 .
drwxr-xr-x 6 mthomas use300      6 Apr 15 20:10 ..
-rwxr-xr-x 1 mthomas use300 816952 Apr 16 00:35 hello_openmp
-rw-r--r-- 1 mthomas use300    267 Apr 15 15:47 hello_openmp_2.f90
-rw-r--r-- 1 mthomas use300    267 Apr 15 15:47 hello_openmp.f90
-rw-r--r-- 1 mthomas use300    311 Apr 15 15:47 openmp-slurm.sb
-rw-r--r-- 1 mthomas use300    347 Apr 15 15:47 openmp-slurm-shared.sb

```
* Note that if you try to run OpenMP code from the command line, in the current environment, the code will run (because it is based on Pthreads, which exist on the node):
```
[mthomas@expanse-ln2 OPENMP]$ ./hello_openmp
Hello from Thread Number[           8 ] and Welcome HPC Trainees!
Hello from Thread Number[           3 ] and Welcome HPC Trainees!
Hello from Thread Number[          16 ] and Welcome HPC Trainees!
Hello from Thread Number[          12 ] and Welcome HPC Trainees!
Hello from Thread Number[           9 ] and Welcome HPC Trainees!
Hello from Thread Number[           5 ] and Welcome HPC Trainees!
Hello from Thread Number[           4 ] and Welcome HPC Trainees!
Hello from Thread Number[          14 ] and Welcome HPC Trainees!
Hello from Thread Number[           7 ] and Welcome HPC Trainees!
Hello from Thread Number[          11 ] and Welcome HPC Trainees!
Hello from Thread Number[          13 ] and Welcome HPC Trainees!
Hello from Thread Number[           6 ] and Welcome HPC Trainees!
Hello from Thread Number[          10 ] and Welcome HPC Trainees!
Hello from Thread Number[          19 ] and Welcome HPC Trainees!
Hello from Thread Number[          15 ] and Welcome HPC Trainees!
Hello from Thread Number[           2 ] and Welcome HPC Trainees!
Hello from Thread Number[          18 ] and Welcome HPC Trainees!
Hello from Thread Number[          17 ] and Welcome HPC Trainees!
Hello from Thread Number[          23 ] and Welcome HPC Trainees!
Hello from Thread Number[          20 ] and Welcome HPC Trainees!
Hello from Thread Number[          22 ] and Welcome HPC Trainees!
Hello from Thread Number[           1 ] and Welcome HPC Trainees!
Hello from Thread Number[           0 ] and Welcome HPC Trainees!
Hello from Thread Number[          21 ] and Welcome HPC Trainees!
```
* In the example below, we used the OpenMP feature to set the number of threads from the command line.

```
[mthomas@expanse-ln3 OPENMP]$ export OMP_NUM_THREADS=4; ./hello_openmp
Hello from Thread Number[           0 ] and Welcome HPC Trainees!
Hello from Thread Number[           1 ] and Welcome HPC Trainees!
Hello from Thread Number[           2 ] and Welcome HPC Trainees!
Hello from Thread Number[           3 ] and Welcome HPC Trainees!
```

[Back to CPU Jobs](#comp-and-run-cpu-jobs) <br>
[Back to Top](#top)
<hr>

#### <a name="hello-world-omp-batch-submit"></a>Hello World (OpenMP): Batch Script Submission
The submit script is openmp-slurm.sb:

```
[mthomas@expanse-ln2 OPENMP]$ cat openmp-slurm.sb
#!/bin/bash
#SBATCH --job-name="hello_openmp"
#SBATCH --output="hello_openmp.%j.%N.out"
#SBATCH --partition=compute
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=24
#SBATCH --export=ALL
#SBATCH -t 01:30:00

# Define the user environment
source /etc/profile.d/modules.sh
module purge
module load intel
module load mvapich2_ib

#SET the number of openmp threads
export OMP_NUM_THREADS=24

#Run the job using mpirun_rsh
./hello_openmp
```
* to submit use the sbatch command:
```
[mthomas@expanse-ln2 OPENMP]$ sbatch openmp-slurm.sb
Submitted batch job 32661678
[mthomas@expanse-ln2 OPENMP]$ squeue -u username
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
32661678   compute hello_op  mthomas PD       0:00      1 (Priority)
...
```

[Back to CPU Jobs](#comp-and-run-cpu-jobs) <br>
[Back to Top](#top)
<hr>

#### Hello World (OpenMP): Batch Script Output:  <a name="hello-world-omp-batch-output"></a>

* Once the job is finished:
```
[mthomas@expanse-ln2 OPENMP] cat hello_openmp.32661678.expanse-07-47.out
Hello from Thread Number[           5 ] and Welcome HPC Trainees!
Hello from Thread Number[           7 ] and Welcome HPC Trainees!
Hello from Thread Number[          16 ] and Welcome HPC Trainees!
Hello from Thread Number[           9 ] and Welcome HPC Trainees!
Hello from Thread Number[          18 ] and Welcome HPC Trainees!
Hello from Thread Number[          12 ] and Welcome HPC Trainees!
Hello from Thread Number[          10 ] and Welcome HPC Trainees!
Hello from Thread Number[           0 ] and Welcome HPC Trainees!
Hello from Thread Number[          14 ] and Welcome HPC Trainees!
Hello from Thread Number[           4 ] and Welcome HPC Trainees!
Hello from Thread Number[           3 ] and Welcome HPC Trainees!
Hello from Thread Number[          11 ] and Welcome HPC Trainees!
Hello from Thread Number[          19 ] and Welcome HPC Trainees!
Hello from Thread Number[          22 ] and Welcome HPC Trainees!
Hello from Thread Number[          15 ] and Welcome HPC Trainees!
Hello from Thread Number[           2 ] and Welcome HPC Trainees!
Hello from Thread Number[           6 ] and Welcome HPC Trainees!
Hello from Thread Number[           1 ] and Welcome HPC Trainees!
Hello from Thread Number[          21 ] and Welcome HPC Trainees!
Hello from Thread Number[          20 ] and Welcome HPC Trainees!
Hello from Thread Number[          17 ] and Welcome HPC Trainees!
Hello from Thread Number[          23 ] and Welcome HPC Trainees!
Hello from Thread Number[          13 ] and Welcome HPC Trainees!
Hello from Thread Number[           8 ] and Welcome HPC Trainees!
```

[Back to CPU Jobs](#comp-and-run-cpu-jobs) <br>
[Back to Top](#top)
<hr>

### Hybrid (MPI + OpenMP) Jobs: <a name="hybrid-mpi-omp"></a>
<b>Subsections:</b>
* [Hybrid (MPI + OpenMP): Source Code](#hybrid-mpi-omp-source)
* [Hybrid (MPI + OpenMP): Compiling](#hybrid-mpi-omp-compile)
* [Hybrid (MPI + OpenMP): Batch Script Submission](#hybrid-mpi-omp-batch-submit)
* [Hybrid (MPI + OpenMP): Batch Script Output](#hybrid-mpi-omp-batch-output)


### Hybrid (MPI + OpenMP) Source Code: <a name="hybrid-mpi-omp-source"></a>
#Several HPC codes use a hybrid MPI, OpenMP approach.
* `ibrun` wrapper developed to handle such hybrid use cases. Automatically senses the MPI build (mvapich2, openmpi) and binds tasks correctly.
* `ibrun -help` gives detailed usage info.
* hello_hybrid.c is a sample code, and hello_hybrid.cmd shows “ibrun” usage.
* Change to the HYBRID examples directory:

```
[mthomas@expanse-ln2 expanse101]$ cd HYBRID/
[mthomas@expanse-ln2 HYBRID]$ ll
total 94
drwxr-xr-x  2 username use300      5 Aug  5 19:02 .
drwxr-xr-x 16 username use300     16 Aug  5 19:02 ..
-rwxr-xr-x  1 username use300 103032 Aug  5 19:02 hello_hybrid
-rw-r--r--  1 username use300    636 Aug  5 19:02 hello_hybrid.c
-rw-r--r--  1 username use300    390 Aug  5 19:02 hybrid-slurm.sb
```
* Look at the contents of the `hello_hybrid.c` file
```
[mthomas@expanse-ln2 HYBRID]$ cat hello_hybrid.c
#include <stdio.h>
#include "mpi.h"
#include <omp.h>

int main(int argc, char *argv[]) {
int numprocs, rank, namelen;
char processor_name[MPI_MAX_PROCESSOR_NAME];
int iam = 0, np = 1;

MPI_Init(&argc, &argv);
MPI_Comm_size(MPI_COMM_WORLD, &numprocs);
MPI_Comm_rank(MPI_COMM_WORLD, &rank);
MPI_Get_processor_name(processor_name, &namelen);

#pragma omp parallel default(shared) private(iam, np)
{
np = omp_get_num_threads();
iam = omp_get_thread_num();
printf("Hello Webinar participants from thread %d out of %d from process %d out of %d on %s\n",
iam, np, rank, numprocs, processor_name);
}

MPI_Finalize();
}
```

[Back to CPU Jobs](#comp-and-run-cpu-jobs) <br>
[Back to Top](#top)
<hr>

#### Hybrid (MPI + OpenMP): Compiling:  <a name="hybrid-mpi-omp-compile"></a>
* To compile the hybrid MPI + OpenMPI code, we need to refer to the table of compilers listed above (and listed in the user guide).
* We will use the command `mpicx -openmp`
```
[mthomas@expanse-ln2 HYBRID]$ mpicc -openmp -o hello_hybrid hello_hybrid.c
[mthomas@expanse-ln2 HYBRID]$ ll
total 39
drwxr-xr-x  2 username use300      5 Aug  6 00:12 .
drwxr-xr-x 16 username use300     16 Aug  5 19:02 ..
-rwxr-xr-x  1 username use300 103032 Aug  6 00:12 hello_hybrid
-rw-r--r--  1 username use300    636 Aug  5 19:02 hello_hybrid.c
-rw-r--r--  1 username use300    390 Aug  5 19:02 hybrid-slurm.sb

```

[Back to CPU Jobs](#comp-and-run-cpu-jobs) <br>
[Back to Top](#top)
<hr>


#### Hybrid (MPI + OpenMP): Batch Script Submission:  <a name="hybrid-mpi-omp-batch-submit"></a>
* To submit the hybrid code, we still use the `ibrun` command.
* In this example, we set the number of threads explicitly.
```
[mthomas@expanse-ln2 HYBRID]$ cat hybrid-slurm.sb
#!/bin/bash
#SBATCH --job-name="hellohybrid"
#SBATCH --output="hellohybrid.%j.%N.out"
#SBATCH --partition=compute
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=24
#SBATCH --export=ALL
#SBATCH -t 01:30:00


# Define the user environment
source /etc/profile.d/modules.sh
module purge
module load intel
module load mvapich2_ib

#This job runs with 2 nodes, 24 cores per node for a total of 48 cores.
# We use 8 MPI tasks and 6 OpenMP threads per MPI task

export OMP_NUM_THREADS=6
ibrun --npernode 4 ./hello_hybrid
```
* Submit the job to the Slurm queue, and check the job status
```
[mthomas@expanse-ln2 HYBRID]$ sbatch hybrid-slurm.sb
Submitted batch job 18347079
[mthomas@expanse-ln2 HYBRID]$ squeue -u username
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
18347079   compute hellohyb  username  R       0:04      2 expanse-01-[01,04]
[mthomas@expanse-ln2 HYBRID]$ ll
```

[Back to CPU Jobs](#comp-and-run-cpu-jobs) <br>
[Back to Top](#top)
<hr>


#### Hybrid (MPI + OpenMP): Batch Script Output: <a name="hybrid-mpi-omp-batch-output"></a>

```
[mthomas@expanse-ln2 HYBRID]$ ll
total 122
drwxr-xr-x  2 username use300      6 Aug  6 00:12 .
drwxr-xr-x 16 username use300     16 Aug  5 19:02 ..
-rwxr-xr-x  1 username use300 103032 Aug  6 00:12 hello_hybrid
-rw-r--r--  1 username use300   3696 Aug  6 00:12 hellohybrid.18347079.expanse-01-01.out
-rw-r--r--  1 username use300    636 Aug  5 19:02 hello_hybrid.c
-rw-r--r--  1 username use300    390 Aug  5 19:02 hybrid-slurm.sb
[mthomas@expanse-ln2 HYBRID]$ cat hellohybrid.18347079.expanse-01-01.out
Hello from thread 4 out of 6 from process 3 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 3 out of 6 from process 2 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 0 out of 6 from process 1 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 2 out of 6 from process 2 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 1 out of 6 from process 3 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 2 out of 6 from process 1 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 4 out of 6 from process 2 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 0 out of 6 from process 3 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 3 out of 6 from process 1 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 5 out of 6 from process 2 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 3 out of 6 from process 3 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 4 out of 6 from process 1 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 0 out of 6 from process 2 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 2 out of 6 from process 3 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 5 out of 6 from process 1 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 5 out of 6 from process 3 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 3 out of 6 from process 0 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 2 out of 6 from process 0 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 0 out of 6 from process 0 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 4 out of 6 from process 0 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 5 out of 6 from process 0 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 1 out of 6 from process 2 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 1 out of 6 from process 1 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 1 out of 6 from process 0 out of 8 on expanse-01-01.sdsc.edu
Hello from thread 0 out of 6 from process 7 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 0 out of 6 from process 6 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 2 out of 6 from process 7 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 2 out of 6 from process 6 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 3 out of 6 from process 7 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 5 out of 6 from process 6 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 4 out of 6 from process 6 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 1 out of 6 from process 7 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 4 out of 6 from process 7 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 1 out of 6 from process 6 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 0 out of 6 from process 4 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 5 out of 6 from process 4 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 2 out of 6 from process 4 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 1 out of 6 from process 4 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 3 out of 6 from process 4 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 4 out of 6 from process 4 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 0 out of 6 from process 5 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 1 out of 6 from process 5 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 4 out of 6 from process 5 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 2 out of 6 from process 5 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 5 out of 6 from process 5 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 3 out of 6 from process 6 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 5 out of 6 from process 7 out of 8 on expanse-01-04.sdsc.edu
Hello from thread 3 out of 6 from process 5 out of 8 on expanse-01-04.sdsc.edu
[mthomas@expanse-ln2 HYBRID]$
```

[Back to CPU Jobs](#comp-and-run-cpu-jobs) <br>
[Back to Top](#top)
<hr>
