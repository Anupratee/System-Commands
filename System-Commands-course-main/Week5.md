# Week 5 Notes
---
## Scripts 
* Software Tools Principles (Ref: Classic Shell Scripting – Arnold Robbins & Nelson H.F. Beebe)
  - Do one thing well
  - Process lines of text, not binary
  - Use regular expressions
  - Default to standard I/O
  - Don’t be chatty
  - Generate same output format accepted as input
  - Let someone else do the hard part
  - Detour to build specialized tools

```bash
#! interpreter
# comments
commands
loops
variables
case statements
functions
```
* program - shell , awk , sed , python , ruby , perl
* script
  - sourced
    - `. scriptname` `source scriptname`
    - PID same as the current shell commands are executed one after other shell environment continues
    - Used to prepare environment
  - executed
    - `./scriptname`
    - Needs execution permission
    - New process gets created to run script
    - PID is not same as the shell commands are executed one after other
    - New environment lost after return
    - Used to create a new functionality
* Script location
  - Use absolute path or relative path while executing the script
  - Keep the script in folder listed in $PATH
  - Watch out for the sequence of directories in $PATH
* bash environment
  - Login shell
    - ```
      /etc/profile
      ~/.bash_profile
      ~/.bash_login
      ~/.profile
      ```
  - Non-login shell
    - ```
      /etc/bash.bashrc
      ~/.bashrc
      ```
* Output from shell scripts
  - `echo`
    - simple
    - terminates with a newline if -n option not given
    - `echo My home is $HOME`
  - `printf`
    - supports format specifiers like in C
    - `printf “My home is %s\n” $HOME`
* Input to shell scripts
  - `read var`
    - string read from command line is stored in `$var`
* Shell Script arguments
  - `$0` name of the shell program
  - `$#` number of arguments passed
  - `$1` or `${1}` first argument
  - `${11}` eleventh argument
  - `$*` or `$@` all arguments at once
  - `“$*”` all argument as a single string
  - `“$@”` all argument as a separate strings
  - example : `./myscript.sh -l arg2 -v arg4`
* Command substitution
  - ```var=`command` ```
  - `var=$(command)`
    - command is executed and the output is substituted.
    - Here, the variable var will be assigned with that output.
* for do loop
  - ```
    for var in list
    do
      commands
    done
    ```
  - commands are executed once for each item in the list
  - space is the field delimiters
  - set IFS if required. If the field separator is different from space.

* case statement
  - ```
    case var in
    pattern1)
      commands
      ;;
    pattern2)
      commands
      ;;
    esac
    ```
  - commands are executed each pattern matched for var in the options
* if loop
  - ```
    if condition
    then
      commands
    fi
    ```
  - ```
    if condition; then
      commands
    fi
    ```
  - commands are executed only if condition returns true
* Conditions
  - test expression
    - `test -e file`
  - `[ exprn ]`
    - `[ -e file ]`
  - `[[ exprn ]]`
    - `[[ $ver == 5.*]]`
  - `(( exprn ))`
    - `(( $v ** 2 > 10 ))`
  - command
    - `wc -l file`
  - pipeline
    - `who|grep “joy” > /dev/null`
  - For negation `! condition`
* String Comparison , Numeric, file comparison
  - expressions
    - unary
    - binary
* test numeric comparisons
  - Table
    |  Comparison | Description  |
    |---|---|
    |	`$n1 -eq $n2`	|	Check if n1 is equal to n2	|
    |	`$n1 -ge $n2`	|	Check if n1 is greater than or equal to n2	|
    |	`$n1 -gt $n2`	|	Check if n1 is greater than n2	|
    |	`$n1 -le $n2`	|	Check if n1 is less than or equal to n2	|
    |	`$n1 -lt $n2`	|	Check if n1 is less than n2	|
    |	`$n1 -ne $n2`	|	Check if n1 is not equal to n2	|
* test string comparisons
  - Table
    |  Comparison | Description  |
    |---|---|
    |	`$str1 = $str2`	|	Check if str1 is same as str2	|
    |	`$str1 != $str2`	|	Check if str1 is not same as str2	|
    |	`$str1 < $str2`	|	Check if str1 is less than str2	|
    |	`$str1 > $str2`	|	Check if str1 is greater than str2	|
    |	`-n $str2`	|	Check if str1 has length greater than zero	|
    |	`-z $str2`	|	Check if str1 has length of zero	|
* Unary file comparisons
  - Table
    |  Comparison | Description  |
    |---|---|
    |	`-e file`	|	Check if file exists	|
    |	`-d file`	|	Check if file exists and is a directory	|
    |	`-f file`	|	Check if file exists and is a file	|
    |	`-r file`	|	Check if file exists and is readable	|
    |	`-s file`	|	Check if file exists and is not empty	|
    |	`-w file`	|	Check if file exists and is writable	|
    |	`-x file`	|	Check if file exists and is executable	|
    |	`-O file`	|	Check if file exists and is owned by current user	|
    |	`-G file`	|	Check if file exists and default group is same as that of current user	|
* Binary file comparisons
  - Table
    |  Comparison | Description  |
    |---|---|
    |	`file1 -nt file2`	|	Check if file1 is newer than file2	|
    |	`file1 -ot file2`	|	Check if file1 is older than file2	|
* while do loop
  - ```
    while condition
    do
      commands
    done
    ```
  - commands are executed only if condition returns true
* until do loop
  - ```
    until condition
    do
      commands
    done
    ```
  - commands are executed only if condition returns false
* functions
  - definition
  - ```
    myfunc()
    {
      commands
    }
    ```
  - call
  - ```
    myfunc
    ```
  - `commands` are executed eachtime `myfunc` is called
  - Definitions must be before the calls
* Demo
  - ```bash
    #! /bin/bash
    # s1.sh is my first script
    echo I am invoked as 
    echo $0
    echo hello world
    echo the PID of the process running this script is :
    echo $$
    ps --forest
    export myvar=MYVAR
    echo $myvar
    ```
  - Source it using `. s1.sh` or `source s1.sh`. This displays the same PID as the Bash terminal
  - Executing this using `./s1.sh` displays an error as there is no executable permission.
  - Provide executable permission using `chmod 755 s1.sh` and then run using `./s1.sh`
  - Ths time the PID is different. 
  - `ps --forest` shows all the processes that are running and the spawned processes.
  - A variable set during execution in a subshell will not be available in the parent shell. If the script is sourced the variable will be available.
  - `$0` displays which ever way the script has been invoked (absolute or relative path or just the name of the script)
  - `./s1.sh -l arg2` will show `-l` as `$1` and `arg2` as `$2`
  - ```bash
    #! /bin/bash
    # s1.sh modified script
    echo Number of arguments
    echo $#
    echo First argument
    echo $1
    echo Second argument
    echo $2
    if test $1 = $2;
    then
      echo The arguments are the same
    fi
    ```
  - Executing the above script using `./s1.sh hello hello` will say that the arguments are the same.
  - ```bash
    #! /bin/bash
    echo use of for loop
    for i in arg1 arg2 arg3
    do
      echo $i
    done
    ```
  - The above script just prints arg1,arg2 and arg3 on 3 lines
  - ```bash
    #! /bin/bash
    echo use of for loop
    for i in file_{1..9}
    do
      echo $i
    done
    ```
  - prints file_1,file_2 ... file_9 
  - ```bash
    #! /bin/bash
    echo use of for loop
    for i in file_{A..D}{1..9}
    do
      echo $i
    done
    ```
  - prints 36 lines
  - ```bash
    #! /bin/bash
    echo use of for loop
    for i in $(ls /bin/z*)
    do
      echo $i
    done
    ```
  - shows each file in bin directory starting with z
  - `file znew | grep "shell script"` identifies whether the file passed (in bin) is a shell script
  - ```bash
    #! /bin/bash
    echo Shell Scripts in bin directory
    for i in $(ls /bin)
    do
      #echo /bin/$i
      file /bin/$i | grep "shell script"
    done
    ```
  - Prints the files which are shell scripts in the bin directory

  ### Shell programming
##### More features in bash scripts

* Debugging
  - Print the command before executing it
    - `set -x ./myscript.sh`
    - `bash -x ./myscript.sh`
  - Place the `set -x` inside the script

* Combining conditions
  - `[ $a -gt 3 ] && [ $a -gt 7 ]`
  - `[ $a -lt 3 ] || [ $a -gt 7 ]`
  - Example [condition-examples.sh](Example_Files/condition-examples.sh)

* Shell arithmetic
	- Using `let`
		- `let a=$1+5`
		- `let "a= $1 + 5"`
	- Using `expr`
		- `expr $a +20`
		- `expr "$a + 20"`
		- `b=$( expr $a + 20 )`
	- Using `$[ expression ]`
		- `b=$[ $a + 10 ]`
	- Using `$(( expression ))`
		- `b=$(( $a + 10 ))`
		- `(( b++ ))`- Without $. Not intending to return. Useful for incrementing
	- Example [arithmetic-example-1.sh](Example_Files/arithmetic-example-1.sh)
* ### expr command operators

| Expression  | Description  |
|---|---|
|	`a + b`	|	Return arithmetic sum of a and b	|
|	`a - b`	|	Return arithmetic difference of a and b	|
|	`a * b`	|	Return arithmetic product of a and b	|
|	`a / b`	|	Return arithmetic quotient of a divided by b	|
|	`a % b`	|	Return arithmetic remainder of a divided by b	|
|	`a > b`	|	Return 1 if a greater than b; else return 0	|
|	`a >= b`	|	Return 1 if a greater than or equal to b; else return 0	|
|	`a < b`	|	Return 1 if a less than b; else return 0	|
|	`a <= b`	|	Return 1 if a less than or equal to b; else return 0	|
|	`a = b`	|	Return 1 if a equals b; else return 0	|
|	`a \| b`	|	Return a if neither argument is null or 0; else return b	|
|	`a & b`	|	Return a if neither argument is null or 0; else return 0	|
|	`a != b`	|	Return 1 if a is not equal to b; else return 0	|
|	`str : reg`	|	Return the position upto anchored pattern match with BRE str	|
|	`match str reg`	|	Return the pattern match if reg matches pattern in str	|
|	`substr str n m`	|	Return the substring m chars in length starting at position n	|
|	`index str chars`	|	Return position in str where any one of chars is found else return 0	|
|	`length str`	|	Return numeric length of string str	|
|	`+ token`	|	Interpret token as string even if its a keyword	|
|	`(exprn)`	|	Return the value of expression exprn	|

* Example [expr-examples.sh](Example_Files/expr-examples.sh)

* Bench Calculator 
	- An arbitaty preciscion calculator language
	- `bc -l`
		- the `l` option loads the math library.
	- `12^6` or `12.6/3.6`
	- Can be used for floating point operations.
	
* heredoc feature 
	- helps while passing long strings without having to worry about `\n` etc.
	- Example [heredoc-example-1.sh](Example_Files/heredoc-example-1.sh)
	- Example [heredoc-example-2.sh](Example_Files/heredoc-example-2.sh)
	- A hyphen tells bash to ignore leading tabs
	
* PATH variable
	- Example [path-example.sh](Example_Files/path-example.sh)
		- IFS (Internal Field Seperator)
___ 
L6.3

* if-elif-else-fi loop
```bash
if condition1
then
	commandset1
else
	commandset2
fi
```
```bash
if condition1
then
	commandset1
elif condition2
then
	commandset2
elif condition3
then
	commandset3
else
	commandset4
fi
```
* case statement options
	- commandset4 is the default for values of $var not matching what are listed
	
```bash
case $var in
	op1)
		commandset1;;
	op2 | op3)
		commandset2;;
	op4 | op5 | op6)
		commandset3;;
	*)
		commandset4;;
esac
```
* c style for loop : one variable
	- extention of POSIX and maynot be available in all the shells
	- Adding `time` before a script command gives the amount of time taken to execute.

```bash
begin=1
finish=10
for (( a = $begin; a < $finish; a++ ))
do
	echo $a
done
```

* c style for loop : two variables
	- Note: Only one condition to close the for loop

```bash
begin1=1
begin2=10
finish=10
for (( a=$begin1, b=$begin2; a < $finish; a++, b-- ))
do
	echo $a $b
done
```

* processing output of a loop
	- Output of the loop is redirected to the tmp file
```bash
filename=tmp.$$
begin=1
finish=10
for (( a = $begin; a < $finish; a++ ))
do
	echo $a
done > $filename
```

* break
	- Break out of inner loop
```bash
n=10
i=0
while [ $i -lt $n ]
do
	echo $i
	(( i++ ))
	if [ $i -eq 5 ]
	then
		break
	fi
done
```
	- Break out of outer loop
```bash
n=10
i=0
while [ $i -lt $n ]
do
	echo $i
	j=0
	while [ $j -le $i ]
	do
		printf "$j "
		(( j++ ))
		if [ $j -eq 7 ]
		then
			break 2
		fi
	done
	(( i++ ))
done
```

* continue
	- Continue will skip rest of the commands in the loop and goes to next iteration

```bash
n=9
i=0
while [ $i -lt $n ]
do
	printf "\n loop $i:"
	j=0
	(( i++ ))
	while [ $j -le $i ]
	do
		(( j++ ))
		if [ $j -gt 3 ] && [ $j -lt 6 ]
		then
			continue
		fi
		printf "$j "
	done
done
```

* shift
	- shift will shift the command line arguments by one to the left.
	- shift is destructive - after the arguments are shifted t the left they are gone. This is only helpful if you don't need the arguments later.
	- n checks if it is a non-zero argument
	- except $0, which is the name of the script, the rest of the arguments get popped 

```bash
i=1
while [ -n "$1" ]
do
	echo argument $i is $1
	shift
	(( i++ ))
done
```

* exec
	- `exec ./my-executable --my-options --my-args`
	- To replace shell with a new program or to change i/o settings
	- If new program is launched successfully, it will not return control to the shell
	- If new program fails to launch, the shell continues

* eval
	- `eval my-arg`
	- Execute argument as a shell command
	- Combines arguments into a single string
	- Returns control to the shell with exit status
	- Example [eval-example.sh](Example_Files/eval-example.sh)
* function 
	- Example [function-example.sh](Example_Files/function-example.sh)
* getopts
	- This script can be invoked with only three options: `a`, `b`, `c`. The options `b` and `c` will take arguments.
	- Example [getopts-example.sh](Example_Files/getopts-example.sh)

```bash
while getopts "ab:c:" options;
do
	case "${options}" in
		b)
			barg=${OPTARG}
			echo accepted: -b $barg
			;;
		c)
			carg=${OPTARG}
			echo accepted: -c $carg
			;;
		a)
			echo accepted: -a
			;;
		*)
			echo Usage: -a -b barg -c carg
			;;
	esac
done
```

* select loop
	- Text Menu
	- Example [select-example.sh](Example_Files/select-example.sh)
```bash
echo select a middle one
select i in {1..10}
do
	case $i in
		1 | 2 | 3)
			echo you picked a small one;;
		8 | 9 | 10)
			echo you picked a big one;;
		4 | 5 | 6 | 7)
			echo you picked the right one
			break;;
	esac
done
echo selection completed with $i
```

* Additional notes
	- Warning : Never `eval` a user supplied string on any command line
	- eval is sending the strings to the shell and printing them out.
	- Can source a file with functions to use it in a shell script
	- `source mylib.sh`
	- Do not give set uid permission to the scripts unless you know what you are doing

___
