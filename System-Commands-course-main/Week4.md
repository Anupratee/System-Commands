### Week 4 Notes

#### Pattern Matching
* Regular Expressions `regex` and `grep` commands
	- POSIX standard
		- IEEE 1003.1-2001 IEEE Standard for IEEE Information Technology – Portable Operating System Interface (POSIX(TM))
		- [Refer](https://standards.ieee.org/standard/1003_1-2001.html)
	- POSIX defines regular expressions to be of 2 different types - Basic and Extended.
* Regex
	- regex is a pattern template to filter text
	- BRE: POSIX Basic Regular Expression engine
	- ERE: POSIX Extended Regular Expression engine
* Why learn regex?
	- PRocess some input from the user or perform some string operations.
	- Languages: Java, Perl, Python, Ruby, ...
	- Tools: grep, sed, awk, ...
	- Applications: MySQL, PostgreSQL, ...
* Usage
	- `grep ‘pattern’ filename` - to operate on every line in the file
	- `command | grep ‘pattern’`
		- the `grep` command operates line after line. A common feature in many utilities in linux.
		- enclose pattern in single quotes 
	- Default engine: BRE
	- Switch to use ERE in 2 ways:
		- `egrep ‘pattern’ filename`
		- `grep -E ‘pattern’ filename`
* ### Special characters (BRE & ERE)

|  Character 	| Description  	|
|---	|---	|
|  `.` 	|   Any single character except null or newline 	|
|  `*`	|   Zero or more of the preceding character / expression	|
|  `[]` 	|   Any of the enclosed characters; hyphen (-) indicates character range	|
|  `^` 	|   Anchor for beginning of line or negation of enclosed characters	|
|  `$` 	|   Anchor for end of line	|
|  `\` 	|   Escape special characters	|

* ### Special characters (BRE)

|  Character 	| Description  	|
|---	|---	|
|  `\{n,m\}` 	|   Range of occurances of preceding pattern at least n and utmost m times 	|
|  `\( \)` 	|   Grouping of regular expressions 	|

* ### Special characters (ERE)

|  Character 	| Description  	|
|---	|---	|
|  `{n,m}` 	|   Range of occurances of preceding pattern at least n and utmost m times 	|
|  `()` 	|   Grouping of regular expressions 	|
|  `+` 	|   One or more of preceding character / expression 	|
|  `?` 	|   Zero or one of preceding character / expression 	|
|  `\|` 	|   Logical OR over the patterns 	|

* ### Character Classes

|  Class 	| Description  	|
|---	|---	|
|  `[[:print:]]` 	|   Printable 	|
|  `[[:alnum:]]` 	|   Alphanumeric 	|
|  `[[:alpha:]]` 	|   Alphabetic 	|
|  `[[:lower:]]` 	|   Lower case 	|
|  `[[:upper:]]` 	|   Upper case 	|
|  `[[:digit:]]` 	|   Decimal digits 	|
|  `[[:blank:]]` 	|   Space / Tab 	|
|  `[[:space:]]` 	|   Whitespace 	|
|  `[[:punct:]]` 	|   Punctuation 	|
|  `[[:xdigit:]]` 	|   Hexadecimal 	|
|  `[[:graph:]]` 	|   Non-space 	|
|  `[[:cntrl:]]` 	|   Control characters 	|

* Backreferences
	- `\1` through `\9`
	- `\n` matches whatever was matched by nth earlier paranthesized subexpression
	- A line with two occurances of hello will be matched using: `\(hello\).*\1`

* ### BRE operator precedence

| Highest to Lowest	|
|---	|
|  [..] [==] [::] char collation 	|
|  \metachar 	|
|  [ ] Bracket expansion 	|
|  \( \) \n subexpresions and backreferences 	|
|  * \{ \} Repetition of preceding single char regex 	|
|  Concatenation 	|
|  ^ $ anchors 	|

* ### ERE operator precedence

| Highest to Lowest	|
|  ---	|
|  [..] [==] [::] char collation 	|
|  \metachar 	|
|  [ ] Bracket expansion 	|
|  ( ) grouping 	|
|  * + ? { } Repetition of preceding regex 	|
|  Concatenation 	|
|  ^ $ anchors 	|
|  \| alternation 	|

* ### Examples using grep
	- [Example File names.txt (Containing Names/Roll-No)](Example_Files/names.txt)
	- Basic use
		- `grep 'Raman' names.txt` matches line with Raman Singh
		- `cat names.txt | grep 'ai'` matches line with Snail
	- Usage of `.`
		- `cat names.txt | grep 'S.n'` matches lines with Singh and Sankaran
	- Usage of `$`
		- `cat names.txt | grep '.am$' ` matches lines that end with xam
	- Escaping a `.`
		- `cat names.txt | grep '\.'` matches lines that have a `.`
	- Using anchors at the begining
		- `cat names.txt | grep '^M'` matches lines begining with m
	- Case insensitive matching with the `i` flag
		- `cat names.txt | grep -i '^e'` matches lines begining with e or E.
	- Word boundaries `\b`
		- `cat names.txt | grep 'am\b'` matches lines with words that end with 'am'
	- Use of square brackets `[]` to give options
		- `cat names.txt | grep 'M[ME]'` matches lines containing 'MM' or 'ME'
		- `cat names.txt | grep '\bS.*[mn]'` matches lines containing words begining with S and ending with m or n.
		- `cat names.txt | grep '[aeiou][aeiou]'` matches lines that have 2 vowels side by side
		- `cat names.txt | grep 'B90[1-4]'` matches words begining with B90 and ending with range 1-4. 
		- `cat names.txt | grep 'B90[^1-4]'` matches words begining with B90 and ending with characters other than the range 1-4. A hat inside square brackets implies negation
	 - Specifying occurances using escaped braces
	 	- `cat names.txt | grep 'M\{2\}'` matches lines which have 'MM'
	 	- `cat names.txt | grep 'M\{1,2\}'` matches lines which have one or 2 'M's
	 - Grouping patterns that are matched using parenthesis. Repeating whatever is matched by using `\1`
	 	- `cat names.txt | grep '\(ma\)'` matches lines containing 'ma'
	 	- `cat names.txt | grep '\(ma\).*\1'` matches a pattern begining with 'ma' and ending with 'ma' eg: U'mair Ahma'd. The `\1` back-references the first parenthesis.
	 	- `cat names.txt | grep '\(.a\).*\1'` matches a pattern like 'Mary Ma'nickam
	 	- `cat names.txt | grep '\(a.\)\{3\}'` matches a pattern like S'agayam'
	 - Using Extended Regular Expression Engine
	 	- `cat names.txt | egrep 'M+'` will match lines where M occures one or more times.
	 	- `cat names.txt | egrep '^M+'` will match lines where M occures one or more times at the begining of a line.
	 	- `cat names.txt | egrep '^M*'`
	 		- `cat names.txt | egrep '^M*a'` matches lines where 'M' may or may not occur followed by 'a'
	 		- `cat names.txt | egrep '^M.*a'` matches lines where 'M' has to occur at the begining of a line followed by any number of characters and ending with 'a'
	 		- Watch out for the interpretation of `*`
	 	- `cat names.txt | egrep '(ma)+'` 'ma' could occur one or more times.
	 	- `cat names.txt | egrep '(ma)*'` 'ma' could occur zero or more times.
	 - Use of pipe as an alternation between 2 patterns of strings to be matched
	 	- `cat names.txt | egrep '(ED|ME)'` matches lines containing 'ED' or 'ME'
	 	-  `cat names.txt | egrep '(Anu|Raman)'` matches lines containing 'Anu' or 'Raman'. Length of string on both sides of pipe need not be the same.
	 	-  `cat names.txt | egrep '(am|an)$'` matches lines containing 'am' or 'an' at the end.

___
4.4
* ### More Examples using grep and egrep
	- Get package names that are exactly 4 characters long
		- `dpkg-query -W -f'${Section} ${binary:Package}\n' | egrep ' .{4}$'`
	- Get package names that are from the math section
		- `dpkg-query -W -f'${Section} ${binary:Package}\n' | egrep '^math'`
	- [Example File chartype.txt (Containing few lines with control character)](Example_Files/chartype.txt)	
		- control character inserted using `echo $'\cc' >> chartype.txt`
	- get lines that have an alphanumeric character at the begining of the line
		- `cat chartype.txt | grep '^[[:alnum:]]'`
	- get lines that have digits at the end of the line
		- `cat chartype.txt | grep '[[:digit:]]$'`
	- get lines that have a ctrl character
		- `cat chartype.txt | grep '[[:ctrl:]]'`
		- `cat chartype.txt | grep -v '[[:cntrl:]]'` will show the reverse including the empty lines
	- get lines that do not have a ctrl character
		- `cat chartype.txt | grep '[^[:cntrl:]]'` (This does not work as intended)
	- get lines that have printable characters (exclude blank lines)
		- `cat chartype.txt | grep '[[:print:]]'`
	- get lines that have blank space characters (exclude blank lines)
		- `cat chartype.txt | grep '[[:blank:]]'`
	- `[[:graph:]]` is used to match any non space character
	- To skip blank lines
		- `cat chartypes.txt | egrep -v '^$'` Here `-v` excludes and `'^$'` captures empty lines
	- Identify a line with a 12 digit number
		- `egrep '[[:digit:]]{12}' patterns.txt`
	- Identify a line with a 6 digit number (Use word boundaries)
		- `egrep '\b[[:digit:]]{6}\b' patterns.txt`
	- Match lines containing Roll Number of the form MM22B001
		- `egrep '\b[[:alpha:]]{2}[[:digit:]]{2}[[:alpha:]][[:digit:]]{3}\b' patterns.txt`
	- Match urls without the http
		- `egrep '\b[[:alnum:]]+\.[[:alnum:]]+\b' patterns.txt`
	- **Trimming text**
		- top to bottom using `head` and `tail`
		- sidways or horizontal trimming of lines using `cut`
			- `cut -c 1-4 fields.txt` displays only first 4 characters. Can also use `-4` for begining to 4th place or `2-` to cut from 2nd place to end.
			- `cat fields.txt | cut -d " " -f 1` - This uses " " as a delimiter `-d` and prints only the first field `-f 1`
			- `cat fields.txt | cut -d ' ' -f 1-2` - to get both fields
			- Capture `hello world` from `1234;hello world,line 1`
				- `cat fields.txt | cut -d ';' -f 2 | cut -d "," -f 1`
				- `egrep ';.*,' fields.txt` (To trim pass the output of grep to `sed`)
			- Combining this with top to bottom trimming
				- `cat fields.txt | cut -d ';' -f 2 | cut -d "," -f 1 | head -n 2 | tail -n 1`

* ### Own experiments using regex
	- Get strictly alphanumeric words
		- `cat test.txt | egrep '\b([a-z]+[0-9]+|[0-9]+[a-z]+)\b'`

* ### REPLIT Code with Us session
	- Getting files with a specific permission pattern from a file
		- `cat lsinfo.txt | grep 'rw-r--r--' ;`
	- Get all files excluding directories in lsinfo.txt whose last modified date is in January
		- `cat lsinfo.txt | grep '^[^d].*Jan'`
	- To count the number of lines that starts with a capital letter and contains the word it (case-sensitive)
		- `cat twocities.txt | grep -c '^[[:upper:]].*\bit\b' `
	- to display all the lines that does not contain the word "we" in it
		- `cat twocities.txt | egrep -v '\bwe\b'`
	- using cut to display only the countries and its capitals of file.txt in the format Country, Capital (eg in file.txt : India, New Delhi; Asia)
		- `cat file.txt | cut -d ';' -f 1 `
	- all the countries in the file file.txt sorted alphabetically by name in reverse order
		- `cat file.txt | cut -d ',' -f 1 | sort -r`
	- cut command to extract the continents (including the one white space in the beginning) of the first 5 lines of file.txt and store it in another file named continent.txt
		- `head -n 5 file.txt | cut -d ';' -f 2 > continent.txt `
	- list the names of all the c++ files in the current directory which contains a line such that the line starts with the string void main() and ends with the character {. There should be one or more spaces/tabs between the characters { and ).
		- `egrep '^void[[:space:]]main\(\)[[:space:]]+{$' *.cpp | cut -d '.' -f 1`
		- `grep '^void[[:space:]]main()[[:space:]][[:space:]]*{$' *.cpp | cut -d '.' -f 1`
	-  print the count of these files in the following line
		- `egrep -l '^void[[:space:]]main\(\)[[:space:]]+{$' *.cpp |tee /dev/tty | wc -l ` 
		- `|tee /dev/tty` is used to print the output to terminal and also pipe the output to the next command.
		- `-l` flag for `grep` and `egrep` prints the name of each input file that matches 
	- command to list all the packages installed on your machine and their versions in the format Package Version in a sorted manner
		- `dpkg-query -W -f='${Package} ${Version}\n' | sort`
