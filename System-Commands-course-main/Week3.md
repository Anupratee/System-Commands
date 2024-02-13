## Week 3 Notes

### Combining Commands and Files
* Executing Multiple Commands
  - `command1; command2; command3;`
    - Each command will be executed one after the other.
  - `command1 && command2`
    - command2 will be executed only if command 1 succeeds
    - If the return code is 0 it is true and if it is greater than 0 it is false
    - `ls && date -Q && wc -l /etc/profile` will display the dir listing followed by error that -Q is invalid; wc is not executed.
  - `command1 || command2`
    - command2 will not be executed if command1 succeeds
    - `ls /blah || date` will display current date after "No such file or directory"
    - `ls || date` will display just the directory listing
    - command2 is like a Plan B if command1 doesn't succeed.
  - Example `ls /blah ; date ; wc -l /etc/profile ;`
  - If we use parenthesis ie `(ls /blah ; date ; wc -l /etc/profile ;)` the command gets executed in a subshell and is returned back to the shell we are using.
  - We can use `echo $BASH_SUBSHELL` to return an integer which tells us at what level of execution we are.
    - `(echo $BASH_SUBSHELL)` will report a value of 1 
    - `(ls; (date; echo $BASH_SUBSHELL))` will report a value of 2
  - Launching too many subshells could be expensive computationally.
* File Descriptors
  - Every command in linux has 3 file descriptors - `stdin` (0) , `stdout` (1), `stderr` (2).
    - `stdin` is a pointer to a stream that is coming from the keyboard or use input
    - `stdout` or `stderr` usually points to the screen where the display or output is made.
    - the three pointers are looking at only the stream of characters.
    - they can be directed to a file or a command, or the default behaviour can be left as it is.
  - Combining a command and a file
    - `command > file1` 
      - `stdout` is redirected to `file1`
      - `file1` will be created if it does not exist
      - if `file1` exists, its contents will be overwritten
      - example : `ls -1 /usr/bin > file1` - displays no output on the screen because there is no error
      - `ls -1 /blah > file1` - displays an error. file1 is overwritten and is now 0 Bytes.
      - `hwinfo > hwinfo.txt`
      - trying this command in a folder where there is no w permissions will generate an error
      - The `cat` command tries to read from the provided file name if not given it tries to read from stdin (keyboard)
        - `cat > file1` will allow you to type content. The feature could be used to create text files on the command line. You can come out using the `Ctrl`+`D` option.
        - `cat file1` displays the content of `file1`
        - `cat` takes input from the keyboard and displays it on the screen (line by line; when you press enter) - Finish by pressing `Ctrl`+`D` to signify end of file.
    - `command >> file1` 
      - contents will be appended to file1
      - new file1 will be created if it does not exist.
      - Example : `date >> file2 ; wc -l /etc/profile >> file2 ; file /usr/bin/znew >> file2 ;`
      - `cat >> file1` to append text to a file from command line. Come out using `Ctrl` + `D`

### Redirections
* combining command and file (continued ..)
  - (contd..)
    - `command 2> file1`
      - redirects `stderr` to `file1`
      - `file1`, if it exists, will be overwritten.
      - `file1` will be created if it does not exist.
      - Example `ls $HOME /blah 2> error.txt`
    - `command > file1 2> file2`
      - `stdout` is redirected to `file1`
      - `stderr` is redirected to `file2`
      - Contents of file1 and file2 will be overwritten.
      - The output is in one file and the errors are in another file.
      - Example : `ls $HOME /blah > output.txt 2> error.txt`
      - `ls -R /etc > output.txt 2> error.txt` - permission related errors in error.txt
    - `command < file1`
      - `stdin` is redirected - a command expecting input from the keyboard could take the input from a file.
      - Example : `wc /etc/profile` behaves similar to `wc < /etc/profile`
    - `command > file1 2>&1`
      - command output will be redirected to `file1`
      - `2>` indicates `stderr` and that is being redirected to `&1` (first stream) which is `stdout`
      - contents of `file1` will be overwritten
      - Example : `ls $ HOME /blah > file1` output alone is sent to file1. Error on screen
      - Example : `ls $ HOME /blah > file1 2>&1` output and error is sent to file1.
    - `command1 | command2` Pipe
      - `stdout` output of command 1 is sent to `stdin` of command2 as input
      - Example `ls /usr/bin | wc -l`
    - `command1 | command2 > file1`
      - command1 and command2 are combined and the `stdout` of command2 is sent to `file1`. Errors are still shown on the screen.
      - Example `ls /usr/bin | wc -l > file1` - file1 has the number of lines counted by wc 
    - `command > file1 2> /dev/null`
      - `/dev/null` file - A sink for output to be discarded. Like a "black hole"
      - We normally don't do anything with the `/dev` folder as there are sensitive system files there.
      - If you are confident that the script is running well and you do not want to display any error on the screen, you can redirect the `stderr` to `/dev/null` 
      - `stderr` is redirected to `/dev/null`
      - Example : `ls $HOME /blah > file1 2> /dev/null`
      - Example : `ls -R /etc > file1 2> /dev/null` - file1 contains the output except errors
    - `command1 | tee file1`
      - Used in sitiations where you want to have a copy of the output in a file as well as on the screen.
      - The `tee` command reads from `stdin` and writes to `stdout` and file/s.
      - Example : `ls $HOME | tee file1` also `ls $HOME | tee file1 file2` for creating multiple copies
      - `diff file1 file2` comapares files line by line
        - no output if the files are identical 
      - Example : `ls $HOME /blah | tee file1 file2 | wc -l` - Here  `tee` keeps copy of output in a file and also sends output to `wc -l` for further processing.
      - Example : `ls $HOME /blah 2> /dev/null | tee file1 file2 | wc -l` to supress errors. Note location of `2>` is since the error is generated there.

### Software Management
* Using Package Management Systems 
  - Tools for installing, updating, removing and managing software
  - Install new / updated software across network
  - Package - File look up, both ways 
    - Which files are given by a particular package and which package contains a given file
  - Database of packages on the system including versions (compatibility and requirements)
  - Dependency checking
  - Signature verification tools (to check authenticity of source of the software)
  - Tools for building packages (to build packages from soure code - particularly true for kernel modules)

* Package types 
  - Package
    - RPM
      - Red Hat
        - CentOS
        - Fedora
        - Oracle Linux
      - SUSE Enterprise Linux
        - OpenSUSE
    - DEB
      - Debian
        - Ubuntu
          - Mint
        - Knoppix
* Commands
  - `lsb_release -a` to find version of Operating System
  - When searching for packages for this version of the OS you can search by OS code name eg: `focal`
* Architectures
  - amd64 | x86_64
  - i386 | x86
  - arm (RISC5 Sakthi)
  - ppc64el | OpenPOWER
  - all | noarch |src (not tied to any architecture)
* Commands
  - `uname -a` gives the kernel version and the type of architecture.
* Tools 
	- Package Type
		- RPM
			- Yellowdog Updater Modifier (yum)
				- Red Hat Package Manager (rpm)
				- Dandified YUM (dnf)
		- DEB
			-  synaptic (GUI)	
			-  aptitude (Command Line)
				- Advanced Package Tool (apt)
					- dpkg
						- dpkg-deb
* Package managemet in Ubuntu using `apt`
	- Inquiring package db
		- Search packages for a keyword
			- `apt-cache search keyword`
		- List all packages
			- `apt-cache pkgnames`
			- `apt-cache pkgnames | sort | less` for page by page sorted display
			- `apt-cache pkgnames nm` for all packages starting with nm
		- Display package records of a package
			- `apt-cache show -a package`

* Package Names
	- Package
		- RPM
			- package-version-release.architecture.rpm
		- DEB
			- package_version-revision_architecture.deb
			- eg : `pool/universe/n/nmap/nmap_7.80+dfsg1-2build1_amd64.deb`
* Package Priorities
	- required : essential to proper functioning of the system
	- important : provides functionality that enables the system to run well
	- standard : included in a standard system installation
	- optional : can omit if you do not have enough storage
	- extra : could conflict with packages with higher priority, has specialized requirements, install only if needed.
	- Priority is displayed as `extra` in the output of `apt-cache show nmap` or `apt-cache show wget` for example.
* Package Sections
	- [Package Sections for Ubuntu focal](https://packages.ubuntu.com/focal/)
	- `apt-cache show fortunes` shows `Section : universe/games`
* Checksums
	- For a small change in the original file the checksum is very different. This is useful to chack if the original file has been tampered or not. 
	- Can be used to verify that nothing has gone wrong to the contents of the file while downloading. 
	- md5sum 
		- 128 bit string
		- `md5sum filename`
	- SHA1
		- 160 bit string
		- `sha1sum filename`
	- SHA256
		- 256 bit string
		- `sha256sum filename`

___
4.2
* Who can install packages in Linux OS ?
	- administrators
	- sudoers in the case of Ubuntu
	- Only sudoers can install/upgrade/remove packages
	- a sudo command can be executed by those who are listed in `/etc/sudoers`
	- Command `sudo cat /etc/sudoers` . If the current `$USER` is not in the sudoers file the incident will be reported.
	- In the file the users listed under `# User privilege specification` have sudo permission.
	- sudo attempts and authentication failures get recorded in `/var/log/auth.log`. View using `sudo tail -n 100 /var/log/auth.log`
* When installing a package the system knows the website/server from which the packages have to be downloaded
	- This information is stored in the folder `/etc/apt`
	- Uncommented lines in the file `sources.list` have the debian/ubuntu sources 
	- A directory `sources.list.d` stores sources for third party software. Allows `apt update` to know new versions to download from repositories stored in these files
	- Synchronize package overview files - `sudo apt-get update` fetches updates and keeps them in cache
	- Upgrade all installed packages - `sudo apt-get upgrade` upgrades the packages. It lists how many updates are going to be affected and how much data is going to be downloaded.
	- `sudo apt autoremove` to remove unused packages that were earlier installed to satisfy a particular dependency but are not needed now.
	- Install a package - `sudo apt-get install packagename`
	- `sudo apt-get remove packagename` to remove a particular package
	- `sudo apt-get reinstall packagename` to fix problems caused by accedential file deletions.
	- Clean local repository of retreived package files - `apt-get clean`
	- Purge package files from the system - `apt-get purge package`
* Package management in Ubuntu using `dpkg`
	- Allows installation directly from a `.deb` file. Package management at a lower level.
	- `/var/lib/dpkg` has some information about the packages
		- Files - `arch`,`available`,`status`
			- `cat arch` displays the architectures for which packages have been installed on the system - `amd64`,`i386`
			- `less available` displays list of packages with info. 
			- `less status` displays if a particular package is installed or not
		- Folder - `info`
			- contains a set of files for each of the packages that have been installed
			- `ls wget*` will give files with information about `wget`
				- `more wget.conffiles`gives location of configuration file 
				- `more wget.list` displays list of files that would get installed on the system with the package
				- `more wget.md4sums` displays the listof md5sums of the installed files. (Used to catch tampering)
* Using `dpkg`
	- List all packages whose names match the pattern
		- `dpkg -l pattern`
	- List installed files that came from packages
		- `dpkg -L package`
	- Display/Report the status of packages
		- `dpkg -s package`
	- Search installed packages for a file
		- `dpkg -S pattern` 
		- eg : `dpkg -S /usr/bin/perl` shows the package from which the executable has come. ie : `perl-base`
	- To query the dpkg database about all the packages - `dpkg-query`
		- Example `dpkg-query -W -f='${Section} ${binary:Package}\n' | sort | less`
		- Example where output is filtered `dpkg-query -W -f='${Section} ${binary:Package}\n' | grep shells`
* Installing a deb package
	- `dpkg -i package_version-revision_architecture.deb`
	- not a good idea since it may have some dependencies that will have to be taken care of manually
	- Do not download deb files from unknown sources and install it on the system
	- By default use package management pointing to a reliable repository
	- Uninstalling packages using `dpkg` is NOT recommended. You may be removing a package that is required by many other packages.
* When compatibility issues cannot be resloved one can use `snap` or `docker` as alternatives when you are unable to install a particular version of a package.

___
