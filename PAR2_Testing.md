---
date: 2018-07-08
author: KarolNi
tags: backup par2 bitrot
---

# PAR2 testing
Recently I was experimenting with PAR2. I'm planning to use it for bitrot protection of my backups.

Test set of files:
- text file 3.4kB
- png image 785.6kB
- jpg image 738.5kB

total 1527580 bytes

Set of par2 files created by PyPar2 (default settings).
Test set has 1990 data blocks of 768 bytes (par2 metrics). Parity files has 5% (100 recovery blocks) of redundancy and total size of 1219776 bytes (1191.2kB). Size of all 7 parity files was 80% of original files.

For bitrot protection par2 block should align with medium block. Modern hard drives claims that it won't return wrong data and entire rotten sector will return zeros (disks has own ECC). (Hypothesis - not tested yet)

For single big file (1944256512 bytes, 1.9GB) default settings (5% redundancy) par2 generated (98349904 bytes, 98.3MB) of parity files, so for single big file storage efficiency if much better. In this scenario data was divided in 2000 blocks (98349904 bytes in one block).

Par2 seems to be single threaded.

## Experiments and results

- Single random byte corruption - need 1 recovery block
- n random bytes corruption - need n or less recovery blocks
- 512 bytes continuous corruption in random place - need 2 recovery blocks
- Random insertion or deletion of data - all data after change (in this file) needs to be reconstructed from recovery blocks, if you have sufficient redundancy
- Appendage to file - need 1 recovery block
- Deletion of file - whole file is reconstructed from recovery blocks, if you have sufficient redundancy
- Rename of file - whole file is interpreted as missing and needs to be reconstructed from recovery blocks, if you have sufficient redundancy
- Deletion of file and corruption in other file - you need recovery blocks for all data blocks of deleted file and recovery blocks for corrupted data
- Singe random byte corruption in parity file - error was detected (number of available recovery blocks decreased by 1, even if parity file contains multiple recovery blocks), all operations were OK (as log as you have sufficient number correct recovery blocks). This was tested in one try - it is still possible that single byte corruption could invalid whole parity file (but you can have multiple different parity files). I believe, that it is extremely unlikely that corruption of parity files could harm protected files during recovery attempt.

