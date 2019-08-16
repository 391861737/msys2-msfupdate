# Introduction
As we know, the `metasploit-framework.msi` installs the framework along with the entire msys2 environment which is 
unnecessarily needed when we have a msys2 platform installed already.  
And after some investigation and hardworking on building it from source on windows, I found it is nearly impossible
to do that. Because the `pg-0.21.0-x64-mingw32` or `pg-0.21.0-x86-mingw32` requires ruby *< 2.5, >= 2.0*, while at 
the same time the metasploit-framework explicitly requires `pg-0.21.0`...  
So the only option left here is to install it using the mis package file built by omnibus. But I do not like the 
redundant msys2 bundled with it, hence this script here to solve the problem for me and also provide an easy way to update it.  

To achive a robust performance, it would do the following things:

### Prepare the msi package file
* Check if a specific msi package file is given
* If not given, then try to use the `wget` command to get the latest msi package if any
* If the msi file is not fully downloaded, resume the downloading process
* If there's no wget found, then try to install it using pacman on `msys2` or exit on `cygwin`

### Etract, unzip
* Now that the msi package is prepared, try to extract it (using `msiexec` which should be on modern WINDOWSes)
* If the given msi file could not be found or invalid, exit
* After extracting into a folder in `/tmp`, unzip the `metasploit-framework.zip` file in it
* The process will be printed every 1000 files being unzipped

### Install
* Use the given framework directory or `/opt/framework` to store the main project files
* Use the given ruby directory or current active ruby's home as the home directory of ruby
* Backup, copy (try hardlink first then normal copy if failed) all necessary files respectively

### Cleanup
* If not flag of keeping is given, cleanup the msi file as well as the extracted folder

### Expose
* Output the shell scripts to `/usr/bin`, the batch script to the given bridge directory if specified and valid
