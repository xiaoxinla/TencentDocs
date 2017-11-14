# About aii-fs
aii-fs is a standard tool to transfer files between user’s local file system and the HDFS of AII platform.
# Installation
* Download the Compressed file from "Downloads" page depending on the OS you use. We provide packages for both Windows and Linux.
* Extract files.
* Modify"aii-fs.conf" following comments in it
# Usage
Use Windows command line (cmd) or bash to enter the directory of aii-fs. Then run `aii-fs.exe -h` or `./aii-fs -h` to get detailed usage.
```
example use:
  aii-fs -ls hdfs://                                         -- list the contents of a root HDFS directory 
  aii-fs -ls -r hdfs://                                      -- list the contents of a root HDFS directory, recursively 
  aii-fs -mkdir hdfs://mydir/mysubdir/mysubdir2              -- makes mysubdir2 and all directories along the way 
  aii-fs -rm hdfs://mydir/mysubdir/myfile                    -- removes myfile from mysubdir 
  aii-fs -rm hdfs://mydir/mysubdir                           -- removes mysubdir and all files and directories in it 
  aii-fs -cp c:\mylocalfile hdfs://mydir/myremotedir         -- copy mylocalfile into myremotedir 
  aii-fs -cp -r c:\mylocaldir hdfs://mydir/myremotedir       -- copy mylocaldir into myremotedir, recursively 
  aii-fs -cp -r c:\mylocaldir\* hdfs://mydir/myremotedir     -- copy mylocaldir's contents into myremotedir, recursively 
  aii-fs -cp c:\mylocaldir\\a hdfs://mydir/myremotedir/b     -- copy file a from mylocaldir to myremotedir and rename to b 
  aii-fs -cp -r hdfs://mydir/myremotedir c:\mylocaldir       -- copy myremotedir into mylocaldir, recursively 
  aii-fs -cp -r hdfs://mydir/myremotedir/* c:\mylocaldir     -- copy myremotedir's contents into mylocaldir, recursively 
exit code:
  0   -- Success 
  1   -- An exception happened during the operation including bad connection 
  2   -- AII_VC environment variable not set to valid VC or insufficient/invalid command line argument(s) 
  3   -- Path not found 
  4   -- Unauthorized access 
  5   -- Path not empty 
  6   -- Check failed after operation 
  100 -- Failed to copy too many times 
  101 -- Failed to concat chunks into file 
```
