This is CS50 Week 1
Today
- variables and types
- Input and Printing
- Functions, Loops, and Conditionals
- Problem Set 1

## Variables and Types
How would you explain what a variable is in a single sentence?
- containers that allows us to store a value in them
- a variable is a name for a value that can change
- example:
```c
int calls = 3;
// type //name
```
- again, `=` is the assignment operator
- Create an integer named calls that gets the value of 3

Why do we begin all statements of variables with this type? 
Why have data types?
- so we can determine easily what the function of the variable is?
- The letter A for example, corresponds to the number 65
	- 65 = 01000001
```c
// two obvious examples of how data types can change 
char country_code = 65;
int country_code = 65;
// two obvious examples
```

```c
int calls = 4;
calls = calls + 2;
calls = calls - 1;
calls = calls * 2;
calls = calls / 2;
// syntactic sugar

int calls = 4;
calls += 2;
calls -= 1
calls *= 2;
calls /= 2

//next

int calls = 4;
calls = calls + 1;
calls = calls - 2;
calls = calls * 3;
calls = calls / 2;

// "Truncation" is generally what happens if you have float divisions on integers
```

## Inputs and printing
`int calls = get_int("Calls: ");`

`get_int("Calls: ");`
- function call

``` c
int calls = 4;
printf("calls is %\n", calls);

// output: "calls is 4"

```

## Functions Loops and Variables

### While loops
```c
int j = 0:
while (j < 4)
{
	printf("#"); 
	j++;
}
printf("\n");
```
go to through this loop: j=0, j = 1, j = 2 , j = 3
- great for not sure how many times you want to do something
### For Loops
- inverse of the while loops
```c
for (int j= 0; j < 4; j++)
{
	printf("#");
}
printf("\n");
```

- "Is there any difference between teh while loop or the for loop"?
	- while is condition based
	- For is conditional
- "Braces?"
	- just the syntax setup of C
- What can I do to get a whole square of loops?
	- Use another loop to have more than one line?
```c
for (int i = 0; i < 4; i++)
{
	for (int j = 0; j < 4; j++)
	
}
```

## Mario
- let's write a program to print a right-aligned pyramid
```
#
##
###
####

how do we do this breh?
```
- prompt > function > int
 `int height + get_int("height: ")`
- prompt > get_int > int
- abstraction, asking the user for the int, then I can make a function for actually drawing out the pyramids
- i might get implicit declaration
- prototype
	- return value
	- function name
	- parameters/inputs
- took the user input value and put it into the `print_row` function
is it ever useful to put the definition above main?
- usually we don't do that
- C also only likes 1 variable with 1 name at a time

can't have two varibels with same name

print_row is up, print_row is towards the bottom of the srouce code
- it's on a separate function with the same name

- there's probably an assocaiton with the height and the row length
For loops
- so long as you have declared the variable within the loop
	- you have access to  it
	- variable `i` for example , is declared and initialized in the loop
		- access to it within the culry braces
	- this concept is called "scope"
## functions