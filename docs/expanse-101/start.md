---
layout: default
title: Getting Started on Expanse
parent: Expanse 101
nav_order: 2
---

## <a name="sys-env"></a>Getting Started on Expanse

### <a name="expanse-accounts"></a>Expanse Accounts
You must have a expanse account in order to access the system.
* Obtain a trial account here:  http://www.sdsc.edu/support/user_guides/expanse.html#trial_accounts
* You can use your XSEDE account.

### <a name="expanse-logon"></a>Logging Onto Expanse
Details about how to access Expanse under different circumstances are described in the Expanse User Guide:
http://www.sdsc.edu/support/user_guides/expanse.html#access

For instructions on how to use SSH, see [here](https://github.com/sdsc/sdsc-summer-institute-2020/tree/master/0_preparation/connecting-to-hpc-systems)
```
[mthomas@gidget:~] ssh -Y expanse.sdsc.edu
Password:
Last login: Fri Jul 31 14:20:40 2020 from 76.176.117.51
Rocks 7.0 (Manzanita)
Profile built 12:32 03-Dec-2019

Kickstarted 13:47 03-Dec-2019

WELCOME TO
__________________  __  _______________
-----/ ____/ __ \/  |/  / ____/_  __/
--/ /   / / / / /|_/ / __/   / /
/ /___/ /_/ / /  / / /___  / /
\____/\____/_/  /_/_____/ /_/
###############################################################################
NOTICE:
The Expanse login nodes are not to be used for running processing tasks.
This includes running Jupyter notebooks and the like.  All processing
jobs should be submitted as jobs to the batch scheduler.  If you don’t
know how to do that see the Expanse user guide
https://www.sdsc.edu/support/user_guides/expanse.html#running.
Any tasks found running on the login nodes in violation of this policy
may be terminated immediately and the responsible user locked out of
the system until they contact user services.
###############################################################################
(base) [mthomas@expanse-ln2:~]

```

[Back to Top](#top)
<hr>

### <a name="example-code"></a>Obtaining Example Code
* Create a test directory hold the expanse example files:
```
[expanse-ln2 ~]$ mkdir expanse-examples
[expanse-ln2 ~]$ ls -al
total 166
drwxr-x---   8 user user300    24 Jul 17 20:20 .
drwxr-xr-x 139 root    root       0 Jul 17 20:17 ..
-rw-r--r--   1 user use300  2487 Jun 23  2017 .alias
-rw-------   1 user use300 14247 Jul 17 12:11 .bash_history
-rw-r--r--   1 user use300    18 Jun 19  2017 .bash_logout
-rw-r--r--   1 user use300   176 Jun 19  2017 .bash_profile
-rw-r--r--   1 user use300   159 Jul 17 18:24 .bashrc
drwxr-xr-x   2 user use300     2 Jul 17 20:20 expanse-examples
[snip extra lines]
[expanse-ln2 ~]$ cd expanse-examples/
[expanse-ln2 expanse-examples]$ pwd
/home/user/expanse-examples
[expanse-ln2 expanse-examples]$
```
* Copy the `/share/apps/examples/expanse101/` directory to your local (`/home/username/expanse-examples`) directory. Note: you can learn to create and modify directories as part of the *Getting Started* and *Basic Skills* preparation work:
https://github.com/sdsc/sdsc-summer-institute-2020/tree/master/0_preparation
```
[mthomas@expanse-ln3 ~]$ ls -al /share/apps/examples/hpc-training/expanse-examples/
total 20
(base) [mthomas@expanse-ln2:~/expanse101] ll /share/apps/examples/hpc-training/expanse101/
total 32
drwxr-sr-x 8 mthomas  use300 4096 Apr 16 10:39 .
drwxrwsr-x 4 mahidhar use300 4096 Apr 15 23:37 ..
drwxr-sr-x 5 mthomas  use300 4096 Apr 16 03:30 CUDA
drwxr-sr-x 2 mthomas  use300 4096 Apr 16 10:39 HYBRID
drwxr-sr-x 2 mthomas  use300 4096 Apr 16 10:39 jupyter_notebooks
drwxr-sr-x 2 mthomas  use300 4096 Apr 16 16:46 MKL
drwxr-sr-x 4 mthomas  use300 4096 Apr 16 03:30 MPI
drwxr-sr-x 2 mthomas  use300 4096 Apr 16 03:31 OPENMP
```
Copy the 'expanse101' directory into your `expanse-examples` directory:
```
[mthomas@expanse-ln3 ~]$
[mthomas@expanse-ln3 ~]$ cp -r /share/apps/examples/expanse101/ expanse-examples/
[mthomas@expanse-ln3 ~]$ ls -al expanse-examples/
total 105
drwxr-xr-x  5 username use300   6 Aug  5 19:02 .
drwxr-x--- 10 username use300  27 Aug  5 17:59 ..
drwxr-xr-x 16 username use300  16 Aug  5 19:02 expanse101
[mthomas@expanse-ln3 expanse-examples]$ ls -al
total 132
total 170
drwxr-xr-x  8 mthomas use300  8 Aug  3 01:19 .
drwxr-x--- 64 mthomas use300 98 Aug  3 01:19 ..
drwxr-xr-x  5 mthomas use300  5 Aug  3 01:19 CUDA
drwxr-xr-x  2 mthomas use300  6 Aug  3 01:19 HYBRID
drwxr-xr-x  2 mthomas use300  3 Aug  3 01:19 jupyter_notebooks
drwxr-xr-x  2 mthomas use300  6 Aug  3 01:19 MKL
drwxr-xr-x  4 mthomas use300  9 Aug  3 01:19 MPI
drwxr-xr-x  2 mthomas use300  9 Aug  3 01:19 OPENMP
```
Most examples will contain source code, along with a batch script example so you can run the example, and compilation examples (e.g. see the MKL example).

[Back to Top](#top)
<hr>

