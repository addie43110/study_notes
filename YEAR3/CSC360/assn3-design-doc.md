# File systems

## Part I
`./diskinfo disk.IMA`

- OS Name (boot sector, bytes 3-10)
- Label of the disk (may be in boot sector 43-54, may be in root directory)
- total size of the disk (total sector count: byte 19, multiplied by 512; or find how many bytes per sector in bytes 11-12)
- free size of the disk (rely on FAT entries; if value of fat entry == 0x00, then empty sector) 
- number of files on the disk (including all files in the root directory and files in all subdirectories) (go through root directory sectors 19 - 32, skip if director entry:
  - value of attribute field (byte 11) is 0x0F
  - 4th bit of attribute field (subdirectory bit) is 1
  - volume label bit of attribute field (bit 3) is 1
  - directory entry is free, 6th or 7th bit is 1
- number of FAT copies (boot sector, byte 16)
- sectors per FAT (boot sector, byte 22-23)

## Part II
`./disklist disk.IMA`  --> list content of root directory and all sub-directories in file system

** can all be found in the root directory section **

- directory name (bytes 0 - 7)
- F for regular files (4th bit of attribute field: byte 11 is not 1)
- D for directories (4th bit of attribute field: byte 11 is 1)
- file size in bytes (10 characters) (bytes 28-31)
- file name (20 characters) (first 8 bytes; if first byte is 0xE5, then directory entry is unused; if first byte of filename is 0x00, then directory entry is free al all remaining directory entries in this directory are also free)
- file creation date and time (date: bytes 16-17, time: bytes 14-15)

## Part III
`./diskget disk.IMA ANS1.PDF`

- Copies file from file system to current directory in Linux.
- convert the given filename to upper case, then search this filename from directory entries in root folder
- if filename matches, get first cluster number and file size
- go through FAT entries to copy
- if(last cluster of file && reach filesize value), stop copy.

## Part IV
`./diskput disk.IMA \subdir1\subdir2\foo.txt`

- copies file from current directory (ie. foo.txt) to file system directory specified
- create new directory entry in root folder
- check if the disk has enough space to store the file
- go through FAT entries to find unused sectors in disk and copy file content
- update the first cluster number field of directory entry we just created, update the FAT entries we used
