---
tags: SoftwareEngineering
---

Bash Scripting is a powerfull tool to for the everyday programmer. Be it by creating configuration files or schedueling programs or whatever you want. 
Just like any other programming language, bash scripting follows a set of rules to create program understandable by the computer. 

### Defining variables
```sh
#! /bin/bash

greeting=Hello
name=Tux
echo $greeting $name
```

### Numerical expressions
Numerical expressions can also be calculated and stores this way:
```sh
#! /bin/bash

var=$((3+9))
echo $var
```

But fractions don't work correctly this way. For decimal calculations we can use `bc` command to get the output to a particular number of decimal places. `bc` (Bash Calculator) is a command line calculator that supports calculations up to a certain number of decimal points.

### Read user input
In bash we can take user input usind the `read` command
```sh
read variable_name
```

To promt the user with a custom message, use the -p flag like this: `read -p "Enter your age" variable_name`

### Numeric Comparison and Logical operators

| Operation             | Syntax        |
| --------------------- | ------------- |
| Equality              | num1 -eq num2 |
| Greater than equal to | num1 -ge num2 |
| Greater than          | num1 -gt num2 |
| Less than equal to    | num1 -le num2 |
| Less than             | num1 -lt num2 |
| Not equal to          | num1 -ne num2 |

**Syntax**:
```sh
if [ conditions ]
	then
		commands
fi
```

Example:
```sh
#! /bin/bash

read -p "X: " x
read -p "Y: " y

if [ $x -gt $y]
	then
		echo X is greater than Y
elif [ $x -lt $y]
	then
		echo X is less than Y
elif [ $x -eq $y]
	then
		echo X is equal to Y
fi
```

We can also add AND `-a` and OR `-o` as well.
The bellow statement translates to: if `a` is greater than 40 and `b` is less than 6.
`if [ $a -gt 40 -a $b -lt 6]`

### Looping and skipping
In this example we will iterate 5 times:
```sh
#! /bin/bash

for i in {1..5}
do
	echo $i
done
```

We can also loop through strings as well

```sh
#! /bin/bash

for X in cyan magenta yellow
do
	echo $X
done
```

**While loops** check for a condition and loop until the condition remains `true`. We need to provide counter statemant that increments the counter to control loop execution. 
In the example bellow, `(( i += 1 ))` is the counter statement that increments the value of `i`.

Example:
```sh
#! /bin/bash
i=1
while [[ $i -le 10 ]] ; do
	echo "$i"
	 (( i += 1 ))
done
```

### Arguments for scripts from the command line
It is also possible to five arguments to the script o execution. 
`$@` represents the position of the parameters, starting from one.

```sh
#! /bin/bash

for x in $@
do
	echo "Entered arg is $x"
done
```
