---
layout: default
title: Getting Started on Comet
parent: Comet 101
nav_order: 2
---
## <a name="sys-env"></a>Getting Started on Comet

### <a name="comet-accounts"></a>Comet Accounts
You must have a comet account in order to access the system.
* Obtain a trial account here:  http://www.sdsc.edu/support/user_guides/comet.html#trial_accounts
* You can use your XSEDE account.

### <a name="comet-logon"></a>Logging Onto Comet
Details about how to access Comet under different circumstances are described in the Comet User Guide:
http://www.sdsc.edu/support/user_guides/comet.html#access

For instructions on how to use SSH, see [here](https://github.com/sdsc/sdsc-summer-institute-2020/tree/master/0_preparation/connecting-to-hpc-systems)
```
[mthomas@gidget:~] ssh -Y comet.sdsc.edu
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
The Comet login nodes are not to be used for running processing tasks.
This includes running Jupyter notebooks and the like.  All processing
jobs should be submitted as jobs to the batch scheduler.  If you don’t
know how to do that see the Comet user guide
https://www.sdsc.edu/support/user_guides/comet.html#running.
Any tasks found running on the login nodes in violation of this policy
may be terminated immediately and the responsible user locked out of
the system until they contact user services.
###############################################################################
(base) [mthomas@comet-ln2:~]

```

[Back to Top](#top)
<hr>

### <a name="example-code"></a>Obtaining Example Code
* Create a test directory hold the comet example files:
```
[comet-ln2 ~]$ mkdir comet-examples
[comet-ln2 ~]$ ls -al
total 166
drwxr-x---   8 user user300    24 Jul 17 20:20 .
drwxr-xr-x 139 root    root       0 Jul 17 20:17 ..
-rw-r--r--   1 user use300  2487 Jun 23  2017 .alias
-rw-------   1 user use300 14247 Jul 17 12:11 .bash_history
-rw-r--r--   1 user use300    18 Jun 19  2017 .bash_logout
-rw-r--r--   1 user use300   176 Jun 19  2017 .bash_profile
-rw-r--r--   1 user use300   159 Jul 17 18:24 .bashrc
drwxr-xr-x   2 user use300     2 Jul 17 20:20 comet-examples
[snip extra lines]
[comet-ln2 ~]$ cd comet-examples/
[comet-ln2 comet-examples]$ pwd
/home/user/comet-examples
[comet-ln2 comet-examples]$
```
* Copy the `/share/apps/examples/comet101/` directory to your local (`/home/username/comet-examples`) directory. Note: you can learn to create and modify directories as part of the *Getting Started* and *Basic Skills* preparation work:
https://github.com/sdsc/sdsc-summer-institute-2020/tree/master/0_preparation
```
[mthomas@comet-ln3 ~]$ ls -al /share/apps/examples/hpc-training/comet-examples/
total 20
(base) [mthomas@comet-ln2:~/comet101] ll /share/apps/examples/hpc-training/comet101/
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
Copy the 'comet101' directory into your `comet-examples` directory:
```
[mthomas@comet-ln3 ~]$
[mthomas@comet-ln3 ~]$ cp -r /share/apps/examples/comet101/ comet-examples/
[mthomas@comet-ln3 ~]$ ls -al comet-examples/
total 105
drwxr-xr-x  5 username use300   6 Aug  5 19:02 .
drwxr-x--- 10 username use300  27 Aug  5 17:59 ..
drwxr-xr-x 16 username use300  16 Aug  5 19:02 comet101
[mthomas@comet-ln3 comet-examples]$ ls -al
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

