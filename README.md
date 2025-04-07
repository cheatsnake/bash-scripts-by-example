# Bash scripts by example

[`English`](https://github.com/cheatsnake/bash-scripts-by-example/blob/master/README.md) [`Russian`](https://github.com/cheatsnake/bash-scripts-by-example/blob/master/README_RUS.md)

**Bash scripts** are sets of the same commands that can be entered from the keyboard, but compiled into a single file and united by some common purpose. This approach allows you to automate a lot of routine tasks such as building projects or installing new programs. Bash is easy to learn and use, flexible and somehow present in the vast majority of Linux distributions.

Bash scripts have an extension `.sh`:

```
$ touch script.sh
```

It is good practice to specify the path to your terminal at the beginning of each script:

```sh
#! /bin/bash
```

> This technique is called **shebang**, you can read more about it [here](<https://en.wikipedia.org/wiki/Shebang_(Unix)>).

A list of available terminals on your system can be viewed with this command:

```
$ cat /etc/shells
```

## Content

- [Bash scripts by example](#bash-scripts-by-example)
  - [Content](#content)
  - [Hello world](#hello-world)
  - [Comments](#comments)
  - [Variables](#variables)
  - [User input](#user-input)
  - [Passing arguments](#passing-arguments)
  - [Conditions if else](#conditions-if-else)
  - [Comparison operators](#comparison-operators)
    - [Comparison operators for numbers](#comparison-operators-for-numbers)
    - [Comparison operators for strings](#comparison-operators-for-strings)
    - [Comparison operators for files](#comparison-operators-for-files)
  - [Logical operators](#logical-operators)
  - [Arithmetic operators](#arithmetic-operators)
  - [Switch case](#switch-case)
  - [Arrays](#arrays)
  - [While loop](#while-loop)
  - [Until loop](#until-loop)
  - [For loop](#for-loop)
  - [Select loop](#select-loop)
  - [Break and continue keywords](#break-and-continue-keywords)
  - [Functions](#functions)
  - [Local variables](#local-variables)
  - [Keyword readonly](#keyword-readonly)
  - [Signal processing](#signal-processing)
  - [Debugging scripts](#debugging-scripts)
  - [Additional materials](#additional-materials)

## Hello world

```sh
#!/bin/bash
echo "Hello world"
```

Running a script:

```
$ bash script.sh
```

The script can be made into an executable file and run without the `bash` command:

```
$ chmod +x script.sh
```

```
$ ./script.sh
```

## Comments

One line comments:

```sh
# This is comment
# And that too
echo "Hello from bash" # this command outputs a line into the console
```

Multiline comments:

```sh
: 'Multi-line comments are very handy
to describe your scripts in detail.
Use them wisely!'
```

## Variables

```sh
MY_STRING="bash is cool"
echo $MY_STRING # Output the value of a variable
```

> The variable name must not begin with a number

## User input

The `read` command reads user input and writes it to the specified variable:

```sh
echo "Input your name:"
read NAME
echo "Hello, $NAME!"
```

> If no variable is specified, the `read` command will by default save all data to the `REPLY` variable

You can write multiple variables. To do this, when entering from the terminal, the values must be separated by a space:

```sh
read V1 V2 V3
echo "1st var: $V1"
echo "2nd var: $V2"
echo "3rd var: $V3"
```

```
$ bash script.sh
$ hello world some other text
1st var: hello
2nd var: world
3rd var: some other text
```

The `-a` flag allows you to create an array in which user input lines separated by spaces will be written:

```sh
read -a NAMES
echo "Array of names: ${NAMES[0]}, ${NAMES[1]}, ${NAMES[2]}"
```

```
$ bash script.sh
Alex Mike John
Array of names: Alex, Mike, John
```

The `-p` flag allows you not to carry user input to the next line.

The `-s` flag allows you to hide the characters you enter (as you do when entering a password).

```sh
read -p "Enter your login: " LOGIN
read -sp "Enter your password: " PASSWD
```

```
$ bash script.sh
Enter your login: bash_hacker
Enter your password: ******
```

## Passing arguments

Arguments are simply values that can be specified when running the script.

All passed arguments are assigned a unique name equal to their ordinal number:

```sh
echo "Argument 1 - $1; argument 1 - $2; argument 1 - $3."
```

```
$ bash script.sh hello test 1337
Argument 1 - hello; argument 1 - test; argument 1 - 1337.
```

The null argument is always the name of the script file:

```sh
echo "You have run the file $0"
```

```
$ bash script.sh
You have run the file script.sh
```

All arguments can be put into a named array:

```sh
args=("$@")
echo "Arguments received: ${args[0]}, ${args[1]}, ${args[2]}."
```

```
$ bash script.sh some values 123
Arguments received: some, values, 123
```

> `@` - is the name of the default array that stores all the arguments (except for the null one)

The number of arguments passed (with the exception of zero) is stored in the `#` variable:

```sh
echo "Total arguments received: $#"
```

## Conditions if else

Conditions always start with the keyword `if` and end with `fi`:

```sh
echo "Enter your age:"
read AGE

if (($AGE >= 18))
then
	echo "Access allowed"
else
	echo "Access denied"
fi
```

```
$ bash script.sh
Enter your age:
19
Access allowed

$ bash script.sh
Enter your age:
16
Access denied
```

There can be as many conditions as you want, for this purpose the construct `elif` is used, which also as `if` can check conditions:

```sh
read COMMAND

if [ $COMMAND = "help" ]
then
	echo "Available commands:"
	echo "ping - will return the PONG string"
	echo "version - returns the version of the program"
elif [ $COMMAND = "ping" ]
then
	echo "PONG"
elif [ $COMMAND = "version" ]
then
	echo "v1.0.0"
else
	echo "The command is not defined. Use the 'help' command for help."
fi
```

> Note that the `if` and `elif` constructions are always followed by a line with the keyword `then`. <br>
> Also remember to separate conditions with spaces inside the curly braces -> `[ condition ]`.

## Comparison operators

Different comparison operators can be used for numbers and strings. Their complete lists with examples are given in the tables below.

> Note that different operators are used with certain brackets

### Comparison operators for numbers

| Operator | Description              | Example            |
| -------- | ------------------------ | ------------------ |
| -eq      | is equal to              | if [ $age -eq 18 ] |
| -ne      | does not equal           | if [ $age -ne 18 ] |
| -gt      | more than                | if [ $age -gt 18 ] |
| -ge      | greater than or equal to | if [ $age -ge 18 ] |
| -lt      | less than                | if [ $age -lt 18 ] |
| -le      | less than or equal to    | if [ $age -le 18 ] |
| >        | more than                | if (($age > 18))   |
| <        | less than                | if (($age < 18))   |
| =>       | greater than or equal to | if (($age => 18))  |
| <=       | less than or equal to    | if (($age <= 18))  |

### Comparison operators for strings

| Operator | Description                                            | Example                |
| -------- | ------------------------------------------------------ | ---------------------- |
| =        | equality check                                         | if [ $str = "hello" ]  |
| ==       | equality check                                         | if [ $str == "hello" ] |
| !=       | check for NOT equality                                 | if [ $str != "hello" ] |
| <        | comparison less than ASCII character code              | if [[$str < "hello"]]  |
| >        | comparison of more than the ASCII character code       | if [[$str > "hello"]]  |
| -z       | check if the string is empty                           | if [ -z $str ]         |
| -n       | check if there is at least one character in the string | if [ -n $str ]         |

There are also operators for checking various conditions on files:

### Comparison operators for files

| Operator | Description                                                               | Example         |
| -------- | ------------------------------------------------------------------------- | --------------- |
| -e       | checks if a file exists                                                   | if [ -e $file ] |
| -s       | checks if a file is empty                                                 | if [ -s $file ] |
| -f       | checks if the file is a normal file and not a directory or a special file | if [ -f $file ] |
| -d       | checks if the file is a directory                                         | if [ -d $file ] |
| -r       | checks if the file is readable                                            | if [ -r $file ] |
| -w       | checks if the file is writable                                            | if [ -w $file ] |
| -x       | checks if the file is executable                                          | if [ -x $file ] |

## Logical operators

Conditions with the "AND" operator return true only when all conditions are true.

> There are several options for writing conditions with logical operators

```sh
if [ $age -ge 18 ] && [ $age -le ]
```

```sh
if [ $age -ge 18 -a $age -le ]
```

```sh
if [[ $age -ge 18 && $age -le ]]
```

Conditions with an OR operator return true when at least one condition is true.

```sh
if [ -r $file ] || [ -w $file ]
```

```sh
if [ -r $file -o -w $file ]
```

```sh
if [[ -r $file || -w $file ]]
```

## Arithmetic operators

```bash
num1=10
num2=5

# Addition
echo $((num1 + num2))      # 15
echo $(expr $num1 + $num2) # 15

# Subtraction
echo $((num1 - num2))      # 5
echo $(expr $num1 - $num2) # 5

# Multiplication
echo $((num1 * num2))       # 50
echo $(expr $num1 \* $num2) # 50

# Division
echo $((num1 / num2))      # 2
echo $(expr $num1 / $num2) # 2

# Residue from division
echo $((num1 % num2))      # 0
echo $(expr $num1 % $num2) # 0
```

> Note that when using multiplication with the `expr` keyword you must use a slash.

## Switch case

It is not always convenient to use if/elif constructions for a large number of conditions. The case construct is better suited for this purpose:

```sh
read COMMAND

case $COMMAND in
	"/help" )
		echo "You opened the help menu" ;;
	"/ping" )
		echo "PONG" ;;
	"/version" )
		echo "Version: 1.0.0" ;;
	* )
		echo "There is no such command :(" ;;
esac
```

> The case with the asterisk \* will only work if none of the conditions above fit.

## Arrays

Arrays allow you to store an entire collection of data in a single variable. This variable can be conveniently and easily interacted with.

```sh
array=('aaa' 'bbb' 'ccc' 'ddd')

echo "Array elements: ${array[@]}"
echo "First element of the array: ${array[0]}"
echo "Array element indexes: ${!array[@]}"

array_length=${#array[@]}
echo "Array length: ${array_length}"
echo "The last element of the array: ${array[$((array_length - 1))]}"
```

```
$ bash script.sh
Array elements: aaa bbb ccc ddd
First element of the array: aaa
Array element indexes: 0 1 2 3
Array length: 4
The last element of the array: ddd
```

> Note that the array elements are separated by a space without a comma.

Array elements can be added/rewritten/deleted as the script runs:

```sh
array=('a' 'b' 'c')

array[3]='d'
echo ${array[@]} # a b c d

array[0]='x'
echo ${array[@]} # x b c d

array[0]='x'
echo ${array[@]} # x b c d

unset array[2]
echo ${array[@]} # x b d
```

## While loop

The while loop repeats the execution of the block of code described between the `do` - `done` keywords until the given condition is true.

```sh
i=0

while (( $i < 5 ))
do
	i=$((i + 1))
	echo "Iteration number $i"
done
```

```
$ bash script.sh
Iteration number 1
Iteration number 2
Iteration number 3
Iteration number 4
Iteration number 5
```

The operation of increasing a number by 1 unit is called an increment, and there is a special notation for it:

```sh
(( i++ )) # post increment
```

```sh
(( ++i )) # pre increment
```

The opposite operation is decrement:

```sh
(( i-- )) # post decrement
```

```sh
(( --i )) # pre decrement
```

With while cycles you can read different files line by line. There are several ways to do this:

```sh
echo "Reading a file line by line:"
while read line
do
	echo $line
done < text.txt
```

```sh
echo "Reading a file line by line:"
cat text.txt | while read line
do
	echo $line
done
```

```sh
echo "Reading a file line by line:"
while IFS='' read -r line
do
	echo $line
done < text.txt
```

## Until loop

The until loop is opposite to the while loop in that it executes the block of code described between the `do` - `done` keywords when the given condition returns false:

```sh
i=5
until (( $i == 0 )) # will be executed until i equals 0
do
	echo "Value of the variable i = $i"
	(( i-- ))
done
```

```
$ bash script.sh
Value of the variable i = 5
Value of the variable i = 4
Value of the variable i = 3
Value of the variable i = 2
Value of the variable i = 1
```

## For loop

The most classic cycle.

```sh
for (( i=1; i<=10; i++ ))
do
	echo $i
done
```

In newer versions of Bash there is a more convenient way to write using the `in` operator:

```sh
for i in {1..10}
do
	echo $i
done
```

The condition after the `in` keyword generally looks like this:

```
{START..END..INCREMENT}
```

> _START_ - which number to start the cycle with; <br> _END_ - up to which number to continue the cycle; <br> _INCREMENT_ - by how much to increment the start number after each iteration (1 – by default).

The for loop can be used to run a set of commands sequentially:

```sh
for command in ls pwd date # List of commands to run
do
	echo "---Running a command $command---"
	$command
	echo "------------------------"
done
```

```
$ bash script.sh
---Running a command ls---
script.sh  text.txt
------------------------
---Running a command pwd---
/home/user/bash
------------------------
---Running a command date---
Sun Jan 22 10:35:51 AM +03 2023
------------------------
```

## Select loop

Extremely handy loop for creating a menu of option selections.

```sh
select color in "Red" "Green" "Blue" "White"
do
	echo "You have selected $color color..."
done
```

```
$ bash script.sh
1) Red
2) Green
3) Blue
4) White
#? 1
You have selected Red color...
#? 2
You have selected Green color...
#? 3
You have selected Blue color...
#? 4
You have selected White color...
```

The `select` loop combines very well with the `case` selection operator. This way you can very easily create interactive console applications with a lot of branching:

```sh
echo "---Welcome to the menu---"

select cmd in "Run" "Settings" "About" "Exit"
do
	case $cmd in
	"Run")
		echo "The program is running"
		echo "Enter the number:"
		read input
		echo "$input squared = $(( input * input ))" ;;
	"Settings")
		echo "Program Settings" ;;
	"About")
		echo "Version 1.0.0" ;;
	"Exit")
		echo "Exiting the program..."
		break ;;
	esac
done
```

## Break and continue keywords

The keyword `break` is used to force an exit from the loops:

```sh
count=1

while (($count)) # always returns the truth
do
	if (($count > 10))
	then
		break # forced exit in spite of the condition after while
	else
		echo $count
		((count++))
	fi
done
```

The `continue` keyword is used to skip the current iteration of the loop and go to the next one:

```sh
for (( i=5; i>0; i-- ))
do
	if ((i % 2 == 0))
	then
		continue
	fi

	echo $i
done
```

```
$ bash script.sh
5
3
1
```

## Functions

Functions are named code sections that can be reused an unlimited number of times.

```sh
hello() {
	echo "Hello World!"
}

# Call the function 3 times:
hello
hello
hello
```

```
$ bash script.sh
Hello World!
Hello World!
Hello World!
```

Functions, just like scripts themselves, can accept arguments. They have the same names, but function arguments are only visible inside the function to which they have been passed:

```sh
echo "$1" # argument passed when running the script

calc () {
	echo "$1 + $2 = $(($1 + $2))"
}

# passing two arguments to the calc function
calc 42 17
```

```
$ bash script.sh hello
hello
42 + 17 = 59
```

## Local variables

If we declare a variable and then declare another one with the same name, but inside the function, we have an overwrite:

```sh
VALUE="hello"

test() {
	VALUE="linux"
}

test
echo $VALUE
```

```
$ bash script.sh
linux
```

To prevent this behavior, the `local` keyword is used in front of the variable name that is declared inside a function:

```sh
VALUE="hello"

test() {
	local VALUE="linux"
	echo "Variable inside a function: $VALUE"
}

test
echo "Global variable: $VALUE"
```

```
$ bash script.sh
Variable inside a function: linux
Global variable: hello
```

## Keyword readonly

By default, every variable created in Bash can subsequently be overwritten. To protect a variable from changes you can use the `readonly` keyword:

```sh
readonly PI=3.14
PI=100

echo "PI = $PI"
```

```
$ bash script.sh
script.sh: line 2: PI: readonly variable
PI = 3.14
```

`readonly` can be used not only at the time the variable is declared, but also afterwards:

```sh
VALUE=123
VALUE=$(($VALUE * 1000))
readonly VALUE
VALUE=555

echo $VALUE
```

```
$ bash script.sh
script.sh: line 4: VALUE: readonly variable
123000
```

The same is true for functions. They can also be overridden, so you can protect them with `readonly` with the `-f` flag:

```sh
test() {
	echo "This is test function"
}

readonly -f test

test() {
	echo "Hello World!"
}

test
```

```
$ bash script.sh
script.sh: line 9: test: readonly function
This is test function
```

## Signal processing

During the execution of scripts, unexpected actions may occur. For example, the user may interrupt the execution of the script with a combination of `Ctrl + C`, or may accidentally close the terminal or some error may occur in the script itself and so on...

In POSIX-systems, there are special signals - process notifications of some event. Their list is defined in the table below:

| Signal  | Code     | Action                | Description                                                  |
| ------- | -------- | --------------------- | ------------------------------------------------------------ |
| SIGHUP  | 1        | Terminate             | Closing the terminal                                         |
| SIGINT  | 2        | Terminate             | Interrupt signal (Ctrl-C) from the terminal                  |
| SIGQUIT | 3        | Terminate (core dump) | The "Quit" signal from the terminal (Ctrl-)                  |
| SIGILL  | 4        | Terminate (core dump) | Invalid CPU instruction                                      |
| SIGABRT | 6        | Terminate (core dump) | Signal sent by abort()                                       |
| SIGFPE  | 8        | Terminate (core dump) | Erroneous arithmetic operation                               |
| SIGKILL | 9        | Terminate             | Process killed                                               |
| SIGSEGV | 11       | Terminate (core dump) | Violation when accessing memory                              |
| SIGPIPE | 13       | Terminate             | Writing to a broken connection (pipe, socket)                |
| SIGALRM | 14       | Terminate             | Alarm expires when the time set by alarm() expires           |
| SIGTERM | 15       | Terminate             | End signal (the default signal for the kill utility)         |
| SIGUSR1 | 30/10/16 | Terminate             | User-defined signal № 1                                      |
| SIGUSR2 | 31/12/17 | Terminate             | User-defined signal № 2                                      |
| SIGCHLD | 20/17/18 | Ignore                | Subsidiary process completed or stopped                      |
| SIGCONT | 19/18/25 | Continue              | Continue executing the previously stopped process            |
| SIGSTOP | 17/19/23 | Stop                  | Stopping the process execution                               |
| SIGTSTP | 18/20/24 | Stop                  | Stop signal from the terminal (Ctrl-Z)                       |
| SIGTTIN | 21/21/26 | Stop                  | Attempting to read from the terminal by a background process |
| SIGTTOU | 22/22/27 | Stop                  | Attempting to write to the terminal by a background process  |

Bash has a keyword `trap` which can be used to catch different signals and provide for certain commands:

```
trap <COMMAND> <SIGNAL>
```

> Under the signal you can use its name (_Signal_ column in the table), or its code (_Code_ column in the table). You can specify several signals, separating their names or codes with a space. <br> **Exceptions:** SIGKILL (9) and SIGSTOP (17/19/23) signals are impossible to catch, so there is no point in specifying them.

```sh
trap "echo Program execution interrupted...; exit" SIGINT

for i in {1..10}
do
	sleep 1
	echo $i
done
```

```
$ bash script.sh
1
2
3
4
^Program execution interrupted...
```

## Debugging scripts

Running the script with the `-x` parameter will show its step-by-step execution, which will be useful for debugging and searching for errors:

```
$ bash -x script.sh
```

## Additional materials

1. 📄 [**Awesome Bash – curated list of delightful Bash scripts and resources** – GitHub](https://github.com/awesome-lists/awesome-bash)
2. 📺 [**Write Your Own Bash Scripts for Automation** – YouTube](https://youtu.be/PPQ8m8xQAs8)
3. 📘 [**Bash cookbook** – C. Albing, 2007](https://theswissbay.ch/pdf/Gentoomen%20Library/Programming/Bash/O%27Reilly%20bash%20CookBook.pdf)
