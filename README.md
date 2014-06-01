audfprint
=========

Audio landmark-based fingerprinting.  

```
Create a new fingerprint dbase with new, 
append new files to an existing database with add, 
or identify noisy query excerpts with match.

Usage: audfprint (new | add | match) (-d <dbase> | --dbase <dbase>) [options] <file>...

Options:
  -n <dens>, --density <dens>     Target hashes per second [default: 7.0]
  -h <bits>, --hashbits <bits>    How many bits in each hash [default: 20]
  -b <val>, --bucketsize <val>    Number of entries per bucket [default: 100]
  -t <val>, --maxtime <val>       Largest time value stored [default: 16384]
  -l, --list                      Input files are lists, not audio
  --version                       Report version number
  --help                          Print this message
```

Uses librosa, get https://github.com/bmcfee/librosa

Uses docopt, see http://docopt.org , get https://github.com/docopt/docopt

Based on Matlab prototype, http://www.ee.columbia.edu/~dpwe/resources/matlab/audfprint/

Usage
=====

Build a database of fingerprints from a set of reference audio files:
```
> python audfprint.py new --dbase fpdbase.pklz Nine_Lives/0*.mp3
Sun Jun  1 09:18:50 2014 ingesting # 0 : Nine_Lives/01-Nine_Lives.mp3  ...
Sun Jun  1 09:18:53 2014 ingesting # 1 : Nine_Lives/02-Falling_In_Love.mp3  ...
Sun Jun  1 09:18:55 2014 ingesting # 2 : Nine_Lives/03-Hole_In_My_Soul.mp3  ...
Sun Jun  1 09:18:58 2014 ingesting # 3 : Nine_Lives/04-Taste_Of_India.mp3  ...
Sun Jun  1 09:19:01 2014 ingesting # 4 : Nine_Lives/05-Full_Circle.mp3  ...
Sun Jun  1 09:19:04 2014 ingesting # 5 : Nine_Lives/06-Something_s_Gotta_Give.mp3  ...
Sun Jun  1 09:19:06 2014 ingesting # 6 : Nine_Lives/07-Ain_t_That_A_Bitch.mp3  ...
Sun Jun  1 09:19:09 2014 ingesting # 7 : Nine_Lives/08-The_Farm.mp3  ...
Sun Jun  1 09:19:11 2014 ingesting # 8 : Nine_Lives/09-Crash.mp3  ...
Added 65044 hashes (25.5 hashes/sec) at 0.009 x RT
Saved fprints for 9 files ( 65044 hashes) to fpdbase.pklz
```
Add more reference tracks to an existing database:
```
> python audfprint.py add --dbase fpdbase.pklz Nine_Lives/1*.mp3
Read fprints for 9 files ( 65044 hashes) from fpdbase.pklz
Sun Jun  1 09:19:30 2014 ingesting # 0 : Nine_Lives/10-Kiss_Your_Past_Good-bye.mp3  ...
Sun Jun  1 09:19:32 2014 ingesting # 1 : Nine_Lives/11-Pink.mp3  ...
Sun Jun  1 09:19:35 2014 ingesting # 2 : Nine_Lives/12-Attitude_Adjustment.mp3  ...
Sun Jun  1 09:19:37 2014 ingesting # 3 : Nine_Lives/13-Fallen_Angels.mp3  ...
Added 28104 hashes (22.9 hashes/sec) at 0.009 x RT
Saved fprints for 13 files ( 93148 hashes) to fpdbase.pklz
```
Match a fragment recorded of music playing in the background against the database:
```
> python audfprint.py match --dbase fpdbase.pklz query.mp3
Read fprints for 13 files ( 93148 hashes) from fpdbase.pklz
Matched query.mp3 as Nine_Lives/05-Full_Circle.mp3 at 50.085 s with 14 of 17 hashes
```
The query contained audio from `Nine_Lives/05-Full_Circle.mp3` starting at 50.085 sec into the track.  There were a total of 17 landmark hashes shared between the query and that track, and 14 of them had a consistent time offset.  Generally, anything more than 5 or 6 consistently-timed matching hashes indicate a true match, and random chance will result in fewer than 1% of the raw common hashes being temporally consistent.

