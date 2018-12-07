# File Systems

## Hard links vs Soft Links
Every file contains 2 parts:
  1. File name part
  2. iNode part

Two ways to create a link
  1. Hardlink
    - created file name part
    - increments original iNode reference count
  2. Softlink
    - creates a new inode which points to the original iNode

## Structure of iNode
  - metadata
    - mode (permissions, filetype)
    - link count (reference count)
    - **does NOT include** file name
      - multiple filename can be attached to one iNode
  - 12 direct block pointers
  - 1 single indirect block pointer
  - 1 double indirect block pointer
  - 1 triple indirect block pointer

## FAT 12 system
  - refer to tutorial notes
