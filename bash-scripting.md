## 1.0 Introduction to bash scripting
### 1.1 KERNAL 
It is a interface between the hardware and software. It is a program which takes commands from the shell. The shell and kernal together can be refered to as the operating system.
user--->application--->shell--->kernal--->hardware
### 1.2 SHELL 
It is a container which holds information. It acts like an interface between the users and kernal.
Types of shells
* gnome
* kde
* xfce
* sh
* bash - born again shell(sh)
* zsh
* csh and tcsh
* ksh (korn shell)
### 1.3 Finding your shell
Use the below command to know what shell are you running currently
>echo $0
The below command outputs all the available shells in your OS
>cat /etc/shells
To change the shell just type the shell in the terminal

### 1.4 Shell scripting
A program to run a set of instructions or commands all together.
To run a shell script, we need to give exectuable permissions
>chmod 755 script.bash or >chmod a+w script
The above command gives exectuable permissions to all users
If running sensitive scripts appropriate permissions must be given
After the permissions, we can run command with the absolute path or the relative path
>~/scripts/xyz.bash # absolute path
>./xyz.bash # relative path where the single dot specifies the current directory.
The script can also be runned like the following
> bash xyz.bash
### 1.5 Scripting naming convention
Step 1: create a directory for scripts 
Step 2: name the script based on the function
Step 3: Create an extenstion with a shell name so it could be identified if you write different script other than bash
Note: Linux is an extensionless system so a script doesn't necessarily have to have this characteristic in order to work.

### 1.6 Script format
Step 1: Define a shell (#!/bin/bash)
The hash exclamation mark ( #! ) character sequence is referred to as the Shebang. Following it is the path to the interpreter (or program) that should be used to run (or interpret) the rest of the lines in the text file. The shebang must be on the very first line of the file (line 2 won't do, even if the first line is blank). There must also be no spaces before the # or between the ! and the path to the interpreter.
Step 2: use comments to specify a statement (# comments)
Step 3: Define variables at the start
Step 4: commands and statements
```bash
ex:
#!/bin/bash
#Author: ....
#Purpose: ....
#Date: ....
#Modification: .....
```
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 2.0 Variables
### 2.1 A variable is a temporary store for a piece of information. There are two actions we may perform for variables:
a.Setting a value for a variable.
b.Reading the value for a variable.
### 2.2 Two things to note while defining variables
a.When referring to or reading a variable we place a $ sign before the variable name.
b.When setting a variable we leave out the $ sign.
### 2.3 [variable_name]=[command]
>d=date
>c='cal 2020'
To run the variable use dollar sign
>$[variable_name]
### 2.4 Some special variables
		$0 - The name of the Bash script.
		$1 - $9 - The first 9 arguments to the Bash script. (As mentioned above.)
		$# - How many arguments were passed to the Bash script.
		$@ - All the arguments supplied to the Bash script.
		$? - The exit status of the most recently run process.
		$$ - The process ID of the current script.
		$USER - The username of the user running the script.
		$HOSTNAME - The hostname of the machine the script is running on.
		$SECONDS - The number of seconds since the script was started.
		$RANDOM - Returns a different random number each time is it referred to.
		$LINENO - Returns the current line number in the Bash script
		Note: If you type the command env on the command line you will see a listing of other variables which you may also refer to.
### 2.5 The variables can only store a single word. To store complex values use single or double quotes.
		a.Single quotes will treat every character literally.
		b.Double quotes will allow you to do substitution (that is include variables within the setting of the value).	
		ex:
			var='cheez'
			newvar='say $var"
			echo $newvar
			output: say $var
			
			newvar="say $var"
			echo $newvar
			output: say cheez
	2.6 Command substitution allows us to take the output of a command or program (what would normally be printed to the screen) and save it as the value of a  
		variable. To do this we place it within brackets, preceded by a $ sign.
		ex:
			>var=$( ls / | wc -l )
	2.7 Scope of the variables
		The scope of a variable can be limited to its own script but with the help of the 'export' we can make the variable available in other scripts
		ex:
			var='date'
			export var
			./anotherscript.bash
		Note: Exporting variables is a one way process. The original process may pass variables over to the new process but anything that process does with the copy of the variables has no impact on the original variables.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

3.0 User Input
	3.1 To take input from the user we use read command
		>read var
		Varies options can be provided such as -p which allows you to specify a prompt and -s which makes the input silent. This can make it easy to ask for a username and password combination like the example below:
		ex:
			read -p 'Username: ' uservar
			read -ps 'password: ' passwdvar
		Note: If you want to run a command which is stored in the variable using echo we use the symbol (`) to enclose it. This is present with the tilde key.
			ex: >d=`date`
				>echo Todays date is $d
	3.2 Reading from STDIN
		To take input from a file use /dev/stdin
		ex:
			cat /dev/stdin
			terminal: >cat inputfile | ./script.bash
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

4.0 Arithmetic
	4.1 let is a builtin function of Bash that allows us to do simple arithmetic.
		let <arithmetic expression>
		let var=5+4
		let var++
		let "var = 5 + 9" #14
	4.2 expr is similar to let except instead of saving the result to a variable it instead prints the answer.
		>expr 5 + 9
		>expr "5 + 9" # 5 + 4
		Note: If the spaces between the numbers are not put then it will output 5+4 (same case for double quotes)
	4.3 Double Parentheses
		$(( expression ))
		>a=$(( 4 + 5 )) #9
		>a=$(( 4 + 5 )) #9
		Variables can be included with the $ sign or not
		>b=$(( a + 3 ))
		>b=$(( $a + 3 ))
		>(( b++ ))
	4.4 Length of a Variable
		${#variable}
		ex:
			b=4953
			echo ${#b} # 4
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

5.0 Conditional Statements
	5.1 Basic If statement
		The set of commands under if block will be executed if the condition is true.
		Syntax:
				if [ <some test> ]
				then
					<commands>
				fi
	5.2 Test operators
		
			! EXPRESSION			The EXPRESSION is false.
			-n STRING				The length of STRING is greater than zero.
			-z STRING				The lengh of STRING is zero (ie it is empty).
			STRING1 = STRING2		STRING1 is equal to STRING2
			STRING1 != STRING2		STRING1 is not equal to STRING2
			INTEGER1 -eq INTEGER2	INTEGER1 is numerically equal to INTEGER2
			INTEGER1 -gt INTEGER2	INTEGER1 is numerically greater than INTEGER2
			INTEGER1 -lt INTEGER2	INTEGER1 is numerically less than INTEGER2
			-d FILE					FILE exists and is a directory.
			-e FILE					FILE exists.
			-r FILE					FILE exists and the read permission is granted.
			-s FILE					FILE exists and it's size is greater than zero (ie. it is not empty).
			-w FILE					FILE exists and the write permission is granted.
			-x FILE					FILE exists and the execute permission is granted.
		Note:
			a) = is slightly different to -eq. [ 001 = 1 ] will return false as = does a string comparison (ie. character for character the same) whereas -eq doesa  numerical comparison meaning [ 001 -eq 1 ] will return true.
			b) When we refer to FILE above we are actually meaning a path. Remember that a path may be absolute or relative and may refer to a file or a directory.
		Remember: There is no boolean in bash.  0 means TRUE (or success). 1 = FALSE (or failure).
	5.3 Nested If statements
		An if statement an be nested in an if statement. Indenting makes it easier to read especially if using nested.
		Syntax:
				if [ <some test1> ]
				then
					if [ <some test2> ]
					then
						<commands>
					fi
				fi
	5.4 If Else statement
		If the test condition is not satisfied then an alternate set of commands will be executed.
		Syntax:
				if [ <some test> ]
				then
					<commands>
				else
					<other commands>
				fi
	5.5 If Elif Else
		When you have series of conditions to execute then this condtional statement can be used
		Syntax:
				if [ <some test> ]
				then
					<commands>
				elif [ <some test> ]
				then
					<different commands>
				else
					<other commands>
				fi
	5.6 Boolean Operations
		If a multiple set of conditions are used, then we can use the following
			and - &&
			or - ||
		ex:
			if [ -r $1 ] && [ -s $1 ]
			then
				echo This file is useful.
			fi
	5.7 Case Statements
		Syntax:
				case $<variable> in
					<pattern 1>)
						<commands>
						;;
					<pattern 2>)
						<other commands>
					;;
					*)
						<commands excecuted if the input is incorrect or doesnt match>
				esac
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

6.0 Looping Statements
	6.1 While Loop
		The loop runs until the condition becomes false
		Syntax:
				while [ <some test> ]
				do
					<commands>
				done
	6.2 Until Loop
		This is similar to while loop but the difference is that it will execute the commands within it until the test becomes true.
		Syntax:
				until [ <some test> ]
				do
					<commands>
				done
		Ex:
			counter=1
			until [ $counter -gt 10 ]
			do
				echo $counter
				((counter++))
			done
	6.3 For Loops
		Syntax:
				for var in <list>
				do
					<commands>
				done
		The for loop will take each item in the list (in order, one after the other), assign that item as the value of the variable var, execute the commands between do and done then go back to the top, grab the next item in the list and repeat over.
		The list is defined as a series of strings, separated by spaces.
		Ex:
			names='Stan Kyle Cartman'
			for name in $names
			do
				echo $name
			done
		output: Stan 
				Kyle 
				Cartman
		It is also possible to specify a value to increase or decrease by each time. You do this by adding another two dots ( .. ) and the value to step by.
		Ex:
			for value in {10..0..2}
			do
				echo $value
			done
		output:
				10
				8
				6
				4
				2
				0
	6.4 Break
		The break statement tells Bash to leave the loop straight away. It may be that there is a normal situation that should cause the loop to end but there are also exceptional situations in which it should end as well.
	6.5 Continue
		The continue statement tells Bash to stop running through this iteration of the loop and begin the next iteration.
	6.6 Select
		The select mechanism allows you to create a simple menu system.
		Syntax:
				select var in <list>
				do
					<commands>
				done
		Ex:
			names='Kyle Cartman Stan Quit'
			PS3='Select character: '
			select name in $names
			do
				if [ $name == 'Quit' ]
				then
					break
				fi
			echo Hello $name
			done
		Note: You may change the system variable PS3 to change the prompt that is displayed.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

7.0 Functions
	7.1 Syntax: 
		function_name () {
			<commands>
		}
	Ex:
		print_something () {
			echo Hello I am a function
		}
		print_something
		print_something
	7.2 Passing parameters
	Ex:
		print_something () {
			echo Hello $1
		}
		print_something Mars
		print_something Jupiter
	7.3 return values
	Ex:
		print_something () {
			echo Hello $1
			return 5
		}
		print_something Mars
		print_something Jupiter
		echo The previous function has a return value of $?
	Ex:
		lines_in_file () {
			cat $1 | wc -l
		}
		num_lines=$( lines_in_file $1 )
		echo The file $1 has $num_lines lines in it.
	7.4 Variable Scope
		By default a variable is global. One can make the variable local to the function
		local var_name=<var_value>
----------------------------------------------------------------------------------------------------------------------------------------------------------------------		
