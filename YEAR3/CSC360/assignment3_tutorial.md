# Assignment 3 Notes

# Your task, should you choose to accept it
You need to output the OS name, and the label of the disk (you must find out where it is!). You must also print the size of the disk, which also can be found somewhere in the disk space.

  - info
  - list
  - get: copy the file outside?
  - put: write a file inside

# FAT12 can be split into 4 parts...
  - boot part
  - FAT tables (FAT1 and FAT2)
  - root (directory entries; each directory entry is 32 bytes)
  - data

## Measurement
  - cluster = 1 sector
  - sector = 512 bytes

## Where to find stuff
 - can find OS operating system in the Boot Sector 0 - 11 bytes
 - 11-12: how many bytes per sector: 0x0002 becomes 0200 (stored in little endian) = 512 bytes
 - how many FAT tables, byte 16
 - number of sectors, byte 19-20
 - 22-23: sector/FAT
 - 43-54: volume label
    - it may be that there are only blanks in these bytes; in this case, we need to go to the root directory to find the volume label

## Directory entry
 - 0-7 file name
 - 8-10 extension
 - 11 attribute
 - 14-15 time
 - 16-17 date
 - 26-27 first logical cluster #
 - 28-32 file size

### How to decode time?
Say CC 4C is your time. This stands for 0x4CCC = 0100 1100 1100 1100. 01001 = 9, 100110 = 38. so this is 9:38 AM?

### How to decode date?
Say your date is 64 4D. This is 0x4D64 = 0100 1101 0110 0100. First seven bits is the year, 0100110 = 38. Then add to 1980 = 2018. Then 1011 = 11 and 00100 = 4 so the date is 2018-11-4.

### Volume label
If attribute is 0x01, we know it is the volume label.

## Example
Let's say your file directory looks like this:

```
\
|---- a.txt  (100 (size))
|---- b.txt (1200)
|---- c
      |----d.h (10)
      |----d.c (50)
```

## Sector 
Sector # is equal to cluster number.


## Other
if entry is 0x00, all entries after are free (including that entry). if entry is 0xE5, then that entry is free, but entries after may not be fre.

# Code
Read the whole map file into memory `mmap`. Read only part of the file `fread()` and `fwrite()`. This is slower, `mmap` is faster but it takes more memory, so good for only small files. Can use `fseek()` to find a certain position to start reading from.
