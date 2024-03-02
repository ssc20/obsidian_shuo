---
tags:
  - cs50
  - computerscience
---

---

# Week 4
#cs50/memory
## Notes

to solve the issue of indexing a value in an address, and if you want to only change the value of one variable of multiple that may be assigned to that address' value
(i.e. 0x123 (f[0], t[0]))

- we require 2 functions
	- `malloc`
	- `free`
	- there are more but will be discussed upon learning about data structures
- these are included in `stdlib.h`
- previously got in trouble by just copying addresses, let's avoid this!
- ![[CleanShot 2024-03-01 at 19.05.57@2x.png]]
- `char *t = malloc()`
	- whether mac, windows, linux, this is:
	- Please find me memory in the computer that i can use for my own purposes
	- let's think of doing `malloc(strlen(s))`, but this might not be the best
	- `malloc(strlen(s) + 1);` to include the `null` value that might be included
- ![[CleanShot 2024-03-01 at 19.08.20@2x.png]]
- within the context of design, this seems a bit buggy
	- it is inefficient to keep asking if `i < strlen(s)
	- could improve by giving a variable to `n = strlen(s); i < n;`
		- but *this* is ALSO buggy; this does not fully copy `s` into `t`
		- we are still forgetting however, about the `\0`
	- you could do:
		- `for (int i = 0, n = strlen(s); i <= n; i++`
	- or (but not recommended because of inefficiencies/stupid)
		- `t[3] = '\0';
		- `t[strlen(s)] = '\0';`
- we should now see disparate `s` and `t`
	- ![[CleanShot 2024-03-01 at 19.12.31@2x.png]]
	- 'disparate'
		> essentially different in kind; not allowing comparison 
- s is now still lowercase, but T is now capitalized
	- if what I had before where `s` is pointing at a chunk of memory, let's have `t` point at a different chunk
	- ![[CleanShot 2024-03-01 at 19.18.13@2x.png]]
	- allowing us to copy all values, including `\0`
		- `malloc` takes 1 argument, the number of bytes to find for you
		- what value is `malloc` returning?
			- it's returning a chunk of memory conceptually
			- `malloc` is returning the address of *that* chunk of memory, not the last address
				- i.e. the new chunk of memory designated by `malloc`
	- here's a difference of string
		- the new chunk of memory (without `malloc` ) is not magically terminated with `null` 
		- this had to be done with the `for` loop
		- `malloc` will keep track of how big the chunks of memory are
		- although it's only returning the address of the first byte of memory, it will know that it used 4 bytes here and 4 bytes there
			- avoid overlapping addresses to avoid corruption
			- you must also remember or figure out how many bytes by putting the `null` character there yourself
		- let's harden the code a little bit more as follows, we *can* do this a bit better
	- if something goes wrong and i'm out of memory (old computer, something 3 billion chars long):
```c
if (t == NULL)
{
	return 1;
}
```
- *if `t` equals a special symbol called `NULL` (2 L's):*
	- return 1, 2... anything that will abort the program early (any value != 0)
- all this time, when using `get_string`, if you read the manual, with a crazy long string...
	- if `get_string == NULL` exit the program immediately 
- there's 1 other improvement we can make here; (most of prev. work is error checking to not treat `s` as invalid when it isn't)
- it turns out we don't need to design a loop to copy a string, no need to reinvent the wheel!
	- ![[CleanShot 2024-03-01 at 19.30.22@2x.png]]
	- Instead of above, we can use a fun function called `strcopy` 
		- takes the destination as it's first argument, and the source as it's second
		- `strcpy(t, s);`
		- `free` is the opposite to `malloc`
			- when you're done, you're supposed to give it back to windows/mac/linux
			- have you ever used a computer for days on end, and maybe it's getting slower and slower?
			- generally really big files consume lots & lots of memory
- if humans who wrote that software be it Photoshop or something else, wrote *buggy code* and kept using `malloc` asking for more & more function:
	- they never call the opposite function, `free`
- computer might even crash! bad things happen when you run out of memory
- it's actually on me in C to manage memory myself
	- after calling `malloc`, you better make sure you `free` it after finishing'
	- make sure you free *after* finishing your function
	- `free(t)`;
	- ![[CleanShot 2024-03-01 at 21.04.58@2x.png]]
	- there is a slight asymmetry that exists:
		- even through `get_string` is still allocating memory, it still uses `malloc`
			- you should not and do not free memory that `get_string` returns to you
			- this is because functions from the `cs50.h` library manage memory for you
			- this training wheel is over after this week!
- `NULL`: it is just an address, and it is the address `-`
	- `NUL` is the terminating character in a string
	- `NULL` means this is the address `0`
	- the address `0` is a special sentinel value to signify error
		- this is 1 byte out of a billion to have a special symbol to indicate something has gone wrong
- introducing a tool that makes things easier as you make bugs
	- invariably, without having to raise your hand or using the duck...
- INTRO
- 'valgrind' #valgrind #debugging

## valgrind
- creating a program called `memory.c` that is buggy, but not **obviously** buggy
- we need `<stdlib.h>` for `malloc`
- declare main without command line arguments
- instead of declaring an `int x`, let's make `int *x = malloc(4)`
	- (give me the address of an integer `x` )
	- let's be a bit crazy in this by manually allocating memory myself
	- with `malloc` i can take control of this access
	- *this is a bit stupid; shouldn't assume an int is always 4 bytes*
		- we can use `malloc(sizeof(int));`
		- so you don't need to assume `4`, you will always get the correct number of bytes
		- we can even try this...
		- `int x[4]` OR ACTUALLY
		- `int *x = malloc(3 * sizeof(int));`
			- this effectively makes `*x` an array; this is kind of deliberate because if an array = contiguous memory, and `malloc` returns to you a chunk of contiguous memory:
				- you can treat what comes back from `malloc` as an array
				- we're treating chunks of allocatable memory as arrays of "chars" (like with strings)
		- let's do something arbitrary here:
```c
int main(void)
{
	int *x = malloc(3 * size of(int));
	x[1] = 72;
	x[2] = 73;
	x[3] = 33;
	// this spells out "hi" in ASCII
}
```
- this program actually runs! 
	- ?  but what is the bug that is preventing any kind of output?
	- the computer doesn't know when the array technically ends
	- given that we have told `malloc` to allocate 3 int-sized chunks, we should actually be starting from:
```c
x[0] = 72;
x[1] = 73;
x[2] = 33;
```
- there's a 2nd more subtle bug:
	- (it's not that the computer doesn't know where the integers end)
	- ~ I didn't call `free`!
		- anytime i call `malloc` , i must call `free`
		- none of these bugs are obvious, and deploy this, be none the wiser
- *valgrind* can help us with this!
	- `valgrind ./memory`
	- the output will look crazy, but *this* is *useful*
		- notice at the top of this, it's indicated what version of *valgrind* you're using and what command we ran
		- `invalid write of size 4`
		- ![[CleanShot 2024-03-01 at 21.43.47@2x.png]]
		- "writing" means changing information, like setting a value or assigning it value
			- this is useful here `memory.c:9`
				- colon 9: this means line 
				- 


Mailbox example 
- white one is labelled `p`
- black one is labelled `n` (this is Carter's)
- metaphorically, these mailboxes are supposed to represent what's going on
- even though it's very weird syntax with & and \* it's still somewhat simple 

`string s = "HI!"`
- this is actually $H \ I \ ! \ 0$
- if `H = 0x123 I = 0x124 ! = 0x125 \0 = 0x126`
	- whether it's decimal or hexadecimal... all these numbers differ by only 1
- when I declare `s` to be a string... what's going on in the actual pointer file?
- ![[CleanShot 2024-02-26 at 22.26.00.png]]
- so, to guess, intuitively, the array of chars... what should we put in the value of `s`?
	- I would make a pointer  for `0x123`
	- ![[CleanShot 2024-02-26 at 22.26.39.png]]
- if `s` is a pointer to the location of the start of the string, then logically, 
	- someone needs to keep track of the end of the string 
	- who keeps track of that? the string itself, stopping at the `null & \0` character
	- `string` is a pointer to the start of an array!
	- you can use a `for` or a `while` loop to 
	- ~ you can treat `s` as an arrow to point to the start of an array

- ? Q:
	- have pointers gotten larger?
		- back in my day, it was 2 GB of memory/RAM...
		- you can count as high as 4 billion values
			- numbers positive and negative results in 2 billion
			- the integers & pointers were only 4 bytes long, which is 32 bits long.
		- & so no matter what, even if with more memory, you dealt with the restriction of 32 bit architecture
		- modern systems now support 64 bits, which allow you to count crazy high

let's now translate this to a bit of code now! 
- ![[CleanShot 2024-02-26 at 22.33.43.png]]
- while not as simple, we can still see the memory reference location
	- let's try finding the very first character of string "s" 
	- `printf("%p\n, &s[0];`
	- remember that & indicates the desire to find the location/address of the variable
	- ![[CleanShot 2024-02-26 at 22.35.57.png]]
	- you can see that all the characters are all just 1 byte away
		- the numbers themselves aren't useful actual information, but it allows us to see what's going on underneath the hood
- let's revert the code so that it prints out hi again
- `string s = "HI!";`
	- let's reveal that all this time...
	- ! STRING IS ACTUALLY `char *s = "HI!"`
		- we invented the datatype `string` to help learn
			- string is an aray of characters
			- they never thought to structure it like this when developing C
- what is a string?
	- it is an address of its 1st character
	- when I claim that string never really existed, the data of a string is best expressed as `char *s`
	- * means that s is the address of a char, and the first address of a char in a string
- let's change `string` to `char *s` and remove our `include <cs50.h>`
- ![[CleanShot 2024-02-26 at 22.39.34.png]]
	- this is that raw, native C code without any training wheels/scaffolding
	- just basic building blocks and primitives

- why don't we use the & symbol?
	- the & symbol tells you what the address of a variable...
	- it might be more consistent to do `char *s = &"HI!";
		- but this looks worse syntactically, but is a reasonable instinct, but the double quotes does that for you
	- the C compiler, `clang` understands though intuitively to, when seeing double quotes around a sequence of characters:
		- it wants to put the address of that first char in the variable for you
- but when had a variable with `n` that we created, we have to distinguish `n` from the address with &, but the `""` takes care of that for you

#### How did we invent 'string'?
- remember when we tinkered with structures. custom data types, to represent a person?
- recall we had a structure of a name & a number representing a person
- we had the keyword "typedef"
	- we used this to represent a person and a number to represent a person
- typedef is the tool to do that!
```c
typedef struct
{
	string name;
	string number;
} person;
```
- let's say you can't/don't want to keep call integers `int`
- `typedef int integer;`
	- read this from right to left,
	- give me a datatype `integer` of type `int`
	- more commonly, you might see something like this
		- C has no data type for a bytte
		- no built in obvious way to rerpesent 8 bits
		- you can use `uint8_t` to do this though...
			- commit this to memory maybe?
	- `typedef uint8_t BYTE;`
		- this is an unsigned integer that uses 8 bits
		- you *will* see this before long
- what else is in `cs50.h`?
	- `typedef char *string`
		- so you never had to think about what a string was up until now!
- another pair of examples perhaps?
	- once everything is addressable with pointers, like using numeric addrsess to represent where thigns are in memory
		- you can actually do something using them called "pointer arithmetic"
	- we the programmers generally don't care what thes pecific values are, but we do care that they do exist
	- if they do exist, we can do some arithmetic!
		- add 1  to go to the next byte
		- add 2 to go to the next byte
		- add 3 to go to the next byte
		- and so forth...
	- ! pointer arithmetic: doing math on addresses
- let's head back to VS Code and see some more of this in action:
```



- Treating cells like squares/pixels instead of the rectangles
- cs50.ly/art
- instead of 0s and 1s, let's use colors
RGB: red, green blue, some combinations give you the colors that you want
- standard ways of calculating this
	- ofc it's just bits (and numbers, 72, 73, etc.)
	- there is a different notation
![[Pasted image 20240224221411.png]]
- USING photoshop to demonstrate this new way of describing colors
- we've picked the color 'black'
	- 0 red, 0 green, 0 blue
	- hash symbol, then various digits
	- if you pick 'white', you'll see a lot of green, red, blue
		- \#FFFFFF
		- (groups of 2, `FF` for 255 red, `FF` for 255 green, `FF` for 255 blue)
		- Red = `FF0000`
		- Green = `00FF00`
		- Blue = `0000FF`
	- `FF` represents 255... seemingly
	- in the real world, using decimal instead of binary
		- in the color world, we take to using hexadecimal, where after:
		- `0 1 2 3 4 5 6 7 8 9 0`
			- we will go:
			- `0 1 2 3 4 5 6 7 8 9 0 A B C D E F`
			- (16 different characters, hence 'hexadecimal') or 'base-16' 
				- case doesn't matter 
			- there *are* other systems but this is the last we will discuss in any kind of extensive detail
- how might we represent decimal numbers in hexadecimal?
	- ![[Pasted image 20240224221919.png]]
	- if we do the math, we will be given the 1s and 16s place ($16^0$) & ($16^1$) 
	- $0 \ 0$, for example, would be $(0\times 16 + 0 \times1)$
	- the numbers stay the same as decimal all the way to '10'
		- 10 would be `0A`, 11 would be `0B`, and so on
		- counting to 16 would entail `10`
		- the last digit in hexadecimal would be `FF`, and the math gets annoying
			- $((16 \times 15)  + (1 \times 15)) = 255$
		- this is not math to do frequently, but just to demonstrate the math in different bases
-  hexadecimal is just *convenient*:
	- with 16 different digits in your alphabet, 0 - F, how many different values do we need to represent 16 different values?
		- 4 different values! -- that's $2^4$ or $2\times 2 \times 2 \times 2 = 16$
		- 4 bits represented by a single digit, it's convenient in practice for computer scientists & programmers
		- while `F` might represent `1111` (16), `1111` or 16 is still not a full byte (*there's only 4 bits here*)
			- (a full byte is 8 bits)
			- no one counts in units of 4, it's always in units of 16, 32, 64
		- because hexadecimal is always in units of 4, we can just use $2$; 8 bits = 1 byte, which **is** a common unit of measure!
		- this is why Photoshop and color programs, web dev, use hexadecimal digits to represent single bytes
- it's not a big deal per se, but it's just convenience
- test

Questions?
- nope

- where else might we see this in the computer world?
	- when we talk about a computer's memory, we can think of each square as having an individual location
- we can number all of the bytes in a computer
- computers use numbers indeed to represent numbers, typically in hexadecimal notation
	- ![[Pasted image 20240224224626.png]]
- #cs50/hexadecimal/0x
- Now, to rewrite this in the hexadecimal equivalent:
	- ![[Pasted image 20240224224645.png]]
- we are now introduced to `0x` syntax
	- this doesn't do anything mathematically
	- although we don't have room for 255 bytes, we do start to see patterns....
	- ![[Pasted image 20240225210006.png]]
- make a code file called `addresses.c`
	- when you write the example code, `int n = 50`, the value of $n$ is stored somewhere in the computer's memory
		- e.g. `0x123`
	- the variable $n$ lives at an *address*
	- something in C gives you access to the location of variables in your computer's memory
	- at what address is this variable?
	-  using a single & (what address is this variable) and the `*` is the 'deference operator'
		- take an address, and go to it....
	- ![[Pasted image 20240225210736.png]]
	- instead of printing out a value, let's print an address
		- use the 2 new capabilities where & gets you the address of `n`
		- change `printf("%i\n", n)` to `printf("%p\n, &n);`
			- p = address
			- notice the added `&` to distinguish that we are providing the address of `n` into the `printf()` function
			- this is a large result number, but we do get something!
			- ![[Pasted image 20240225211011.png]]
		- this `%p` passed into printf as a 'format code' that leverages the fact that C supports what is known as pointers
		- pointers is really just an address, address of variable that you can store in another variable called a pointer
	- let's still declare a variable `n` but create a pointer!
	- 
```c
int n = 50;
int *p = &n;
```
- `int *p = &n;` means "provide me a variable called 'p', inside of which I can store the address of an integer"
	- what address to put in there? well, we can use `&` to say, give me the address of a variable in itself.
	- let's play around in vscode with 2 variables:

```c
#include <stdio.h>
{
	int n = 50;
	int *p = &n;
	printf("%p\n", &n);
}
```





# Week 3
## Notes
### Algorithms
- taking a break from syntax
- we have a lot of vocabulary now!
	- might not grasp functionality
- but instead, let's think about how to think 'algorithmically'
	- quantize real world problems so we can map puzzle pieces from week 0, with this new syntax to solve those problems
- recall the time to solve v. size of problem chart
	- how many units of measure -- whatever you're using
	- the first algorithm: going 1 page at a time
		- 1:1 slope!
	- the second algorithm, we started doing two pages at a time
		- might skip a page;
			- faster, but not yet 2/1
	- the third algorithm; logarithmic curve
		- even if you doubled the size of the phone book
			- it was just one more page turn
			- not another 500 or 1000
- think back to how we want to divide and conquer the problem
	- if i were to take attendance on stage, we could do it old school... one step at a time
		- or double by counting 2 at a time
- everyone stand up and think of the number 1
	- second step, ultimate theoretically: pair off with someone standing
		- add their number to yours
		- remember the sum
	- most likely, everyone is thinking of the number two
		- unless you're an odd one out
	- next step is that one of you in the pairs should sit down
	- notice we're about to induce a loop so the rest is on you!
		- if still standing, go back to step 2
		- keep going if still standing!
	- theoretically, there's only 1 person standing left
		- asking each other their number
		- 46
		- 16
		- 48
		- (sitting down)
		- 32
		- 43
		- 22
		- 16
	- if there were 1000, then 500, then 250...
		- why? 
		- each step of the algorithm, half was sitting down each time
	- the total count is 231
	- everyone paired off!
	- click enter, do all of the remaining addition, should have the total count of people in the room
	- unlike 1st or 2nd algo, the 3rd, should've been fundamentally faster
		- no matter how many people
		- 
- as a backup, the actual number. of people in the room
	- (i have no idea, but apparently it was really off)
- same idea of "dividing and conquering"
- halving, halving, and that's called logarithms
- doing something n times, or even n / 2 times, where n is the number of ppl in the room
- in contact books like on iOS/Android
	- the divide/conquer, logarithimc search, is what is being used
	- these ideas are everywhere, and hopefully you find the one person you're looking for
		- the person who should have the count of "everyone"
		- should 'coalesce' in the figure of the last person standing
- the idea of data structures, arrays
	- key definition: a collection
	- strings are just an example of char char char
		- not necessarily 
	- list in concept
	- consecutive aka contiguous
		- an array is important in C, is because it's contiguous
		- the bytes are back to back to back somewhere in the computer's memory
	- we can actually start to problem solve
		- pair this down to just an abstract size of 7
- suppose there are these numbers
	- {1, 5, 10 , 20, 100, 500}
		- the catch with C: find the number 50....
			- every number found it obviously, since we have a "birds-eye" view of the memory 
			- the computer does NOT; it can only look at each value 1 at a time
			- you have to shield your eye and look at 1 number at a time L-R in order to find if the 50 is actually there
		- analogous to a set of gym lockers
		- ![[CleanShot 2024-01-24 at 20.48.31.png]]
	- so we have some terminology, I've labelled the locker doors like so:
		- ![[CleanShot 2024-01-24 at 20.49.00.png]]
	- although there are 7 lockers, I'm only counting to 6...
		- again, this is equiv to `n-1`
	- suppose we are to use these lockers, let's find an actual number in these doors
		- constantly searching for information...
	- let's abstract this!
		- the black box model; input > algorithm > output
	- the input is an array of 7 numbers
		- the problem, find me the data if it's there....
		- the algorithm is where we need to do our thinking
			- you can generalize this to web pages , contacts, or any other type of info that is in a computers database
- Example going hard
	- Sam is Applied Math
	- Louis is doing econ with stats
	- David was in Matthews too (matthews south)
	- Behind you are these seven lockers
		- find us the number 50
		- like you to then explain how you go about finding it...
	- *opened the last locker*
		- $10 bill
	- *next locker left*
		- $100 bill
	- eventually find the $50
	- if you could explain, what was your algorithm
		- keep going till you find it...
	- Carter is going to shuffle the numbers carefully...
- could Sam have done better?
	- that took 5 steps to find the $50 bill
	- if you got lucky, you could've found it on the 1st try
	- taken out all the dollar bills, sorted them, put them back in, then do the divide and conquer approach
		- valid algo, but all the work seems to be inefficient
		- this might be a reasonable solution if she wants to search again and again
		- assign a cost up front to prevent this.
	- go from left to right...
		- show everyone the numbers to prove there's no funny business going on here
		- {20, 500, 10, 5, 100, 1, 50}
		- had to literally go into the last locker on the right..
		- the performance of that algorithm, really depends on where the number ends up being
		- it really just boils down to luck, unfortunately
	- (i'm guessing 1 left, 1 right?, or some combination of that?)
		- Louis has the memory, but let's try and assume no memory
	- Louis is starting from the middle
		- since there's 4 numbers, i know some are bigger, i'll go to the right
		- relative to these three, 
			- 50 has to be between 20 and 100
	- Louis' instinct, was remember the numbers and deduce the number...
		- (trial difficulty)
		- Louie's instinct, to find it, to index it, is an ctual technique
		- where you actually take ifnormation and go to where you want
			- a concept called hashtables
		- let's try and formalize the algorithms that the volunteers intuitively came up with
	- 
- Linear search
	- #linearsearch #pseudocode
	- stepping in a line, no matter which direction you're going
	- let's try and formalize; translate it into pseudo code
```pseudocode

for each door from left to right
	if 50 is behind door
		return true
return false

// notice that the return value is outside of the 'for' loop

// we want to avoid concluding within the first iteration, that the 50 behind the door is not present at all (returning false prematurely)

```
- notice that only towards the bottom of the algorithm, 
	- if she opened the door, found it to be the wrong door...
	- if you don't previously return true, make sure you try again until you do

```c
For i from 0 n-1 // say n is 7, through 6 doors
	if 50 is behind doors[i] // if door is the i'th door, and it has the 50, it's true
		Return true
Return false // otherwise, after options exhausted, where door is the i'th door, and doesn't have the 50, return false entirely
// notice that this psuedo code borrows from C
```

- Binary Search
	- #binarysearch

```
If no doors left
	Return false
If 50 is behind middle door
	Return true
Else if 50 < middle door
	Search left half
Else if 50 > middle door
	search right half
// note the 4 possibilities, this should be the question you ask first, since there *could* be no doors left
```
- we can make this more technical
```
If no doors left
	Return false
If 50 is behind doors[middle]
	Return true
Else if 50 < doors[middle]
	Search doors[0] through doors[middle - 1]
// don't waste time serching the middle door we already looked at
Else if 50 > doors[middle]
	Search doors[middle + 1] through doors[n - 1]
// not n because we start counting from 0
```
- we can try the more technical pseudocode, since it could be translated verbatim to C code
- only headache is rounding issues, 
	- because if you divide int by fraction, you work through what's going to be truncated when rounded down or up as a solution
- let's consider bit more formally a question that we'll come back to the future
	- what is the running time of these algorithms = measuring efficiency of these algorithms
- going back to the picture, where representitive of the phone book
	- bug aside, the attendence counting algorithm,
		- this green line theoretically represents the time for us to calculate that
	- describe things mathematically at a time
		- 2 people or 2 pages at a time
- n = 1 page a ta time
	- n / 2 = 2 pages at a time
- $log_{2} n$
	- log base 2 of n
	- what is the valuetaking n and dividing it by 2 again and again and again and again
		- until left with 1 person/value standing
- being this precise, it's not that intellectually interesting
- generally a computer scientist is asked the efficiency of your algorithm
	- how good or bad?
		- will talk about it being on some order of steps
		- just wave your hand at it
		- broad strokes: all we care about is certain numbers
	- when computer scientists, they thowr away efficiencies
		- throw away constant factors
		- that might be dividing here or a base here
	- BIG O NOTATION
		- #big_o_notation
		- O(n)
		- O(n)
		- O(log2n)
			- this actually becomes O(logn)
	- doesn't matter again, fundamentally what the base/factors are
- zoom out,
	- ![[CleanShot 2024-01-24 at 21.10.51.png]]
	- the algorithms become closer and closer
	- once n is large enough, they are practically the same algorithm
	- the fundamentally different shape of log though, is very appealing
- this is why constant terms are thrown away (don't change the picture much)
- algorithms can be described in the order of these running times
	- n is just the number of values (what you're doing in code)
	- (n times n - 1)
		- say everybody wants to shake everybody's hand, that's how you calculate it
		- toss away the 1
- O(n^2) - quadratic
	- this means N people doing N things
	- if we were to ask you to shake everyone's hand in the room
	- how many handshakes?
		- n of you, and you've got to shake everyone n's hand...
		- 
- O(n log n) (other fancier terms but not for now)
- O(n) - linear
	- this will take linear time
	- left to right: {1, 2, 3, 4}
	- n pages, it'll take me n steps
		- will always take me n steps, going 1 by 1 from the start
	- we use this to consider the "worst case"
- O(log n) - logarithmic
	- this is our example of binary search
	- takes fewer and fewer steps to run even in the worst case
- O(1) - constant
	- this doesn't actually mean 1 step
		- but it does refer to a constant number of steps taken total
		- when everyone stood up
			- it always is "everyone" stands up, so 1 step taken total
- we have this capital Greek $\Omega$ 
	- exact same idea of best case
	- ![[CleanShot 2024-01-24 at 21.21.03.png]]
	- this will take as few as...
	- how many steps might it take in the best case? (guessing lockers)
		- linear search: just 1 step
		- binary search: 1 step
		- $\Omega$ 1
		- $\Omega$ 1
		- what about attendance?
			- i have to point at eeryone...
			- the best and worst case is that it takes n steps
- these cases where best = worst
	- #theta
	- Theta notation
	- $\theta$ 
		- same number of steps as Big O and Omega analysis
- you'll go more into these ideas with computer science
	- #algorithmefficiency
	- $O$ upper bound
	- $\Omega$: lower bound
- with that being said, let's see how we might translate this to actual code
	- something that makes sense
	- not just new syntax, but applications of these concepts
- lets go make search.c
	- ![[CleanShot 2024-01-24 at 21.32.54.png]]
	- "correct" implementation of the linear search
	- `return 0` for main indicates success
		- 0 is false, 1 is true, so it's counter-intuitive
		- the program works, 1 possiblity: 0
			- it doesn't, and there's an infinite number of that.. so
		- when you return 0, the program will effectively terminate
	- `return 1` is pedantic
		- don't care what the exit status of this in the program
		- intro'ing the idea of returning 0, i should also introduce the idea of introducing a return value to indicate failure
	- testing, students might appreciate this more so that ppl understand what's better under the hood
- for strings though....
	- ![[CleanShot 2024-01-24 at 21.38.42.png]]
	- -   this doesn't seem to work
	- if you want to compare strings in C, you don't do it like you did integers
	- you actually need another technique altogether
	- `string.h` is needed
		- library that includes:
			- `strlen`
			- `strcmp` - string compare
				- will tell us if 2 strings are the same or not
	- let me go back to my code now and see what I'll do differently
		- ![[CleanShot 2024-01-24 at 21.40.56.png]]
		- now it works correctly!
		- why might it be valuable for `strcmp` to return 0
			- when comparing 2 strings...
				- it's not just that they're equal...
				- like us sorting information, it's usually useful to know if this string equals another
					- but also alphabetically, if it comes before or after?
				- == can only give a true or a false
					- so this might not be that helpful
				- strings are compared based on ASCII-betical order
					- comparing (65) against another letter's integer value
			- not doing any sorting, it's immaterial
				- but strcmp again returns 0
			- using == to compare strings[i] and s
				- not comparing in the way you might have thought..
			- == does something different in python or java
		- let's put this into play with some actual additional code
- implemented this idea of the phonebook...
	- ![[CleanShot 2024-01-24 at 21.48.01.png]]
	- does any aspect of this phonebook rub you the wrongway?
		- it's too much! but why?
		- separating the names and the numbers
			- it looks a little bit weird
			- technical term: code smell (already know this)
		- if i add fourth name, fifth name... number.. it's probably going to go out of sync?
			- i shouldn't decouple names from numbers in this instance
		- when you perceive this, there's a chance to improve!
		- we need another tool:
- Data structures
	- arrays are our first example of data structures
	- little bit of new syntax:
		- will allows us to create own data structures, own types of variables
		- largely using syntax we've used thus far
- to represent a person in our phonebook
	- wouldn't it be nice if C had a datatype called person?
	- i could create an array containing people in my phone book
	- maybe people have a name and an umber
		- `person people[]`
		- `string name; string number;`
We can do this
```c
typedef struct
{
	string name;
	string number;
} 
person;
```
put all of the variables you want to associate with this datatype
- create a new data type called person
	- when C was created alongside the ints and the floats and so forth
how can i actually use this?
	- you can go back into your code
- a structure can be a vairable of toehr variabels
- typedata means define the following type, invent the type for me
- new datatype despite not being implemented decades ago, natively in C
Can improve it as follows
- it's going to look a little bit complicated at fist
- but will scale better over time
![[CleanShot 2024-01-25 at 21.03.50.png]]

- we can fulfill the phone book format later with a loop
	- but for now this fulfills our purpose
- we've solved this problem with a brand new data type
	- represents this custom data, where struct clusters all these keywords together
- when it comes to styling your code, style50 will put the name of the datatype on the other side of the closing brace
	- this is fine; this is the "right" way of styling it
```c
typedef struct
{
	string name;
	string number;
} person;
```
Do you have to assing both name and number
- can you get away with one of the name only?
- will result in bogus data
- run the risk of crashing your code, even though you never initialized it

now we have the ability to represent more interesting structures...
- sorting

we assumed someonene had sorted the data for us..
- what if we sort info first, then go find the 50
- how expensive is it to sort?
- how much do microsoft and google spend to keep data sorted?

unsorted > blackbox/algo > output

- everything is arguably unsorted by default, but we want to get it sorted
- somewhere in this blackbox we have to sort this information
	- if we have these integers unsorted
- ![[CleanShot 2024-01-25 at 21.11.37.png]]
- maybe it's time for some brownies?

- could we get 8 more volunteers?
	- if everyone grab a number
	- ![[CleanShot 2024-01-25 at 21.18.50.png]]
- notice this array of volunteers is entirely unsorted
- sort yourselves from smallest to largest
	- looked for the number that was lower. and higher, then found the middle
- what you guys did was a little hard to translate into code
	- let's try taking 2 passes in this
	- start from the same point each time
- we can approach sorting in a couple of different ways
	- needs to be methodical
		- pseudocode > code
		- quantize everything step by step
		- espeically not 8 people, 80 people, 800 people
	- probably would have taken forever
		- no doors in front of volunteers
	- find the smallest number and put i on the left, can't just take a step back...
		- let's find it more methodically
		- this is actually the smallest number i've ever seen
		- i can forget about the 7...,
		- 1 is clearly smaller, but not even smallest, my smallest element so far is 1
		- mariana, where should we put you clearly
		- if this is an array, contiguous, cancan't have the luxury...
	- let's swap everybody
		- if mariella, the 0, smaller, i can start taking fewer bytes
			- 2 was the smallest... now 1 is the smallest
		- 5, 4, let's take 2 where you belong
			- it's like -1 each time, as we verify how this works
			- ![[CleanShot 2024-01-25 at 21.39.52.png]]
			- there's a lot of looping and conditionals in how we were doing this example
			- if you guys could reset yourselves to the same ordering...
- #algorithm
- I liked the intuition where they organically looked to the person, on the left and the right
	- ![[CleanShot 2024-01-25 at 21.41.01.png]]
	- these 2 will swap
		- 7, 5 swap
		- 7, 4 swap
		- 7, 1 swap
		- 7 6 swap
		- 7 0 swap
		- 7 3 swap
		- not done yet... but who is definitely in the right place?
			- Eli (7) is!
			- I can skip him moving forward, taking one byte out
		- 2 5 ok
		- 5 4 swap
		- 5 1 swap
		- 5 6 ok
		- 6 0 swap
		- 6 3 swap
		- don't have to worry about Eli 7
		- or Haron 6
	- even though i'm going back and forth, the problem is getting easier each time
	- 2 4 good
	- 4 1 swap
	- 4 5 ok
	- 5 0 swap
	- 5 3 swap
	- 5 6 7 good
	- 2 1 swap
	- 2 4 ok
	- 4 0 swap
	- 4 3 swap
	- 3 4 5 6 7 ok
	- 2 0 swap 
	- 2 3 ok
	- 1 0 swap
	- 1 2 swap
	- 0 1 ok
	- 1 2 3 4 5 6 7 ok
	- 
Now the example is finished...
- showcased 2 approaches
	- 1. selected the element again and again, looking for the smallest
	- 2. fixing local problems, doing it again and again until all the minor problems
		- this is closer to the organic example
- 1 = "selection sort"
	- deliberately selecting the smallest element again and again
	- don't know a priori, until going through the system at least once
	- this assumes i don't have an infinite amount of memory
		- propose only keeping track of one number at a time
		- smallest element seen thus far
		- if i used more memory, i could remember from the first pass, where 2 is , where 3 is
			- different algo.. more space... more memory
```c
For i from 0 to n-1
	Find smallest number between numbers[i] and numbers[n-1]
	Swap smallest number with numbers[i]
```
- when the loop starts at 0,
	- from the far left to the far right...
	- swap the smallest number with the far left
	- evicting people from the left, then the next, then the next
- this is an algorithm having us go back and forth iteratively selecting the smallest elements
	- think of them representing an array...
	- doors like this where the leftmost = 0
		- rightmost is n - 1
- how many total steps does selection sort perhaps take?
	- let's do a quick visualization
	- ![[CleanShot 2024-01-25 at 21.53.57.png]]
	- tall purple represent large numbers, short represent smaller numbers
		- in pink again and again... equivalent me walking through the volunteers
		- as soon as I have found the smallest element, evict whoever was originally there
			- it holds briefly. in pink what it thinks the smallest number is
		- this algorithm feels kind of slow...
			- doing a lot of work?
			- where is the work coming in?
				- keeps going back and forth and back and forth
				- shaving a little bit of time... doesn't have to go ALL the way back..
				- but cyclicity
					- comparisons again and again
					- I can't remember...
			- efficient or inefficient is something like selection sort?
			- if we have n numbers...
				- `n-1`  first pass
					- that put the smallest number
					- don't need to walk in front of all 8
					- $(n - 1), + (n - 2) + (n - 3) ... + 1$
					- this series here...
						- $n(n - 1)/2$
							- no matter what n is, this formula captures the sum of the series
							- that is how many steps taken again and again when implementing for 8 people
						- = $n^2/2 - n/2$
							- n^2 seems like the biggest term, the *dominant term*
							- dividing this quadratic formula by 2 is big... but as n gets larger and larger...
					- selection sort is on the order of $O(n^2)$
					- kind of a slow one, again
					- can we do actually better?
					- if selection sort is in the order of $n^2$?
						- best case...
						- no special conditional, if sorted already, exit early
				- this line of pseudocode tells us what...
					- not precise, but on the order of $n^2$
					- any number of doors, it's going to end up being omega of $n^2$
					- even in the best case where numbers are already sorted, there is nothing in the algo that says I'm done and exit prematurely
					- $O n^2$,  $\Omega n^2$
						- by coincidence, because these boundaries are the same:
							- $\theta$ $n^2$
- ? Sorry for the repeats!
- *Sorting* is the act of taking an unsorted list of values and transforming this list into a sorted one
- when a list sorted, searching that list is far less taxing on the computer;
	- recall that we can use binary search on a sorted list
	- there are many different types of sorting algorithms
- *selection sort* is one such search algorithm
- the algorithm for selection sort in pseudocode is
```c
for i from 0 to n-1
	Find smallest number between numbers[i] and numbers[n-1]
	Swap smallest number with numbers[i]
```

- can be simplified through `(n - 1) + (n - 2) + (n - 3) + ... + 1`
	- this could be simplified to $n(n-1)/2$ or more simply $O ( n^2)$
- *Bubble sort* is another sorting algorithm that works by repeatedly swapping elements to "bubble" larger elements to the end
- the pseudocode for bubble sort is
```c
Repeat n-1 times
	For i from 0 to n-2
		If numbers[i] and numbers[i+1] out of order
			Swap them
		If no swaps
			Quit
```
- as we further sort the array, we know more and more of it becomes sorted, so we need to only look at the pairs that haven't been sorted yet
- analyzing selection sort, we made only 7 comparisons
	- representing this mathematically, where $n$ represents number of cases, it could be said that selection sort can be analyzed as:
	- $(n - 1) + (n-2) + (n - 3) + \dots + 1$
		- or more simply: 
		- $n^2-n/2$
- considering that mathematical analysis, $n^2$ is really the most influential factor determining efficiency of the sorting algo
	- $\therefore$ selection sort is considered to be of the order of $O(n^2)$ in the worst case where all values are unsorted
	- even when all values are sorted, it will take the same number of steps
	- $\therefore$ the best case can be noted as $\Omega(n^2)$ 
		- since both the upper and lower bound cases are the same, the efficiency of this algo as a whole can be regarded as $\Theta$ $(n^2)$ 
	- analyzing bubble sort, the worst case is $O(n^2)$ 
	- the best case is $\Omega (n)$ 
	- You can visualize a comparison of these algorithms

#mergesort
- better than selection sort and bubble sort

```c 
If only one number
	Quit
Else
	Sort left half of numbers
	Sort right half of numbers
	Merge of numbers
```
- `1346 0257`
	- split into 2 halves
	- suppose this is where I am halfway through merge sort
	- now I need to merge the sorted halves
	- point at your left hand at the left half, then your right hand at the right half
	- in order to merge, slot in an empty array up:
		- which is smaller ( on the left)? 0
		- right hand to the number to the right half to the left half,
			- 1 < 2
		- end up with: `0 1 2`
		- 3 < 5
			- `0 1 2 3`
		- 4 < 5
			- ` 0 1 2 3 4`
		- 6 > 5
			- ` 0 1 2 3 4 5`
		- 6 < 7
			- `0 1 2 3 4 5 6 7`
	- can represent it with the above array, but with the other racks
- selection sort and buble sort
	- only allowed certain amount of memory, just 1 variable to track
	- selection sort
		- keep track of who was the smallest element
	- bubblke sort
		- `i, i + 1`
- in CS, you can trade off 1 resource for another
	- you have to spend more "space", or money
	- conversely, if fine with time, you can get away with lower space
	- deciding what is more 'expensive' for you
- suppose these are the numbers in question:
`6 3 4 1 5 2 7 0`
- selection sort
	- best case $O(n^2)$
	- $\Omega(n^2)$
	- damned if you do, damned if you don't
- bubble sort
	- $\Omega(n^2)$
	- $O(n^2)$
	- sometimes can get lucky and shave time off... resulting in the better $O$ case
- merge sort
	- no more looping back and forth!
	- inspired by recursion: "divide and conquer"
- #mergesort 
- Left half:
	- `6 3 4 1`
	- do so by stealing more memory, (second shelf to work on this)
	- array\[4]
		- let's recursively use the same algorithm, by sorting the left half of this left half...
		- array of size 4, only 3 steps to solve it
		- `6 3`
			- how do we solve and sort an array of size\[2]?
			- sort the left and the right....
				- `6`
				- what is our special base case?
					- quit or return; we're already done
					- list of size\[1] is already done...
				- `3`
					- 🤔
				- well: let's merge them!
		- "left hand right hand"
			- 6 > 3
			- `3 6`, this is sorted! 
	- moving on the right half: `4 1`
		- we apply algorithm again, but we run into the base case
			- quit or return
		- merge: 4 > 1
		- `1 4`
	- now we're at the point in the story where left-left is sorted, left-right is sorted
		- merge and sort!
		- `6 3 4 1` --> `1 3 4 6`
- Right half:
	- sort array of size 4: `5 2 7 0`
		- sort array(s) of size 2: `5 2`
			- sort left then right
			- 5 < 2
			- merge: `2 5`
		- sort array(s) of size 2: `7 0`
			- sort left then right
			- 7 > 0
			- merge: `0 7`
		- merge the 2 arrays of size 2: `2 5 0 7`
			- sort: `0 2 5 7`
- we are at the part of the story where we're on the 2nd shelf
	- ![[CleanShot 2024-02-07 at 21.56.16.gif]]
	- seems that we are comparing 2 values, starting from the left half, to its equivalent on the right;
		- but I suppose this could change depending on what is regarded as higher or lower, and on the criteria of the sort
- seems we kind of cheated
	- the leaves we just said: "SORTED"
		- the "MERGE" is doing the magic of sorting
		- recursion is the thing that bends your brain a bit
	- not a lot of "again & again & again & again"
		- do things once, then move on
	- if it helps you to see in another way...
	- (running Comparison of Sorting Algorithms Site)
		- was using 3 shelves/arrays
		- can be slightly more intelligent and just back and forth between 2 spaces
		- you'll notice that merge sort is working on varying sizes of "halves" and things seem to happen very quickly
		- same number of bars but it happened *way* faster
	- why is that?
		- back to the diagram in question:
$O(n^2),\  O(n log_{n}), \  O(n), \  O(log_{n}), \  O(1)$
- let's see how much better it is:
	- ![[CleanShot 2024-02-07 at 22.01.58.png]]
	- these are remnants or "states" leaving breadcrumbs
	- how many pieces of work...
		- counting 1x1, it seems I moved around things 24 times
		- how do I actually reason about that;
			- my arrays are my temporary shelves
		- $log_{2}{n}$
			- this is because we are doing in half each time
				- dividing, dividing, dividing
				- it's like an inverse exponential function...
			- took a list of size 8
				- 2 lists of size 4
				- 4 lists of size 2
				- 8 lists of size 1
				- 
			- how many times can you do this, starting with 8 numbers? (n = 8)
				-  $log_2(8)$
				- $log_2(2^3)$
					- base and the number can cancel out
					- $log_2(2^3) = 8$
						- which means that's how many times you can divide a problem of size 8 in half, in half, in half
		- each time, in this chart, we had to keep merging them 
			- laterally in this chart, there are $n$ steps
				- I had to merge all of those things back together
			- what is the height of all these remnants?
				- ![[CleanShot 2024-02-07 at 22.06.40.png]]
				- it's 3 as in $log_2(2^3)$
					- this is technically $3 * 8 = 24$ steps
					- more generally, this is $log n = height, \ n width$
		- the total running time of this merge sort:
			- $n\ log_2\ n$
				- and we can throw away the base because that's just a constant factor, resulting in:
			- $n\ log\ n$
				- when we talk about $O$ notation, we have $O(n\ log\ n)$
				- conversely, it is also $Omega(n\ log\ n)$
					- which means that bubble sort might still outperform it
						- at least when inputs are already sorted or certainly relatively small
					- but that's probably ok: the data we're sorting probably isn't sorted
						- we could even half merge sort to do 1 pass to check initially to see if it's already sorted
							- could massage the algo as a result to make it smarter
					- fundamentally, merge sort is 'in theta of' or : $\Theta \ n(log\ n)$
					- it's on the order of $n (log n)$ steps
		- in terms of our chart:
			- merge sort:
				- strictly higher than linear
				- strictly lower than quadratic:
					- $n, \ n^2$
				- it clearly seems to be faster
					- not as good as linear search
					- definitely not as as good as binary sort
					- but better than selection sort and bubble sort
		- final film, minute long that compares of all these algorithms
		- **Visualization and Comparison of Sorting Algorithms - Random**
			- top: selection sort
			- bottom: bubble sort
			- middle: se and hear appreciation of $n(log\ n)$
			- this is *$n(log\ n)$ vis a vis $n^2$*
				- 10 seconds for merge sort to finish
			- Merge sort > selection sort > bubble sort
				- fastest to slowest

## Shorts: 

### Linear Search
- linear search algo we can use to find an element in an array
- algo = step by step instructions for completing a task
- linear search: the idea of the algorithm is to iterate across the array from left to right searching for a specified element
	- in pseudocode:
		- Repeat, starting at the first element:
			- if the first element is what you're looking for (the target), stop
			- otherwise, move to the next element
- ![[CleanShot 2024-02-07 at 22.21.33.png]]
	- start with 11, it's not 9, go to the next one, again and again
		- once we find $9$, we can stop
			- the linear search has completed, succesfully
		- what if we're looking for an element that's not in our array
			- we can keep repeating this process again and again
			- won't know if we've found it until we complete our sequence of the array
	- the algorithm "failed", in that it couldn't find $50$
		- while we didn't find anything, it still succeeds even if not in the array
- **Worst-case scenario**: we have to look through the entire array of $n$ elements, either because the target element is the last element of the array or doesn't exist in the array at all
- **Best-case scenario**: the target element is the first element of the array, and so we can stop looking immediately after we start
- what does this say about the complexity of the search?
	- $O(n)$
		- worst-case
	- $\Omega(1)$
		- best-case
### Binary search
- binary search another algo we can use to find element in an array
- requires special condition, but if met, much quicker
- Binary search: the idea of the algorithm is to divide and conquer, reducing the search area by half each time, trying to find a target number
	- in order to leverage this power, however, our array must first be sorted, else we cannot make assumptions about the array's contents
	- *Pseudocode:*
		- Repeat until the (sub)array is of size\[0]
			- calculate the middle point of the current (sub)array
			- if the target is in the middle:
				- stop
			- otherwise:
				- if the target is less than what's at the middle
					- repeat, changing the end point to be just to the left of the middle
				- if the target is greater than what's at the middle
					- repeat, changing the start point to be just to the right of the middle
- ![[CleanShot 2024-02-07 at 22.28.01.png]]
- array of size\[15]
	- where we're starting to look? where we're stopping to look? what's our middle point?
	- let's first define a set of indices
		- ![[CleanShot 2024-02-07 at 22.28.50.png]]
		- compared to linear search, we need to know how many elements we're iterating over, but we don't actually care about what element;
		- in binary search... we do
	- for binary search, we can't start yet
		- we must have this sorted first!
			- especially if we just discard half of the array
	- you can use any sorting algorithm to fulfill this requirement
	- ![[CleanShot 2024-02-07 at 22.30.08.png]]\
	- calculate the midpoint of this array, targeting 19
		- the first element is index \[0]
		- the last element is index\[14]
		- start and end
		- calculate the midpoint
			- $(0 + 14)/2$
			- midpoint = 7
			- 15 is not what we're looking for
			- however, we know 19 > 15 (what we found at the middle)
				- new start point, is to be just to the right, 
					- then repeat the process again
	- our new array start is index\[8]
	- we've effectively now ignored everything to the left of 15
		- instead of having to search 15 elements, we only have to search 7
- ![[CleanShot 2024-02-07 at 22.33.55.png]]
- let's calculate the new midpoint
	- $((8)+(14)) / 2 = 11$
		- this midpoint value is *less* than what we're looking for
		- repeat the process again, changing the *end* point to be just to the left of the midpoint, new endpoint of $10$
- ![[CleanShot 2024-02-07 at 22.36.11.png]]
- now this is the only part of the array that we have to search through
	- now, finding the new midpoint
		- $((8) + (10)) / 2 = 10$
			- now, our target is in the middle!
			- we have found exactly what we're looking for and can stop
- we know this works if it's in the array... **what if it's not**
- ![[CleanShot 2024-02-07 at 22.37.26.png]]
- let's go through the process again, same size\[15]
	- $((14)+(0)) / 2 = 7$
	- the target > midpoint, repeat, while changing start to be just to the right of the middle
	- ![[CleanShot 2024-02-07 at 22.38.24.png]]
	- ignore the entire left half of the array
	- $((8) + (14)) / 2 = 11$
		- target < midpoint; end point to be just to the left of the middle
	- ![[CleanShot 2024-02-07 at 22.39.29.png]]
		- $((8) + (10)) / 2 = 9$
			- the target < middle; change end point to be just to the left of the midpoint
		- ![[CleanShot 2024-02-07 at 22.40.26.png]]
		- midpoint, starts at 8, ends at 8, that means it's 8
			- $((8) + (8)) / 2 = 8$
			- Are we looking for size\[8] = 17? Nope.
				- we were looking for 16
			- if it does exist... than we'll set the end point to be to the left of our current midpoint
				- change the end point to 7
				- ![[CleanShot 2024-02-07 at 22.42.12.png]]
	- look at how $start > end$, that doesn't make any sense
		- we have a subarray of size\[0]
		- gotten to this point we can guarantee that $16$ does not exist
			- start and end points have crossed
		- slightly different to start = end
			- if we were looking for 17, at the points before those starts and ends cross, it would've been fine, finding 17
			- only when they cross, we can guarantee that it doesn't exist
- **Worst-case scenario:** $O(log_n)$ 
	- We have to divide a list of $n$ elements in half repeatedly ($log_n$) to find the target element, either because the target will be found at the end of the last division or doesn't exist in the array at all
		- we have to cut the problem a certain number of times
			- the certain number of times: $log_n$ 
		- still substantially better than linear search in the worst case
- **Best-case scenario:** $\Omega(1)$ The target element is at the midpoint of the full array, and so we can stop looking immediately after we start
	- 
- binary search is better than linear search
	- but you still have to be able to leverage the power of binary search, by sorting your array first.

### Bubble Sort
- Bubble sort: to move higher valued elements generally towards the right and lower value elements generally towards the left
	- *Pseudocode*
		- Set swap counter to a non-zero value
		- Repeat until the swap counter is 0:
			- Reset swap counter to 0
			- Look at each adjacent pair
				- if 2 adjacent elements are not in order, swap them and add 1 to the swap counter
- If thinking before visualizing, it moves lower valued elements to the left, and higher valued elements to the right
	- accomplishing what we want to do
- ![[CleanShot 2024-02-07 at 22.49.11 1.png]]
	- arbitrarily chose $-1$, set swap counter to a nonzero value
		- repeat this process until $Swap Counter = 0$
			- if $swap counter = 0$ we wouldn't even start the process
		- set $Swap Counter = 0$ then look at every adjacent pair, if out of order, swap them and +1 to swap counter
	- look at each adjacent pair, 5 and 2
	- swap: ![[CleanShot 2024-02-07 at 22.51.15.png]]
	- look at the next adjacent pair, 5 and 1, swap, then add 1
		- ![[CleanShot 2024-02-07 at 22.51.43.png]]
		- 5 and 6 are in order, no need to swap:
		- ![[CleanShot 2024-02-07 at 22.51.52.png]]
			- then 6 and 4, out of order, swap, add +1
			- ![[CleanShot 2024-02-07 at 22.52.14.png]]
- 6 is now at the end!
- in selection sort, we ended up moving smallest elements in the sorted array from left to right, smallest to largest
- in bubble sort, in this algorithm, we're building a sorted array right to left, largest to smallest
	- we have effectively bubbled the greatest value $6$ to the end
	- in future iterations, we don't have to consider 6 anymore
- let's go back to the algo, resetting the swap counter to $0$
	- now look at each adjacent pair
	- ![[CleanShot 2024-02-07 at 22.53.53.png]]
	- 1 and 2 swapped, +1
	- ![[CleanShot 2024-02-07 at 22.54.06.png]]
	- now we've moved 5, the 2nd largest number, to the end
	- we now know that the number is sorted
		- but we need to prove this! need to guarantee that this is sorted
	- the swap counter is 2.
	- reset it to 0; look at each adjacent pair
		- 1 and 2 in order
		- 2 and 3 in order
		- 3 and 4 in order
		- 5 and 6 in order
		- the swap counter is still 0; if we don't need to swap anything, then we don't need to swap anything
			- we can now declare that the entire array must be sorted, because we didn't have to sort it
- **Worst-case scenario:** $O(n^2)$ The array is in reverse order; we have to "bubble" each of the $n$ elements all the way across the array, and since we can only fully bubble 1 element into position per pass, we must do this $n$ times
	- actually
- **Best-case scenario**: $\Omega \ (1)$ The array is perfectly sorted, and we make no swaps on the first pass
	- best case: better than selection sort!

### Selection Sort
- Selection sort: find the smallest unsorted element and add it to the end of the sorted list.
	- *Pseudocode:*
		- repeat until no unsorted elements remain
			- search the unsorted part of the data to find the smallest value
			- swap the smallest found value with the first element of the unsorted part
	- ![[CleanShot 2024-02-07 at 22.59.04.png]]

- the entire array is the unsorted part
	- search through and find the smallest value = 1
	- part 2: swap that value with the 1st element of the unsorted part = 5
	- 1 and 5 are swapped
		- ![[CleanShot 2024-02-07 at 23.00.12.png]]
	- we can visually see we moved the smallest value in the array to the beginning, effectively sorting it
		- ![[CleanShot 2024-02-07 at 23.00.34.png]]
		- repeat the process again through the unsorted array
			- 2 is the smallest, the smallest value in the unsorted array is $2$
			- it's sorted
		- find the smallest in the unsorted = $3$ then swap it with the 1st value
			- $3$ is the smallest, $5$ is the first
			- ![[CleanShot 2024-02-07 at 23.01.50.png]]
		- find the smallest in the unsorted, = $4$, first element is $5$
			- 4 is sorted
		- find smallest =$5$ then first element is $6$
		- the only element is $6$, and confirm it is sorted. 
			- 6 is the smallest, and only element, no actions left to take.
- **Worst-case scenario**: $O(n^2)$ We have to iterate over each of the $n$ elements of the array (to find the smallest unsorted element ) and we must repeat this process $n$ times, since only 1 element gets sorted on each pass
- **Best-case scenario:** $\Omega(n^2)$Exactly the same, there's no way to guarantee the array is sorted, until we go through this process for all the elements
- $\Theta n^2$

- note that the representations are based in "scale"
	- $n^2$ is not the exact same thing as say, where $n = 5$, that the value is $25$
		- instead, it refers to the scale of $n^2$ over $n$, $n\ log(n)$ , $log \ n$, $1$. 
## Algorithm Breakdown (O, Big O, Theta)
![![Spaces/Home/CS50/#^Table]]



### Recursion
- if you code for a long time, you might come to see it as beautiful
	- it's a very neat way that it looks
	- Recursion is one way of getting beautiful/elegant looking code
	- surprisingly short
- Recursion: We might describe an implementation of an algorithm as being particularly "elegant" if it solves a problem in a way that is both interesting and easy to visualize
	- the technique of **recursion** is a very common way to implement such an "elegant" solution
	- the definition of a recursive function is one that, as part of its execution, invokes itself
	- the factorial function ($n!$) is defined over all positive integers
	- $n!$ equals all of the positive integers less than or equal to $n$, multiplied together
	- Thinking in terms of programming, we'll define the mathematical function $n!$ as `fact(n)`
	- ![[CleanShot 2024-02-08 at 21.16.27.png]]
	- the factorial of 1 = 1
		- factorial 2 is 2 x1
		- factorial 3 is 3 x 2 x 1
	- ![[CleanShot 2024-02-08 at 21.17.28.png]]
	- fact 4 = 4 * fact 3*
	- $n! = n * !(n-1)$
		- `fact(n) = n * fact(n-1)`
			- n times the product of all numbers less than me
			- lets us recursively define the factorial function
- this forms the basis for a **recursive definition** of the factorial function
- every recursive function has 2 cases that could apply, given any input:
	- the *base case*, which when triggered will terminate the recursive process
	- the *recursive case*, which is where the recursion will actually occur
		- where it calls itself; slight variation that makes it a teensy bit smaller
		- passing it down to a diff call down the line
	- which is the base case in the factorial function?
		- ![[CleanShot 2024-02-08 at 21.20.48.png]]
		- the Factorial of 1 is just 1; no multiplication is necessary
			- we need to stop the factorial somewhere, maybe when we get to 1?
```c
int fact(int n)
{
	if (n == 1)
		return 1;
	else
		return n * fact(n-1);
//recursive case
}
```
- the base case is where
	- you can get rid of curly braces if only 2 branches
	- we're making the problem smaller
	- if n = 5, n times fact 4
		- call stacking: we will learn exactly why this process works
		- as we approach 1, all of the previous functions have the answer they are looking for
		- fact 2 = 2 x fact 1
		- fact 1 returns 1
		- call for fact 2 gives it back to fact 3, 3 x 2 = 6, then give it back to fact 4
		- this alone is the solution to calculating the fractorial of a number
- in general, but not always, recursive functions replace loops in non-recursive functions
- 
```c
int fact(int n)
{
	if (n == 1)
		return 1;
	else
		return n * fact(n-1);
}

// top is recursive; bottom is iterative

int fact2(int n)
{
	int product = 1;
	while(n > 0)
	{
		product *= n;
		n--;
	}
	return product;
}
```
- notice we have to declare an integer product
	- keep multiplying by n and decrementing n until we calculate the product
	- these do the same thing but don't do it in the same way
- it is possible to have > 1 base or recursive case, depending on what your function is looking to do
	- the fibonacci sequence is an example of this
	- **Multiple base cases**: the Fibonacci number sequence is defined as follows:
		- base case: n = 1; the first element is 0
		- base case: n = 2; the second element is 1
		- the $n^th$ element is the sum of the $(n-1)^th and (n-2)^th$ elements
		- every other element is defined as the sum of n-1 and n-2
		- third element is 0 + 1 = 1
		- 4th element is 1 + 1 (3rd) = 2
		- 
	- **Multiple recursive cases**: The Collatz conjecture.
		- The Collatz conjecture is applied to positive integers and speculates that it is always possible to get "back to 1" if you follow these steps:
			- if $n$ is 1, stop
			- Otherwise, if $n$ is even, repeat this process on $n/2$
			- Otherwise, if $n$ is odd, repeat this process on $3n + 1$
		- Write a recursive function `collatz(n)` that calculates how many steps it takes to get to 1 if you start from n and recurse as indicated above
		- ![[CleanShot 2024-02-08 at 21.34.23 1.png]]
		- above is various Collatz numbers
		- if n = 1 ; collatz(n) = 0 (no need for steps)
			- n = 2; it's going to be 1 step + however many steps it takes for 1 (0)
			- n = 3;
				- 3, 10, 5, 16, 8, 4, 2, 1
		- how can you define collatz(n) so that it calculates how many steps it takes to get to 1
```c
int collatz(int n)
{
	
	// base case
	
	if (n == 1)
		return 0;
	else (n % 2 == 0)
		return collatz / 2
	else
		return 3 * collatz + 1


	
}
```
- ![[CleanShot 2024-02-11 at 00.41.00@2x.png]]

```c
int collatz(int n)
{
	// base case
	if (n == 1)
		return 0;
	// even numbers
	else if ((n % 2) == 0)
		return 1 + collatz(n / 2);
	// odd numbers
	else
		return 1 + collatz(3 * n + 1);
}
```
- 
- n = 3
	- 3; return base case value count of 1 (+1)
		- return 1 (+1 = 2) + collatz(3 * 3 + 1) = collatz(10)
			- 2 + collatz(10)
			- 2 + 1 + collatz(5)
				- 3 + collatz(5)
					- 3 + collatz(3 * n + 1)
						- 3 + collatz(16)
							- 3 + 1 + collatz(8)
							- 4 + collatz (8/2)
							- 4 + collatz (4)
								- 4 + 1 + collatz(4/2)
								- 5 + collatz(2)
								- 5 + 1 + collatz(2/2)
								- 6 + collatz(1)
								- 6 + 1
								- 7
### Merge Sort
- all other sorting algos have an $O(n^2)$ 
- in merge sort, the idea of the algorithm is to sort smaller arrays and then combine those arrays together (merge them) in sorted order
	- merge sort leverages something called **recursion** (already covered)

- Pseudocode:
	- sort the left half of the array (assuming n > 1)
	- sort the right half of the array (assuming n > 1)
	- merge the 2 halves together
- instead of one six-element array...
	- let's imagine six one-element arrays, and then merge them together!
- recursion-wise, this will talk about recursion quite a bit
- this is the most complicated of the 4 other types of algorithms
![[CleanShot 2024-02-12 at 12.51.18@2x.png]]
- algorithm doesn't yet 'know' how to solve this
	- split this again, considering that we must be consistent in whichever side is larger or smaller for "left" or "right"
	- ![[CleanShot 2024-02-12 at 12.52.06@2x.png]]
		- a single element array must necessarily be sorted, so we can mark this as sorted!
		- go back to the right half of the purple part:
			- ![[CleanShot 2024-02-12 at 12.52.46@2x.png]]
			- recursively speaking, we drill down and find 2 > 1, so it is sorted
			- ![[CleanShot 2024-02-12 at 12.53.50@2x.png]]
			- which one has the lower element? 1 does; we insert 1 into a new hypothetical array, and then 2 afterwards
			- we're still looking into the left half of the red array
				- now we're finished with the halves of the purple array, let's look at the entire purple array
				- 5, 1 , 2
					- ![[CleanShot 2024-02-12 at 12.54.57@2x.png]]
			- compare the first element of the right part, and the first element of the left part
				- 5 compare 1, 5 > 1
				- 1 becomes the new first element in a new array
					- 2 < 5, 2 becomes the next element
					- 5 or null lower? well, the real option is 5, so 5.
				- ![[CleanShot 2024-02-12 at 12.58.13@2x.png]]
- recursively thinking, we have sorted the left half of the overall red array
	- let's put this aside now, and do the right half
	- 'arbitrarily' divide it, 3 is the single element. 
	- ![[CleanShot 2024-02-12 at 12.59.14@2x.png]]
	- now for the right half, with > 1 element, sort the left half and the right half:
		- ![[CleanShot 2024-02-12 at 12.59.36@2x.png]]
		- ![[CleanShot 2024-02-12 at 12.59.45@2x.png]]
		- now again, do the side by side comparison for this array level: 6 and 4
		- 4 < 6; 1st element in the sub array, 6 = 2nd element in the sub array
		- now sorted left and right halves of the right half
	- with the exact same process, we compare:
		- Lower: 3 or 4? 3.
			- 3
			- Lower: 4 or NULL? 4.
				- since nothing is left on the left side, then everything on the right has to be bigger than what's in the merged array.
				- the only thing left is on 1 side, therefore, everything (can) must go
				- ![[CleanShot 2024-02-12 at 13.02.48@2x.png]]
- we have gone through 2/3 of the pseudocode steps
- final thing we have to do is merge the 2 halves together
- ![[CleanShot 2024-02-12 at 13.03.20@2x.png]]
	- which is lower, 1 or 3?
		- ![[CleanShot 2024-02-12 at 13.03.51@2x.png]]
		- 1.
	- Lower: 2 OR 3?
		- 2.
		- ![[CleanShot 2024-02-12 at 13.04.03@2x.png]]
	- Lower: 5 or 3?
		- 3.
		- ![[CleanShot 2024-02-12 at 13.04.13@2x.png]]
	- Lower: 5 or 4?
		- 4.
		- ![[CleanShot 2024-02-12 at 13.04.29@2x.png]]
	- Lower: 5 or 6?
		- 5.
		- ![[CleanShot 2024-02-12 at 13.04.47@2x.png]]
	- Lower: 6 or Null?
		- 6.
	- going through this process recursively, smaller and smaller sub-arrays
		- everything is now in order, in dark-blue
		- not as necessarily straight forward as selection sort or bubble sort...
			- but:
			- **Worst-case scenario:** We have to split $n$ elements up and then recombine them
				- effectively doubling the sorted subarrays as we build them up (combining sorted 1-element arrays into 2-element arrays, combining sorted 2-element arrays into 4-element arrays...)
				- $n\ log n$ 
					- since we have $n$ elements, and then combine if doubling $n$ times...
					- this in general is faster than $n^2$ 
					- it can be slower than bubble sort $\O (n)$ 
				- in an average case though, merge sort is faster, at the cost of taking up more memory
				- very powerful! can make speed of sorting an array that much faster.
			- **Best-case scenario:** The scenario is already perfectly sorted, but we still have to split and recombine it back together with this algorithm
				- $n\ log n$
- 

## Section
### Searching and Sorting
- list of people on the screen,
	- not necessarily sorted by name
		- {Matthew, Samia, Alyssa, Douglas, Cecelia, Lucas, Ramya}
	- spotting Alyssa out of these people is quite easy
	- what algorithm to use to search, if the list isn't sorted?
		- we could use linear search or binary search
		- linear search: start at one side, then look at 1 person then the other person
	- imagine if we were on a stage, and a light illuminates only 1 person
		- see: matthew, then samia, then *alyssa*, 
		- this is an example of linear search
	- let's imagine us rearranging the entire list...
- *binary search*
	- phonebook: opened to the middle, too far, or not far enough, then tear off the half
	- binary search: look in the middle: then have we gone too far or not far enough?
		- if this is alphabetical.... then we've gone too far since Lucas (who is in the middle) is too far
	- cecelia! still too far, let's tear it off,
		- we land on Alyssa (we win!)
- How many steps did each algorithm take?
	- Linear Search: 3 steps
	- Binary Search: 3 steps
	- it seems like both searches performed the same way; they *seem* equivalent
		- the choice depends on the problem size, and the input, and al ittle bit of luck
	- the preferred idea is that it is dependent on the size of the data set
- what's the greatest number of steps that these algorithms will take?
	- (sounds like big O)
	- linear search: 7
	- binary search: $log_{2} (7)$, about 3 steps
	- think about the number of steps in a general case, with $n$
	- $linear search = \O (N)$
	- $binary search = \O(log(N))$
	- the *upperbounds*
	- you can find different algorithms; researches are finding faster ways than even binary search
- case where we're tying to find Alyssa with Linear Search
	- Binary Search, let's say it's a bit shuffled, and still looking for Alyssa
		- Alyssa, luckily, is in the middle
- What's the fewest number of steps that an algorithm could take? (Omega notation)
	- $linear search: (1)$
	- $binary search: (1)$ 
	- computer scientists think about 1 input, they think about general inputs
		- best case scenario is for both, it's a minimum of 1 step for each algorithm
	- even if it took a fixed number of steps, it would still be called (1)
		- again, this is the COUNT OF STEPS, not the count of inputs or values

- ? An algorithm is a set of clearly defined steps to accomplish a task
- & **Thought Question**
	- Suppose that you create a new algorithm and assess its runtime
	- the *fewest* steps this algorithm will ever take is 2, and only 2
	- what is the $\Omega$ notation for this algorithm
		- the best case will take 2 steps, it might make sense to say $\Omega 2$, but it's still on the order of $\Omega(1)$

$\O (1), \O(log(N)), \O(N), \O(N^2), \Omega(1), \Omega(log(N)), \Omega(N), \Omega(N^2)$

- let's say the algo takes 100 steps in the worst case, well... it kind of depends now
	- don't ask how many steps the algo took, but how does the algo scale?
		- if it's a fixed count of steps, it's $\O (1)$
		- however if I added one more elemnt for 101 or 102 steps,
			- that would be $\O(n)$
- there's this idea that we can take any algorithm and translate it into any language we want

### Sort
- Sort problem in PSET, let's tackle this...

MERGE SORT: $\O(N log(N)), \Omega(Nlog(N))$
SELECTION SORT: $\O(N^2), \Omega(N^2)$
BUBBLE SORT: $\O(N^2), \Omega(N)$

- ! While merge sort is faster, there are scenarios where bubble sort is faster
	- in the best cases, bubble sort will be faster
		- if data is closer to being sorted, then bubble sort will actually be faster
		- for every steps we do, we have to add in another $log N$ steps in merge sort
			- for bubble sort, we don't have to do that...
- Sorting problem asks you to determine the identities of 3 different undefined sorting algorithms
	- give them those inputs, and ask them, then characterize the algorithms based on what you're doing

- reversed = the worst case
- sorted = the best case
- random = the "average" case
![[CleanShot 2024-02-14 at 21.28.07.png]]

- Why is the reversed list faster than sorting the sorted list for Merge sort?
	- since we've assigned best case or worst case
	- runtime factors; algorithms aren't assessed on runtime length, but the number of time (given how many other processes could compound the runtime)

### Struct
- we want to create our own data type to use; it's not a full data type, but a way to encapsulate information so we can refer to it with only 1 name
- We have 2 candidates for the US government
	- in an election, what would you store?
	- (probably: name, number of votes)
- up until now we could create indep variable
	- struct lets us combine pieces into a single 1, and 

```c
typedef struct
{
	string name;
	int votes;
}
candidate;

// 1st
candidate president;
president.name = "Samia";
president.votes = 10;


// or

candidate candidates[4];
```
- we can break down the syntax here;
	- create a new "type" that can be re-used in the rest of the file
	- known as a structure's **members**
		- one is a string we'll call "name"
		- one is an integer we'll call "votes"
	- we can reuse that type throughout the program
- let's create a candidate called president
	- notice the line where we have the datatype "candidate" named "president"
	- i could access them using the member's name
	- could you have a struc of other strucs?
		- yeah probably lmao
- can we change the data in there if needed during runtime?
	- could assign diff values to this candidate's members
		- (name/votesa)
		- shouldn't be able to store new values while running the program
- could we just use candidate.name instead?
	- candidate is not a variable, but a template for one...
		- every new candidate we created needs its own name
		- candidate.president, candidate.vicepresident, candidate.candidate
			- do have to substantiate type name then the variable name

### Most Votes
- Create an array of candidates
- Search the array to find the most votes awarded to any single candidate
	- maybe using linear search?
- Print out that candidate's name
- goal: get practice using syntax of strucs

![[CleanShot 2024-02-15 at 21.18.15.png]]
i would argue that there's no way to avoid repeating code here
- Carter's votes are greater than 0, but this is still invalid
- should I print it despite it being the greatest?
	- need to confirm this is a certainty...

### Recursion
- often, recursion tends to be a very elegant solution to some problems
- it's not always the best way to do things
- 1 problem like this is the idea of a factorial
	- (take the last number, apply current int x value of last output)
- Factorial: multiply number by n -1, n - 2, n - (n - 1)
	- 1! = 1; 2! = 2 * 1; 3! = 3 * 2 * 1
	- 4! = 4 * 3 * 2 * 1
- base cases, 
	- compute 2 factorial for 3 factorial
	- to figure out 4 factorial, find 3 factorial, find 2 factorial, find 1 factorial = 1
		- since factorial definition aside from base case is n x factorial(n - 1)
$4! = 4 * 3!$ call stack
$3! = 3 * 2!$ call stack
$2! = 2 * 1!$ call stack
$1! = 1$ base case 

go down the call stack, then up the call stack after establishing the base case

- recursion faster than regular loops?
	- recursion is great, but it isn't always necessarily faster
- solve the problem by first solving smaller version of that problem



let's take the same idea and translate it into code
- Carter showing how this all interacts with `debug50`
- as things come into picture here....
	- you will see variables portion and call stack
![[CleanShot 2024-02-15 at 21.58.14.png]]

in a call stack, what order do calls execute?
	- this is called a stack
	- the last function added was factorial
	- the 1st function....
	- resolve the very 1st base-case



			 




## PSET 3

### Runoff
#### Problem to solve
You already know about plurality elections, which follow a very simple algo for determining the winner of an election;
- every voter gets 1 vote
- the candidate with most votes wins.

but the plurality vote does have some disadvantages; what happens in an election with 3 candidates, and the ballots below are cast?
![[CleanShot 2024-02-16 at 00.08.43@2x.png]]
- A plurality vote would here declare a tie between Alice & Bob, since each have 2 votes
	- but is that the *right* outcome?

**There's another kind of voting system known as a ranked-choice voting system**
- in a ranked-choice system, **voters can vote for >1 candidate**
	- instead of just voting for their top choice, they can rank the candidates in order of preference
![[CleanShot 2024-02-16 at 00.09.45@2x.png]]

- Here, each voter, in addition to specifying their 1st preference candidate, has also indicated their 2nd and 3rd choices
	- now, what was previously a tied election could now have a winner
- the race was originally tied between Alice and Bob, so Charlie was out of the running
- **But the voter who chose Charlie preferred Alice over Bob, so Alice could here be declared the winner**

Ranked choice voting can also solve yet another potential drawback of plurality voting:
![[CleanShot 2024-02-16 at 00.11.10@2x.png]]
- **Who should win this election?**
	- in a plurality vote: Charlie wins the election with 4 votes
	- Bob has only 3.
	- Alice only has 2.
	- A majority of the voters (5 out of the 9) would be happier with either Alice or Bob instead of Charlie
- By considering ranked preferences, a voting system may be able to choose a winner that better reflects the preferences of the voters
- one such ranked choice voting system is the instant runoff system
	- **instant runoff: voters can rank as many candidates as they wish**
	- if any ***candidate has a majority (>50%) of the 1st preference votes, that candidate is declared the winner of the election***
	- **if no candidate has >50% of the vote, then an "instant runoff" occurs**
		- *the candidate who received the fewest number of votes is eliminated from the election*
		- *anyone who originally chose that candidate as their 1st preference now has their 2nd preference considered*
		- why do it this way?
			- effectively, this stimulates what would have happened if the least popular candidate had not been in the election to begin with
		- **this process repeats: if no candidate has majority of the votes, the last place candidate is eliminated**
		- *once a candidate has a majority, that candidate is declared the winner*
		- *and anyone who voted for them will instead for their next preference (who isn't already been eliminated)*
Sounds a bit more complicated than a plurality vote, doesn't it?
- it arguably has the benefit of being an election system where the winner of the election more accurately represents the preferences of the voters
- in a file called `runoff.c` in a folder called `runoff`, create a program to simulate a **runoff election**

#### Understand the code in `runoff.c`
- whenever you'll extend the functionality of existing code, it's best to be sure you first understand it in its present state

- Look at the top of `runoff.c` first
	- 2 constants are defined:
	- `MAX_CANDIDATES` for the max. number of candidates in the election
	- `MAX_VOTERS` for max. number of voters in the election

```c 
// Max voters and candidates

\# define MAX_VOTERS 100
\# define MAX_CANDIDATES 9
```

- notice that `MAX_CANDIDATES` is used to size an array `candidates`

```c
// Array of candidates
candidate candidates[MAX_CANDIDATES];
```
- `candidates` is an array of `candidate`
	- in `runoff.c`, a candidate is a 'struct'
		- every candidate has a `string` field for their `name`
			- also, an int representing the number of `votes` they currently have
			- `bool` value called `eliminated` that indicates whether the candidate has been eliminated from the election
			- the array `candidates` will keep track of all of the candidates in the election
```c
// Candidates have name, vote count, eliminated status
typedef struct
{
	string name;
	int votes;
	bool eliminated;
}
candidate;
```
- Now you can better understand `preferences`, the two-dimensional array
	- the array `preferences[i]` will represent all of the preferences for voter number `i`
	- the integer, `preferences[i][j]` will store the index of the candidate, from the `candidates` array, who is the `j`th preference for voter `i`

```c
// preferences[i][j] is jth preference for voter i

int preferences[MAX_VOTERS][MAX_CANDIDATES];
```
- **the program also has 2 global variables: `voter_count` and `candidate_count`**

```c
// Number of voters and candidates
int voter_count;
int candidate_count;
```

##### Main
Notice that after determining the number of candidates & number of voters, *the main voting loop begins **giving every voter a chance to vote**
- as the voter enters their preferences, the `vote` function is called to keep track of all of the preferences
- if at any point the ballot is deemed to be invalid, **the program exits**

- Once all of the voters are in, another loop begins:
	- this one is going to keep looping through the runoff process of checking for a winner and eliminating the last place candidate until there is a winner

- the first call here is to a function called `tabulate` which should look at all of the voters' preferences and compute the current vote totals
	- this is done by looking at each voter's top choice candidate who hasn't yet been eliminated
- next, the function `print_winner` should print out the winner if there is one
	- **if there is,  the program is over**
	- otherwise, the program needs to determine the fewest number of votes anyone still in the election received 
		- via a call to `find_min`
	- if it turns out that everyone in the election is tied with the same number of votes (as determined by the `is_tie` function):
		- the election is declared a tie
	- otherwise, the last-place candidate (or candidates) is eliminated from the election via a call to the `eliminate` function
- if you look a bit further down in the file, you'll see that the rest of the functions - `vote`, `tabulate`, `print_winner`, `find_min`, `is_tie`, and `eliminate` - are all left up to you to compute

- you should not modify anything else in `runoff.c` (and the inclusion of additional header files if you'd like)

#### Personal Thoughts
The following functions need to be completed or defined:
- `vote`
	- used to keep track of all of the preferences
	- if at any point the ballot is deemed to be invalid, the program exits
- `tabulate`
	- looks at all voters' preferences and computes the current vote totals
		- by looking at each voter's top choice candidate who hasn't yet been eliminated
- `print_winner`
	- this should print out the winner if there is one
		- if there is, the program is over
		- otherwise, the program needs to determine the fewest number of votes anyone still in the election received (via a call to find_min)
			- int, then comparison in if statement
			- this seems like we'll need some kind of search/sorting function?
- `find_min`
	- this is the actually sort or search function? idk.
- `is_tie`
	- if it turns out that everyone in the election is tied with the same number of votes here,
		- declare the election a tie
- `eliminate`
- Otherwise.... the last-place candidate or candidates eliminated from the election via a call here


```c
candidates[i].name = argv[i + 1];

candidates[i].votes = 0;

candidates[i].eliminated = false;
```

- ! Preferences are stored in the array `preferences[i][j]
	- i = the voter i
	- j = the preference # j
- 
  1. the program first validates whether the given parameters for candidates are valid or not
    1. there must be at least 2 candidates, otherwise it exits and lets the user know
  2. the program populates the array of candidates given the arguments (argc)
    1. the program then iterates through the given arguments, assigning them:
      1. the name through argv
      2. a `0` vote value
      3. a `false` flag for whether they are considered `eliminated`
  3. the program evaluates the number of voters as well
  4. the program queries the user for votes based on how many voters there are, and how many candidates there are
    1. main calls the `vote` function, which returns a boolean value
      1. provided the boolean expression results in `true` proceed onwards
      2. otherwise: report that it is an invalid vote and exit the program
  5. If a winner cannot be declared, proceed with runoffs (not a function)
  6. calculate the number of votes in a while loop (true)
    1. if an election has been won (checked through calling print_winner), exit the loop
    2. find the minimum value of votes that a candidate has received; assign them the `min` boo flag for the election
      1. done through `find_min()` and `is_tie`
  7. if this is a tie, everyone is regarded as a winner and break the loop
  8. if a loser is found with a minimum number of votes, `eliminate(min)` shall be called and the minimum vote count candidate is eliminated
  9. the vote counts are reset to 0
1. 
#### Completed Code
```c 
#include <cs50.h>
#include <stdio.h>
#include <string.h>

// Max voters and candidates
#define MAX_VOTERS 100
#define MAX_CANDIDATES 9

// preferences[i][j] is jth preference for voter i
int preferences[MAX_VOTERS][MAX_CANDIDATES];

// Candidates have name, vote count, eliminated status
typedef struct
{
    string name;
    int votes;
    bool eliminated;
} candidate;

// Array of candidates
candidate candidates[MAX_CANDIDATES];

// Numbers of voters and candidates
int voter_count;
int candidate_count;

// Function prototypes
bool vote(int voter, int rank, string name);
void tabulate(void);
bool print_winner(void);
int find_min(void);
bool is_tie(int min);
void eliminate(int min);

int main(int argc, string argv[])
{
    // Check for invalid usage
    if (argc < 2)
    {
        printf("Usage: runoff [candidate ...]\n");
        return 1;
    }

    // Populate array of candidates
    candidate_count = argc - 1;
    if (candidate_count > MAX_CANDIDATES)
    {
        printf("Maximum number of candidates is %i\n", MAX_CANDIDATES);
        return 2;
    }
    for (int i = 0; i < candidate_count; i++)
    {
        candidates[i].name = argv[i + 1];
        candidates[i].votes = 0;
        candidates[i].eliminated = false;
    }

    voter_count = get_int("Number of voters: ");
    if (voter_count > MAX_VOTERS)
    {
        printf("Maximum number of voters is %i\n", MAX_VOTERS);
        return 3;
    }

    // Keep querying for votes
    for (int i = 0; i < voter_count; i++)
    {

        // Query for each rank
        for (int j = 0; j < candidate_count; j++)
        {
            string name = get_string("Rank %i: ", j + 1);

            // Record vote, unless it's invalid
            if (!vote(i, j, name))
            {
                printf("Invalid vote.\n");
                return 4;
            }
        }

        printf("\n");
    }

    // Keep holding runoffs until winner exists
    while (true)
    {
        // Calculate votes given remaining candidates
        tabulate();

        // Check if election has been won
        bool won = print_winner();
        if (won)
        {
            break;
        }

        // Eliminate last-place candidates
        int min = find_min();
        bool tie = is_tie(min);

        // If tie, everyone wins
        if (tie)
        {
            for (int i = 0; i < candidate_count; i++)
            {
                if (!candidates[i].eliminated)
                {
                    printf("%s\n", candidates[i].name);
                }
            }
            break;
        }

        // Eliminate anyone with minimum number of votes
        eliminate(min);

        // Reset vote counts back to zero
        for (int i = 0; i < candidate_count; i++)
        {
            candidates[i].votes = 0;
        }
    }
    return 0;
}

// Record preference if vote is valid
bool vote(int voter, int rank, string name)
{
    // Iterate through each indivudal candidate
    for (int k = 0; k < candidate_count; k++)
    {
        // Check if a match is found:
        if (strcmp(candidates[k].name, name) == 0)
        {
            // Creating and setting index; preferences[0][0] is the 1st voters 1st choice, which will be candidate [k].
            preferences[voter][rank] = k;
            // Debugging, checking if correct candidate is returned/set
            printf("Candidate name: %s\n", candidates[preferences[voter][rank]].name);

            // Affirming that the function has run (vote valid, preference noted)
            return true;
        }
    }
    // Affirming that the function has not run correctly (vote invalid, preference not noted)
    return false;
}

// Tabulate votes for non-eliminated candidates
void tabulate(void)
{
    // Iterate through each voter
    for (int i = 0; i < voter_count; i++)
    {
        // Iterate through each candidate
        for (int j = 0; j < candidate_count; j++)
        {
            // Check whether the voter's first preferred candidate is NOT eliminated ("false")
            if (candidates[preferences[i][j]].eliminated == false)
            {
                // Increment vote count for candidate by 1.
                candidates[preferences[i][j]].votes++;
                break;
            }
        }
    }
    return;
}

// Print the winner of the election, if there is one
bool print_winner(void)
{
    // To help identify the winner provided criterion are met.
    int vote_max = 0;

    // Calculate the number of winners (checking if there is a singular winner or no)
    int winners = 0;

    // Iterate through all candidates
    for (int i = 0; i < candidate_count; i++)
    {
        if (!candidates[i].eliminated && candidates[i].votes > vote_max)
        {
            vote_max = candidates[i].votes;
        }
    }

    // Count how many candidates have the maximum number of votes
    for (int i = 0; i < candidate_count; i++)
    {
        if (!candidates[i].eliminated && candidates[i].votes == vote_max)
        {
            winners++;
        }
    }

    // Here, create float variable to determine percentage:
    // Calculate if majority has been reached (> 0.50); type-cast variables in expression to ensure intended results
    float proportion = ((float) vote_max / voter_count);

    // If only 1 winner, find and print the winner's name
    if (winners == 1 && (proportion > 0.50))
    {
        for (int i = 0; i < candidate_count; i++)
        {
            if (!candidates[i].eliminated && candidates[i].votes == vote_max)
            {
                printf("Winner: %s\n", candidates[i].name);
                return true;
            }
        }
    }
    return false;
}

// Return the minimum number of votes any remaining candidate has
int find_min(void)
{
    // Use -1 as an initial value to indicate it's not set
    int vote_min = -1;

    for (int i = 0; i < candidate_count; i++)
    {
        // If this is the first non-eliminated candidate, or if this candidate's votes are fewer than current vote_min
        if (!candidates[i].eliminated && (vote_min == -1 || candidates[i].votes < vote_min))
        {
            vote_min = candidates[i].votes;
        }
    }

    return vote_min;
}

// Return true if the election is tied between all candidates, false otherwise
bool is_tie(int min)
{
    // To satisfy "return true if the election is tied between all candidates"
    int ties = 0;

    // Function only looks at non-eliminated candidates
    int non_eliminated_count = 0;

    // Iterate through all candidates
    for (int i = 0; i < candidate_count; i++)
    {
        // Evaluate if the candidate is eliminated; if not, do not do anything
        if (!candidates[i].eliminated)
        {
            // Increment non eliminated candidate count by 1
            non_eliminated_count++;

            // If the candidates votes are equal to the minimum vote total
            if (candidates[i].votes == min)
            {
                // Increment 'ties'
                ties++;
            }
        }
    }

    // This function returns true if the election is tied between all active candidates, and false otherwise
    return ties == non_eliminated_count;
}

// Eliminate the candidate (or candidates) in last place
void eliminate(int min)
{
    int non_eliminated_count = 0;
    int to_eliminate = 0;

    // 1st Iterationto count non-eliminated candidates and those with minimum votes
    for (int i = 0; i < candidate_count; i++)
    {
        if (!candidates[i].eliminated)
        {
            non_eliminated_count++;
            if (candidates[i].votes == min)
            {
                to_eliminate++;
            }
        }
    }

    // Proceed with elimination only if NOT all active candidates are at the minimum
    if (to_eliminate < non_eliminated_count)
    {
        for (int i = 0; i < candidate_count; i++)
        {
            if (!candidates[i].eliminated && candidates[i].votes == min)
            {
                // Eliminate the candidate
                candidates[i].eliminated = true;
            }
        }
    }

    // No further action necessary
    return;
}
```
### Tideman
#### Problem to Solve
You already know about plurality elections, which follow a very simple algo for determining the winner of an election:

But the plurality does have some disadvantages; what happens, for instance, in an election with 3 candidates, and the ballots below are cast?

- plurality would here declare a tie between Alice & Bob, since each has 2 votes; but that is the right outcome?
- there's another kind of voting system known as a ranked-choice voting system; in a ranked-choice, voters can vote for more than 1 candidate
	- instead of just voting for their top choice, they can rank the candidates in order of preference
	- the resulting ballots might therefore look like the below:
- Here, each voter, in addition to specifying their 1st pref candidate, has also indicated their 2nd & 3rd choices
	- and now what was previously a tied election could now have a winner
	- the race was originally tied between Alice and Bob



3 candidates, 10 voters
7 vote for 1
1 votes for 2
2 vote for 3





### Sort
#### Problem to solve
- recall from lecture that we saw a few algo's for sorting a sequence of numbers:
	- selection sort
		- iterates through the unsorted portions of a list, selecting the smallest element each time and moving it to its correct location
	- bubble sort
		- compares pairs of adjacent values one at a time and swaps them if they are in the incorrect order
		- this continues until the list is sorted
	- merge sort
		- recursively divides the list into 2 repeatedly and then merges the smaller lists back into a larger one in the correct order
		- 
- In this problem:
	- you'll analyze 3 (compiled!) sorting programs to determine which algorithms they use
	- in a file called `answers.txt` in a folder called `sort`:
		- record your answers
			- an explanation for each program by filling in the blanks marked `TODO`

#### Distribution Code
- for this problem, you'll need some "distribution" code
	- that is, code written by CS50 staff.
	- provided to you are 3 already compiled C programs
		- `sort1, sort 2, sort3`
		- several `.txt` files for input 
		- `answers.txt` in which to write your answers
- each of the `sort` files implements a different sorting algorithm:
	- selection sort
	- bubble sort
	- merge sort
	- (not necessarily in that order)
- ! Your task is to determine which sorting algorithm is used by each files; start by downloading these files

![![Spaces/Home/CS50/#^Table1]]

### Plurality
#### Problem to solve
- elections come in all shapes and sizes
	- in the UK, the Prime Minister is officially appointed by the monarch, who generally chooses the leader of the political party that wins the most seats in the House of Commons
	- the US uses a multi-step Electoral College process where citizens vote on how each state should allocate Electors who then elect the President
- Perhaps the simplest way to hold an election though is via method commonly known as the *plurality vote* (known as: *first-past-the-post* or *winner take all*)
	- in the plurality vote, every voter gets to vote for one candidate
	- at the end of the election, whichever candidate has the greatest number of votes is declared the winner of the election
- for this problem: you'll implement a program that runs a plurality election per the below:

#### Distribution Code
- for this problem, you'll extend the functionality of "distribution code" provided to you by CS50 staff
	- log into **cs50.dev**, click on your terminal window, and execute `cd` by itself, you should find that your terminal window prompt resembles below:
		- `$`
- Next, execute
	- `wget https://cdn.cs50.net/2023/fall/psets/3/plurality.zip`
	- this will download a .ZIP called `plurality` into your codespace
- Then execute `unzip plurality.zip` to create a folder called `plurality`; you no longer need the ZIP, so you can execute: `rm plurality.zip`
	- respond with `y` followed by Enter at the prompt to remove the ZIP file you downloaded
- Now type: `cd plurality` followed by Enter to move yourself into that directory.
- Execute `ls` and you should see a file named `plurality.c`
	- executing `code plurality.c` should open the file where you will type your code for this problem set;
		- if not, retrace your steps and see if you can determien where you went wrong












    
    
#cs50/week/2/arrays
#cs50/week/2

# Week 2
## PSET 2



### INFO
#cs50/pset/2
1. Log into cs50.dev using your GitHub account
2. Run `update50` in your codespace's terminal window to ensure your codespace is up-to-date and, when prompted, click **Rebuild now**
3. Submit **Scrabble**
4. Submit **Readability**
5. Submit one of:
	1. **Caesar**, if feeling less comfrotable
	2. **Substitution**, if feeling more comfortable
If you submit both Caesar and Substitution, we'll record the higher of your two scores.

When to do it?
Tuesday, December 31, 2024 at 11:59 PM EST.


### PSET 2 - SUBSTITUTION
#### Problem to solve
- in a substitution cipher, we "encrypt" (i.e., conceal in a reversible way) a message by replacing every letter with another letter
	- to do we, we use a *key*
	- in this case, a mapping of each of the letters of the alphabet to the letter it should correspond to when we encrypt it
	- to "decrypt" the message:
		- the receiver of the message would need to know the key, so that they can reverse the processs: translating the encrypt text (generally called *ciphertest*) back into the original message (generally called *plaintext*)
- a key, for example, might be the string `NQXPOMAFTRHLZGECYJIUWSKDVB 
	- this 26-char key means that `A` (the first letter of the alphabet) should be converted into `N` ( the first character of the key), `B` (the second letter of the alphabet) should be converted 

#### submission
```c
#include <cs50.h>
#include <ctype.h>
#include <stdbool.h>
#include <stdio.h>
#include <string.h>

const int ALPHABET_SIZE = 26;

string crypto(string key, string input);

int main(int argc, string argv[])
{
    // If command line input is [place: 2]
    if (argc == 2)
    {
        string key = argv[1];

        // Check if key length is ALPHABET_SIZE
        if (strlen(key) == ALPHABET_SIZE)
        {
            // Array to keep track of which characters have been seen
            bool seen[ALPHABET_SIZE] = {false};

            // flag to check validity
            int isValid = 1;

            // Check each character
            for (int i = 0; i < ALPHABET_SIZE; i++)
            {
                if (!isalpha(key[i]))
                {
                    // not an alphabetical character
                    isValid = 0;
                    printf("Key must only contain alphabetic characters.\n");
                    return 1;
                    break;
                }
                else
                {
                    int c = toupper(key[i]) - 'A'; // Normalize character index to 0-25
                    if (seen[c])                   // if this evaluates true, which it does by default, then:
                    {
                        // character has been seen before
                        isValid = 0;
                        printf("Key must not contain repeated characters.\n");
                        return 1;
                        break;
                    }
                    else
                    {
                        seen[c] = true; // Mark this character as seen
                    }
                }
            }

            // provided the key evaluates as valid, run crypto
            if (isValid)
            {
                printf("Valid key.\n");

                string userinput = get_string("plaintext: ");

                string testphrase = crypto(key, userinput);

                printf("ciphertext: %s\n", testphrase);
            }
            else
            {
                printf("Invalid key. Ensure the key contains each letter exactly once.\n");
                return 1;
            }
        }
        else
        {
            printf("Invalid key length. Ensure the key is ALPHABET_SIZE characters long.\n");
            return 1;
        }
    }
    else
    {
        printf("Incorrect number of arguments. Please provide a single key.\n");
        return 1;
    }

    return 0;
}

// function to convert input string using provided key, to testphrase
string crypto(string key, string input)
{
    // initalize variables; create an array to base an index off of (alphaLower, alphaUpper)
    char keyC[ALPHABET_SIZE + 1] = {0};
    int inputLength = strlen(input);
    string user = input;
    int alphaLower[ALPHABET_SIZE];
    int alphaUpper[ALPHABET_SIZE];

    for (int i = 0; i < ALPHABET_SIZE; i++)
    {
        alphaLower[i] = 'a' + i;
        alphaUpper[i] = 'A' + i;
    }

    for (int i = 0; i < ALPHABET_SIZE; i++)
    {
        keyC[i] = toupper(key[i]);
    }

    for (int i = 0; i < inputLength; i++)
    {
        if (isalpha(user[i]))
        {

            if (isupper(user[i]))
            {
                // create "index" number to reference the letter's numerical position in the alphabet
                int index = user[i] - 'A';
                user[i] = toupper(keyC[index]);
            }

            if (islower(user[i]))
            {

                int index = user[i] - 'a';      // Get the index in the alphabet
                user[i] = tolower(keyC[index]); // Replace with the corresponding character from the key
            }
        }
    }

    return user;
}

// NQXPOMAFTRHLZGECYJIUWSKDVB
```

### PSET 2 - Readability

#### Problem to solve
According to Scholastic, E.B.White's *Charlotte's Web* is between a second and fourth grade reading level.
- Lowis Lowry's *The Giver* is between an eighth- and twelfth-grade reading level. 
- What does it mean though for a book to be at a particular reading level?

- In many cases, a human expert might read a book and make a decision on the grade (i.e., year in school) for which they think the book is most appropriate.
	- an algorithm could likely figure that out too!

- implement a program that calculates the approximate grade level needed to comprehend some text
	- your program should output `Grade X` where `X` is there grade level computed, rounded to the nearest integer
	- if the `grade level` is `16` or higher (equivalent to or greater than a senior undergraduate reading level), your program should output `Grade 16+` instead of giving the exact index number
	- if the grade level is less than 1, your program should output `Before Grade 1`

#### Background
- so what sorts of traits are characteristic of higher reading levels? 
	- longer words probably correlate with higher reading levels
	- likewise, longer sentences probably correlates with higher reading levels, too
- a number of "readability tests" have been developed over the years that define formulas for computing the reading level of a text
	- one such readability test is the *Coleman-Liau index*
		- this index is designed to output that (U.S) grade level that is needed to understand some text.
		- the formula is as follows:
		- `index = 0.0588 * L - 0.296 * S - 15.8`
			- where `L` is the average number of letters per 100 words in the text
			- `S` is the average number of sentences per 100 words in the text
#### Code journal
- Need to take a line of input (text)
- Need to make an index output in int
	- compute number of words
	- compute number of letters
	- compute number of sentences


### PSET 2 - SCRABBLE
#cs50/pset/2/scrabble


```c
int upper[i] = toupper(word[i]); // take the returned value of toupper
```

#### Problem to solve
In the game of Scrabble, players create words to score points, and the number of points is the sum of the point values of each letter in the word.

For example, if we wanted to score the word "CODE", we would note that the 'C' is worth 3 points, the 'O' is worth 1 point, the 'D' is worth 2 points, and the 'E' is worth 1 point. Summing these, we get that "CODE" is worth 7 points.

![[CleanShot 2024-01-21 at 17.48.51@2x.png]]

In a file called `scrabble.c` in a folder called `scrabble`, implement a program in C that determines the winner of a short Scrabble-like game. 

Your program should prompt for input twice: once for "Player 1" to input their word and once for "Player 2" to input their word. 

Then, depending on which player scores the most points your program should either print "Player 1 wins!", "Player 2 wins!", or "Tie!" (in the event that two players score equal points).

#### Advice and Hints
- write some code that you know will compile
- Write some pseudocode before writing more code
- Convert the pseudocode to code

#### Correctness
- in terminal run
	- `check50 cs50/problems/2024/x/scrabble`
- submit:
	- `submit50 cs50/problems/2024/x/scrabble`



#### Advice
- try out any of David's programs from class via Week 2's examples
- To see the manual pages for C functions, visit manual.cs50.io
- If you see any errors when compiling your code with `make` , focus first on fixing the very first error you see, scrolling up as needed. 
- If unsure what it means, try asking `help50` for help. 
- For instance, if trying to compile `readability` and `make readability` is yielding errors, try running `help50 make readability` instead!

#####
WALKTHROUGH:
```c
/* If unsure how to solve the problem itself, break it down into smaller problems that you can probably solve first. For instance, this problem is really only a handful of problems:

Prompt for the user for two words
Compute the score of each word
Print the winner
Let’s write some pseudcode as comments to remind you to do just that */

#include <ctype.h>
#include <cs50.h>
#include <stdio.h>
#include <string.h>

int main(void)
{
    // Prompt the user for two words

    // Compute the score of each word

    // Print the winner
}

/*

Some problems in problem sets, like this one, might contain spoilers (like the next one) that ultimately walk you through the entire solution. While you are permitted to use this code, we really do strongly encourage you to try things out yourself first! The other problems in the problem set won’t have this sort of walkthrough, and typically the problem that contains the “full spoiler” is a warm-up version of the bigger problem you’ll later need to tackle. 

*/
```

- First, consider how you might prompt the user for two words; recall that `get_string` a function in the CS50 library, can prompt the user for a string.

```c
#include <ctype.h>
#include <cs50.h>
#include <stdio.h>
#include <string.h>

int main(void)
{
	// Prompt the user for two words
	string word1 = get_string("Player 1: ");
	string word2 = get_string("Player 2: ");

	// Compute the score of each word

	// Print the winner
}
```

- Next consider how to compute the score of each word; since the same scoring algorithm applies to both words, you have a good opportunity for *abstraction*
	- here we'll define a function called `compute_score` that takes a string, called `word` as in put, and then returns `word`'s score as an `int`.

```c
#include <cstype.h>
#include <cs50.h>
#include <stdio.h>
#include <string.h>

int compute_score(string word);

int main(void)
{
	// Prmopt the user for 2 words
	string word1 = get_string("Player 1: ");
	string word2 = get_string("Player 2: ");

	// Compute the score of each word
	int score1 = compute_score(word1);
	int score2 = compute_score(word2);

	// Print the winner
}

int compute_score(string word)
{
	// Compute and return score for word
}
```

- now turn to implementing `compute_score`;
	- to compute the score of a word, you need to know the point value of each letter in the word
	- you can associate letters and their point values with an *array*
	- imagine an array of 26 `int`s, called `POINTS`, in which the 1st number is the point value for 'A', the second number is the point value for 'B', and so on
	- by declaring and initializing such an array outside of any single function, you can ensure this array is accessible to any function, including `compute_score`
```c
#include <ctype.h>
#include <cs50.h>
#include <stdio.h>
#include <string.h>

// Points assigned to each letter of the alphabet
int POINTS[] = {1, 3, 3, 2, 1, 4, 2, 4, 1, 8, 5, 1, 3, 1, 1, 3, 10, 1, 1, 1, 1, 4, 4, 8, 4, 10};

int compute_score(string word);

int main(void)
{
    // Prompt the user for two words
    string word1 = get_string("Player 1: ");
    string word2 = get_string("Player 2: ");

    // Compute the score of each word
    int score1 = compute_score(word1);
    int score2 = compute_score(word2);

    // Print the winner   
}

int compute_score(string word)
{
    // Compute and return score for word
}

```
- To implement `compute_score`, first try to find the point value of a single letter in `word`
	- recall that to find the character at the nth index of a string, `s`, you can write `s[n]`
	- `word[0]` for example will give you the first character of `word`
- Now recall that computers represent characters using 'ASCII', a standard that represents each character as a number
- Recall too that the 0th index of `POINTS`, `POINTS[0]`, gives you the point value of 'A'
	- Think about how you could transform the numeric representation of 'A' into the index of its point value
	- what about 'a'? You'll need to apply different transformations to upper-and lower-case letters, so you may find the functions `isupper` and `islower` to be helpful to you
	- keep in mind that characters that are not letters should be given zero points
		- for example, `!` is worth 0 points
- if you can properly calculate the value of *one* character in `words`, odds are you can use a loop to sum the points for the rest of the characters
	- once you've tried the above on your own, consider this (quite revealing!) hint below:
```c
#include <ctype.h>
#include <cs50.h>
#include <stdio.h>
#include <string.h>

// Points assigned to each letter of the alphabet
int POINTS[] = {1, 3, 3, 2, 1, 4, 2, 4, 1, 8, 5, 1, 3, 1, 1, 3, 10, 1, 1, 1, 1, 4, 4, 8, 4, 10};

int compute_score(string word);

int main(void)
{
    // Prompt the user for two words
    string word1 = get_string("Player 1: ");
    string word2 = get_string("Player 2: ");

    // Compute the score of each word
    int score1 = compute_score(word1);
    int score2 = compute_score(word2);

    // Print the winner   
}

int compute_score(string word)
{
    // Keep track of score
    int score = 0;

    // Compute score for each character
    for (int i = 0, len = strlen(word); i < len; i++)
    {
        if (isupper(word[i]))
        {
            score += POINTS[word[i] - 'A'];
        }
        else if (islower(word[i]))
        {
            score += POINTS[word[i] - 'a'];
        }
    }

    return score;
}
```
- finally, finish your pseudocode's last step: printing the winner
	- recall that an `if` statement can be used to check if a condition is true and that the additional usages of `else if` or `else` can check for other (exclusive) conditions
```c
#include <ctype.h>
#include <cs50.h>
#include <stdio.h>
#include <string.h>

// Points assigned to each letter of the alphabet
int POINTS[] = {1, 3, 3, 2, 1, 4, 2, 4, 1, 8, 5, 1, 3, 1, 1, 3, 10, 1, 1, 1, 1, 4, 4, 8, 4, 10};

int compute_score(string word);

int main(void)
{
    // Prompt the user for two words
    string word1 = get_string("Player 1: ");
    string word2 = get_string("Player 2: ");

    // Compute the score of each word
    int score1 = compute_score(word1);
    int score2 = compute_score(word2);

    // Print the winner   
    if (score1 > score2)
    {
        printf("Player 1 wins!\n");
    }
    else if (score1 < score2)
    {
        printf("Player 2 wins!\n");
    }
    else
    {
        printf("Tie!\n");
    }
}

int compute_score(string word)
{
    // Keep track of score
    int score = 0;

    // Compute score for each character
    for (int i = 0, len = strlen(word); i < len; i++)
    {
        if (isupper(word[i]))
        {
            score += POINTS[word[i] - 'A'];
        }
        else if (islower(word[i]))
        {
            score += POINTS[word[i] - 'a'];
        }
    }

    return score;
}
```

#### Code Journal
my initial entry before chat'gpt lol.
```c
#include <cs50.h>
#include <stdio.h>
#include <ctype.h>
#include <string.h>

// constant of 2 players
const int N = 2;

// prototype

// scrabble takes the string word then converts to uppercase, and scores it.
int scrabble(string word);

// main; take the input of users as "string", then plug into "scrabble"
int main(void)
{
    // intialize an array of strings of length "N" (2)
    string words[N];
    // arrays must be intialized with a "known" length, however syntax allows us to only specify 1 value for the timebeing.
    int scores[N] = {0};
    // also note we have 2 arrays, words and scores
    // receive input from players
    for (int i = 0; i < N; i++)
    {
        words[i] = get_string("Player %i: ", i + 1);
        scores[i] = scrabble(words[i]);
    }

    if (scores[0] > scores[1])
    {
        printf("Player 1 wins!\n");
    }
    else if (scores[1] > scores[0])
    {
        printf("Player 2 wins!\n");
    }
    else
    {
        printf("Tie!");
    }

    // printf("Player 1: %s, Player 2: %s\n", words[0], words[1]);
    // printf("Player 1: %i, Player 2: %i\n", scores[0], scores[1]);


}

int scrabble(string word)
{
    // convert each letter to lowercase, then calculate score
    int score = 0;
    // setting scoringsystem array 'system' to cs50's provided system (A = 1 pt, B = 3 pt)
    int system[26] = {1, 3, 3, 2, 1, 4, 2, 4, 1, 8, 5, 1, 3, 1, 1, 3, 10, 1, 1, 1, 1, 4, 4, 8, 4, 10};

    // toupper 'for' loop
    for (int i = 0, n = strlen(word); i < n; i++)
    {

        // conversion of each word's char to uppercase
        int a = toupper(word[i]); // take the returned value of toupper
        int sx = a - 64 - 1;

        if (a <= 64 || a >= 91 )
        {
            score += 0;
        }

        else
        {
        score += system[sx];
        }
    }

    return score;
}



// }




// // prototype

// // implement a function that takes the string input
// int scrabble(string x, string y);

// // main; take the input of users as "string", then plug into "scrabble"
// int main(void)
// {
//     string a = get_string("Player 1: ");
//     string b = get_string("Player 2: ");
//     scrabble(a, b);
//     printf("Player 1: %s, Player 2: %s\n", a, b);
// }

// int scrabble(string a, string b)
// {



// }

/*


> get_string x 2
    > (i need 2 strings, player a, player b)

> somewhere, i need to dictate the array's positioning
>> so points[0] (position "A" or 0 = 1, Position "B" = 3)
>>> simultaneously, i need to dignify CAPS == LOWER totals
    ..... add or minus 32 depending on what case it is, there should be a unified point total
    if char[0] (say the word is "at")
    >>>> if (char[0] == A || a)
        {
            char[0] = 1
        }
>> can i reference multiple arrays? array for the original point total, array for string.
> i could calculate the strlen?
    > this would be used for identifying how many times to
    iterate
        > as in, for x char in the array(string) do i check the points for
            > after evaluating the strlen, running the for loop, += (pts)
            i < strlen; i++
            {
                > depending on what char pos you're evaluating for, return the pts value
                sum += 0
            }

*/
```
final code:
```c
#include <cs50.h>
#include <ctype.h>
#include <stdio.h>
#include <string.h>

// Setting constants.

// Number of players
const int N = 2;
// Scrabble letter scores
const int SCORES[26] = {1, 3, 3, 2, 1, 4, 2, 4, 1, 8, 5, 1, 3, 1, 1, 3, 10, 1, 1, 1, 1, 4, 4, 8, 4, 10};

// Protoyping
int scrabble(string word);

// Main function (query players, initialize 'scrabble')
int main(void)
{
    string words[N];
    int scores[N] = {0};

    // based on N players, run the below 'for' loop 'N' (2) many times.
    for (int i = 0; i < N; i++)
    {
        words[i] = get_string("Player %i: ", i + 1);
        scores[i] = scrabble(words[i]);
    }

    // determining who wins:
    if (scores[0] > scores[1])
    {
        printf("Player 1 wins!\n");
    }

    else if (scores[1] > scores[0])
    {
        printf("Player 2 wins!\n");
    }

    else
    {
        printf("Tie!\n");
    }
}

int scrabble(string word)
{
    // initializing 'score' variable
    int score = 0;

    // iterate through word length, based on strlen as a limit
    for (int i = 0, n = strlen(word); i < n; i++)
    {
        // check if character is an alphabetical character (no numbers or symbols)
        if (isalpha(word[i]))
        {
            // if alphabetical character, convert to uppercase (if needed) and add to 'score' variable using SCORES as an index.
            // note the 'A' is using the int value of "A" as a character (65)

            int letterIndex = toupper(word[i]) - 'A';
            score += SCORES[letterIndex];
        }
    }

    return score;
}

```
## Notes
### Compiling
#cs50/week/2/compiling
- compilers have acted as an abstraction

source code > compiler > machine code

- bottom-up understanding of what it could be
- hello world:
```C
#include <stdio.h>

int main(void
		{
			printf("Hello world\n".)
		})
```
- make itself is not a compiler, but a program that runs the compler for you
- runs a program called `clang`
- if i just run clang hello.c
	- `bash: there is no such file or directory`
	- running ./a.out actually works
- we can configure clang with command line arguments that somehow modify the behaviour of a program
	- create a program called hello...
	- `clang -o hello hello.c`
	- second version with the
```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
	string name = get_string("What's your name? ");
	printf("hello, %s\n", name);
}
```

- error once running `clang -o hello hello.c`
	- linker command failed...
- when using a 3rd party library, you must use an additional command line argument
- `clang - o hello hello.c -lcs50`
- make has been inputting these command line arguments for you
- compiling is 1 of 4 steps that get involved in turning binary into outputs

### Preprocessing
- Preprocessing
	```c
	#include <cs50.h>
	int main(void)
	{
		for (int i = 0; i < 3; i ++)
		{
			meow();
		}
	}
void meow(void)
{
	printf("meow\n");
}
	```
- The hashcodes for the library inclusions can be thought of as `pre-processing directives`
- The code gets compiled into something called assembly language
	- people wrote in assembly used this!
		- people before that used machine code, be it in code or punch cards
	- compiler > assembly language
		- printf, get_string are referenced in the assembly code
		- ![[Pasted image 20240113204606.png]]
- clang names the output file `a` because "assembly"
- compiling
	- clang, assembly code
- assembling
	- assembly code into 1's and 0's
- linking
	- takes all of the 0s and 1s of the 3 files intelligently into one single file
	- - there are 3 different files:
	- `cs50.h`
	- `stdio.h`
	- `.c`
	- so:
		- hello.c 
		- cs50.c
		- stdio.c
	- Clang must compile all of these into the corresponding 0s and 1s
		- the 0s and 1s must be linked together.
	- i guess taking source code and libraries?

- Compiling: process of taking our source code which takes your source code into assembly code
	- other compoiles?
		- Gcc: GNU COMPILER COLLECTION
		- clang is somewhat more recent
			- the error messages are more user-friendly
		- alternatives to compiling exist
- implications of source code into machine code?
	- maybe you can decompile it?
	- go from 0s and 1s into source code?
		- change something in another person's code
		- then people with MacS and PCs can take all of that and reverse engineer it
- Reverse engineering
	- easier said then done
	- Loops alone
		- for
		- while
		- do-while
	- typically lose the variable and function names typically
	- generally the midnset is if you're good to decomplile it, 
		- could implement?
	- having techniques 
### Debugging
#cs50/week/2/debugging
- going through Mario program
	- ![[CleanShot 2024-01-13 at 21.03.07.png]]
		- seeing 4 hashes in a line, I'm looking for only 3
		- `printf` technique
			- `printf("i is %i\n", i)`
			- ![[CleanShot 2024-01-13 at 21.05.39.png]]
			- ![[CleanShot 2024-01-13 at 21.05.56.png]]
	- The canonical solution here would be to change the `<=` to `<`
		- now the code matches expectations
		- remember to remove the debugging lines of code
		- this can devolve into trial and error
	- if you ever find yourself slipping down the hill, better to use something other than `printf`
- next example:
	- ![[CleanShot 2024-01-13 at 21.11.37.png]]
	- ![[CleanShot 2024-01-13 at 21.11.46.png]]
	- the initial issue is that the prototype is missing
	- let's look at a debugger
		- `debug50`
		- the debugger needs your code to already be compiled
		- `debug 50 ./buggy`
	- What you need to run debugger
		- breakpoints
			- where do you want to pause?
			- might have accidentally this
			- the red dot in the gutter of VS Code
				- "gutter" is the space behind the line numbers
			- setting break point = red dot
			- set the breakpoint on the first line of executable code
				- highlighted in yellowish code
			- showcases variables
				- locals
					- `h` in this example, shows a "garbage value"
						- h: 21912
					- call stack feature
			- below, there is a `WATCH` section
				- `call stack`
				- `main()`
				- hitting the play button will run from start to end
			- Step into, poke around the contents of `get_int`
			- these buttons are going to be our friends
			- "Step over"
				- doesn't mean skip
				- means execute, but don't bother me by going into the weeds
				- ![[CleanShot 2024-01-13 at 21.54.50.png]]
			- typing in "3"
				- the variable `h` should take on the value of 3
				- we will see the "locals h" update to 3
				- Output would have outputted 4 hashes
			- I want to see what happens in `print_column(h)`
			- yellow highlight now jumps into the function
			- comparing i against height, you should be able to see this
			- height is still 3, but i keeps incrementing
		- clicking "Step Over"
			- notice that 3 is less than or equal to height, and that means the hash is still printing
		- can fix code and now it should work!


```c
#include <cs50.h>
#include<stdio.h>
#include <string.h>

int main(void)
{
	string name = get_string("Name: ");
	int length = strlen(name);
	printf("%i\n", length);
}
```
- within cs50.h one line of code that defines return value, the function name and the arguments for get_string
#cs50/week/2/libraries

### Command-line Arguments (argc, argv[])

```c
int main(int argc, string argv[])
{
	...
}
```
- the above code is an example of an alternative to `int main(void)`

- `command-line arguments` are those arguments that are passed to your program at the command line;
	- all those statements you typed after `clang` are considered command line arguments
	- you can use these arguments in your own programs
- in your terminal window, type `code greet.c` and write code as follows:
```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
	string answer = get_string("What's your name? ");
	printf("hello, %s\n", answer);
}
```
- notice that this says `hello` to the user
- still, would it not be nice to be able to take arguments before the program even runs?
```c
#include <cs50.h>
#include <stdio.h>

int main(int argc, string argv[])
{
	if (argc == 2)
	{
		printf("hello, %s\n", argv[1]);
	}
	else
	{
		printf("hello, world\n");
	}
}
```
- notice that this program knows both `argc`, the number of command line arguments
	- and `argv` which is an array of the characters passed as arguments at the command line
- therefore, using the syntax of this program, executing `./greet David` would result in the program saying `hello, David`.
#cs50/week/2/command-line-arguments/argc #cs50/week/2/command-line-arguments/argv 

### Exit status
#cs50/week/2/command-line-arguments/exitstatus
- one other thing about the code we've been writing
- all programs eventually exit because we see that they exit
- every program has an "exit status"
	- by default, it will always be 0
- every other number in the world, than 0, it's bad
- "An unknown error occurred"
	- Error code: 1132
		- zoom, somehow had an error, and returned "1132"
		- some programmer at Zoom might have understanding of what is happening here
	- error code: 404
		- means file not found, webpage not found
		- numbers that represent errors
		- not necessarily an exist status
- this means that the `int main(int argc, string argv[])` lines will always return some kind of `int`
	- our programs can return values
- every program exited with `0` as the exit status/return value by default
- this is useful inherently to troubleshooting/class QA
- can use `echo $?` to see the return value of the last program ran
- get in the habit of automatically testing your code
	- these test can detect the status codes, knowing that your code succeeds or fails
- goal of this week: is to solve problems
### Cryptography
#cs50/week/2/arrays/cryptography
- cryptography is the art of the science of encrypting
	- encrypting: scrambling information
	- ability to send info securely
	- whether it is in file-format or wirelessly
- if i look at this message and encrypted, no one can see but me and the recipient
- plain text > ? > ciphertext --> plaintext > cipher > ciphertext
- ciphertext is what you want to convert into..
- cipher: algorithm for encrypting or scrambling information in a reversible way
- ciphers take as input not only the plaintext message in English, but also a key
- metaphorically, keys open locks
	- but its generally a big number
		- 32 bit, 64 bit, 1024 bits
	- for all intents and purposes you are secure
- Let's say for `HI!`
	- our secret is going to be `1`
		- `1` is my key, in this example
		- `HI!` > `IJ`
			- effectively incr. alphabet +1 char
			- get to Z, wrap back to A
				- shift the alphabet by +1 place
			- you look at this, then the `!` leaks
			- won't know the message unless decrypted
		- this message is not that secure
		- you could "brute-force" the solution
			- eventually it's going to pop-out
		- this is called the "Caesar" cipher
		- key of 3
			- first person by lore to have thought of this idea!
		- key of 13
			- "ROT 13"
				- HI! > UV!
				- I LOVE YOU > V YBIR LBH
		- key of 26
			- "ROT 26"
			- doesn't always help you...
			- I LOVE YOU > I LOVE YOU
	- Decryption
		- using a key of 1
			- just minus 1!
			- key of 13?
				- subtract 13
	- `UIJT XBT DT50`
		- let's decrypt using `-1`
		- this is what the lightbulbs in the video were spelling out
		- `THIS WAS CS50`
			- grab your own rubber duck lmao.
	- 




#cs50/week/2 


### More on Debugging
#cs50/week/2/debugging #cs50/week/2/debugging/debug50


- guess program
- guess wrong, infinite output flood


```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
	int number = 5;

	int guess = get_int("What's your guess? ");

	while (guess != number)
	{
		printf("Wrong guess!");
	}
	
	printf("You're correct!");
}
```
- go to left side and see the red circles pop up to turn it bright red
- use debug50 to step through the code
- code opened up in `debug50`
	- my `number `variable is set 0
	- my `guess` is 32764
- `number` set to 5 as intended
- `guess` becomes 3
- if my `guess` is wrong and `number` not equal
	- shouldn't I just get Wrong guess?
- go through code with "step over"
- line in yellow highlight goes back to lkne 10
	- it goes through again and prints "wrong guess"
- i should've used an if statement that checks **once** rather than indefinitely with *while*
- however, now it's returning also `You're correct`


```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
	int number = 5;

	int guess = get_int("What's your guess? ");

	if (guess != number)
	{
		printf("Wrong guess!");
	}
	else
	{
		printf("You're correct!");
	}
}


```

#cs50/week/2/debugging

- secret program
	- type in the right password, you should see "come right in"
- the password is "please"
- but we don't see anything...

```c
#include <cs50.h>

bool check_phrase(string);

int main(void)
{
	string phrase = get_string("What's the secret phrase? ");

	bool correct = check_phrase(phrase);

	if (correct == true)

}

bool check_phrase(string phrase)
{
	string password = "Please";
	// 0 means true
	if (strcmp(phrase, password == 0)
	{
		return true;
	}

	return false;
}
}

```

- set a break point, or p lace t opause in our code
- find a pale red circle, click it, set it (carter sets it to line 9)
- i can run `debug 50 secret`
	- code should boot up and pause right at that moment
	- need to step over to actually run that part of the program
- `phrase` becomes 0xd136b0 "Please"
	- `correct` stays false
	- so the print does not come in
- haven't seen what's wrong in my code..
- let's step into a function
	- run it again, let's dive into the function
	- stepping into this, i can see that `phrase == password` is true
	- HOWEVER you can't compare 2 strings with equal signs
	- you need `strcmp`, which i've added in.
- make secret again
	- run secret, type in password of `please`
	- now it says `come on in`
	- mission success!


#cs50/week/2/arrays 
### Arrays 

- arrays are a fundamental **data structure**, and they are extremely useful
- we use arrays to hold values of the same type at contiguous memory locations
- one way to analogize the notion of array is to think of your local post office, which usually has a large bank of post office boxes.

- ==Arrays==
	- an **array** is a block of contiguous space in memory
	- which has been partitioned into small, identically-sized blocks of space called **elements**
	- each of which can store a certain amount of **data**
	- all of the same data type such as **int** or **char**
	- and which can be accessed directly by an **index**
- ==Post office boxes==
	- a **mail bank** is a large space on the wall of the post office
	- which has been partioned into small, identically-sized blocks of space called **post office boxes**
	- each of which can hold a certain amount of **mail**
	- all of a similar type such as **letters** or **small packages**
	- and which can be accessed directly by a **mailbox number**

- in C, the elements of an array are indexed starting from 0
	- this is one of the major reasons we count from zero
- if an array consists of *n* elements, the first element is located at index 0
	- the last element is located at `(n-1`
- C is very lenient.
	- it will not prevent you from going "out of bounds" of your array; be careful!
		- might get a `segmentation fault`
- Array declarations
- #cs50/week/2/arrays/declaration
- `type name[size];`
	- the `type` is what kind of variable each element of the array will be
	- the `name` is what you want to call your array
	- the `size` is how many elements you would like your array to contain.
		- e.g. how many integers do you want?
	- `int student_grades[40];`
		- declares array called `Student_grades` consisting of `40` integers
	-  `double menu_prices[8]`
		- declaers array with room for 8 doubles
- if you think of a single element of an array of type `data-type` the same as you would any other variable of type `data-type` (which ,effectively, it is) then all the familiar operations make sense.
```c
bool truthtable[10];

truthtable[2] = false;
if(truthtable[7] == true;)
{
	printf("TRUE!\n");
}
truthtable[10] = true;
```
- `truthtable[2]` is the 3rd entry (remember counting from 0)
- `truthtable[7] == true`
	- this is the 8th, remember!
- `truthtable[10]`, this is** higher** than the highest index, which is the** 9th** number
	- again think n-1
	- this is legal, but if something exists in memory where 10 is,
		- might suffer **segmentation fault**
- when declaring and initializing an array simultaneously, there is a special syntax that may be used to fill up the array with its starting values
- #cs50/tip
```c
// instantiation syntax
bool truthtable[3] = { false, true, true };

// OR
bool truthtable[] = { false, true, true };

// individual element syntax
bool truthtable[3];
truthtable[0] = false;
truthtable[1] = true;
truthtable[2] = true;
```
- very good at home exercise to practice `loops` with arrays
- notice above that in `instantiation syntax` that you don't need to declare how many elements there are
- arrays can consist of more than a singel dimension
	- you can have as many size specifiers as you wish.
`bool battleship[10][10];`
- you can choose to think of this as either a 10x10 grid of cells
	- in memory though, it's really just a 100-element one-dimensional array
	- multi-dimensional arrays are great **abstractions** to help visualize game boards or other complex representations.
		- instead of having to think of tic tac toe or battleship as a single-line (thinking about this concept in technicalities)
- while we can treat individual elements of arrays as variables ,we cannot treat entire arrays themselves as variables
- we cannot for instance assign one array to another using the assignment operator
	- that is not legal C
- instead we must use a loop to copy over the elements one at a time.

```c
// this doesn't work
int foo[5] = { 1, 2, 3, 4, 5 };
int bar[5];

bar = foo;

// try this

int foo[5] = { 1, 2, 3, 4, 5 };
int bar[5];

for(int j = 0; j < 5; j++)
{
	bar[j] = foo[j];
}

```

- one thing that's tricky: variable scope, most variables are `passed-by-values`
	- (means we're making a copy)
	- the callee doesn't get its own variables, but its own *copy*
- arrays do not follow this rule
	-  #cs50/week/1/scoping 
	- arrays are **passed by reference**
	- the callee receives the actual array, ==not a copy of it==
		- what does that mean when the callee manipulates elements of the array?
	- for now, we'll gloss over why arrays have this special property, but we'll return to it soon enough.
	- making copies is not worth it for the function because arrays are so bulk and cumbersome
		- we trust the function to not break anything
		- it gets the full array, not the global copy of it

```c

void set_array(int array[4]);
void set_int(int x);

int main(void)
{
	int a = 10;
	int b[4] = { 0, 1, 2, 3 };
	set_int(a);
	set_array(b);
	printf("%d %d\n", a, b[0]);
}

void set_array(int array[4])
{
	array[0] = 22;
}

void set_int(int x)
{
	x = 22;
}

// should return 10, 22
```

- functions retain a COPY
- arrays provided the full-set
	- this is why `set_int(a)` is ran (presumably given "22", but `a` returns 10
		- the functions copy of `a` or `x`, in this instance, is local to itself `set_int(int x)`
	- `int b[4]` and `set_array(b)` on the other hand;
		- because `set_array(int array[4]` assigns `b` the return value of `array[0] = 22;`
		- `b` = 22

```c
#include <cs50.h>
#include <stdio.h>

void set_array(int array[4]);
void set_int(int x);


int main(void)
{

	int a = 10;
	int b[4] = { 0, 1, 2, 3 };
	set_int(a); // This will not change 'a' in main
	set_array(b); // This will change b[0] to 22
	printf("%d, %d\n", a, b[0]);
}

  

void set_array(int array[4])
{
	array[0] = 22; // This changes b[0] in main to 22
}

  

void set_int(int x)
{
	x = 22; // This changes x locally but not 'a' in main
}
```


#cs50/week/2/casting

- in C, `char` is essentially a small integer type
- integral data type, stored as a number
- the `%i` or `%d` implicitly converts the `char` to `int` and displays the integer (ASCII value) of the character
	- this is a form of "integer promotion"
- this might only apply to printing
- doing arithemtic or other operations where type needs to be explictly known or controlled
	- you might need to cast the `char` to an `int` or another type explicitly

- strings = arrays of `char` terminated by a null character (`'\0'`)
	- where there are no implicit conversions or promotions for arrays (including strings) are there for single 'chars'
		- there are several conventions and functions int he standard library specifically designed to work with strings (null-terminated `char` arrays)
		- here are a few key points and common operations
1. **string literals:**
	1. when you initialize a `char` array with a string literal, like `char str[] = "Hello";`
		1. C automatically appends the null terminator `'\0'` at the end of the array, marking the end of the string
2. **Printing Strings:**
	1. You can print an entire string using `%s` in `printf`.
		1. it will print characters starting from the beginning of the `char` array until it hits the null terminator.
3. **String Handling Functions**
	1. the C standard library (<string.h>) provides many functions for string manipulation and querying
		1. `strlen` , get the length of a string (non-counting null terminator)
		2. `strcpy` copy one string to another
		3. `strcat` concatenate two strings
		4. `strcmp` compare two strings
	2. these functions expect C strings to be null-terminated
		1. they rely on the null terminator to know where the string ends
4. **Passing Strings to Functions:**
	1. while passing strings to functions, you usually pass a pointer to the first element of the `char` array (e.g. `char* str`) 
	2. inside the function, you can treat this pointer just like an array, thanks to pointer arithmetic and the way C handles arrays and pointers
5. **No bounds checking**
	1. C does not perform automatic bounds checking on arrays
	2. when you're working with strings, it's your responsibility to ensure that you don't access out of bounds and that strings are properly null-terminated
6. **Buffer Overflows:**
	1. Be cautious with functions that write to strings (like `strcpy` and `strcat`)
	2. if the destination array is not large enough to hold the result, it can lead to buffer overflows, a common source of bugs and security vulnerabilities
	3. functions like `strncpy` and `strncat` allow you to specify the maximum number of characters to copy/concatenate and can be safer alternatives

#cs50/week/2/arrays 
- C needs to know 3 things for an array
	- name of the array
	- size of the array
	- what kind of data we're storing
- C only stores 1 kind of datatype
	- integers!
- the counting of an array when laid out like this
	-  `int nights[5]`
	- not zero-indexed when written
	- however, when ACCESSED, it is zero-indexed
		- (wtf?)
	- int nights[5] = {7, 8, 6, 7, 8};


Questions
- Can you change the size of an array?
	- you cannot change the size of an array
		- you can allocate more memory later
		- copy from 1 space of memory, into another space
- can an array exist on multiple planes, like a 3d array?
	- you can have arrays within arrays (each array's element is another array)
		- you can iterate on this
		- this results in a 3d kind of structure
- what about negative -1 indexes?
	- programmed in python?
		- `[-1]` for example
	- this is not possible in C
- for `int nights[5]`, could we add only three and later on add the other 2?
	- at the declaration, specify 3 values, then the other 2
	- the final 2 values, or the unspecified values, can be anything.
	- however this only goes for `int nights[5] = {7, 8, 6, 7, 8};`

**Doubling up**
#cs50/week/2/arrays #cs50/week/2/arrays/strings/example
- create an array of integers in where each integer is 2 times the value of the previous integer
- the first element is 1.
- print the array, integer by integer
- e.g. `{1, 2, 4, 8, 16}`

- a `for` loop is good for when we know overall how many times we want to iterate
```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    int size = 8;
    int sequence[size];

    sequence[0] = 1;
    printf("%i\n", sequence[0]);

    for (int i = 1; i < size; i++)
    {
            sequence[i] = sequence[i - 1] * 2;
            printf("%i\n", sequence[i]);
    }
}
```
- although it is not good practice to invoke "magic numbers"
	- in arrays, it may actually be valid, like in the above example, as it allows us to easily change/specify the array values.
	- (so you don't have to change it everywhere)
	- we can even take user input:
```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    int size = get_int("Enter a size: ");
    int sequence[size];

    sequence[0] = 1;
    printf("%i\n", sequence[0]);

    for (int i = 1; i < size; i++)
    {
            sequence[i] = sequence[i - 1] * 2;
            printf("%i\n", sequence[i]);
    }
}
```

### Strings
#cs50/week/2/arrays/strings
- strings work the same way, almost like scrabble?
- strings are nothing more than arrays where elements are characters
- HELLO
	- `phrase[0] = H, [1] = E....`
	- ASCII, we can see how chars map to each integer
		- `A' = 65
	- Consider `H E L L O` again
		- `{72, 69, 76, 76, 79}`

#cs50/week/2/arrays/strings/example 
Alphabetical
- write a program to check if an array of characters is in alphabetical order or not
- Assume the characters are all upper case

what can we do to loop through the number of characters in a string?
- for loop through the string, since string = array!
- if i'm checking this condition, it is not worth it to run like this
- ![[CleanShot 2024-01-19 at 22.25.01.png]]
- how can we sort this stuff alphabetically?
- ![[CleanShot 2024-01-19 at 22.29.21.png]]
	- you can see that the integers +1 each time...
	- remember that these values are CASE SENSITIVE
- Looking at `B`, the integer is greater than `A`
- If these characters are not in alphabetical order... print `Not in alphabetical order\n`?
#cs50/tip
- you can input `return 0;` to state that your program has finished
- looking at the code again:
	- ![[CleanShot 2024-01-19 at 22.35.13.png]]
	- we don't want to be checking values outside of our memory...
	- we can see that `i` encroaches on the boundaries, in this case, `strlen(phrase)`
	- this also surprisingly will not work:
		- ![[CleanShot 2024-01-19 at 22.36.40.png]]
		- this only really looks at the first 2 characters, rather than the full string
		- need to print f at the END!
		- (I really gotta think about what I'm doing before I move on...)
questions
### Command line arguments
#cs50/week/2/command-line-arguments
- you may have seen several programs like this before
- if i type in `make alphabetical`
	- i am feeding it some kind of values..
- you can compare this with `check50`
	- you can see, without any parameters, output with information
- can we give, for `mario`, that the user can give some arguments *before* the program even runs?
- in order to have this `$ ./mario 8`
	- `int main (int argc, int argv[])`
	- compared to
	
```c
int main(void)
{
	int height = get_int("H: ");
}
```

```c
int main(int argc, string argv[])
{

}
```

argv
- write a program that prints each command-line argument given to the program
- `argc` refers to the number of arguments that my program received
	- number of inputs, including the name of the program itself
	- `$ ./mario 8` is actually 2 inputs
		- mario, and 8
		- you can see this in action with this
		- ![[CleanShot 2024-01-19 at 22.46.54.png]]
- can be stored in our `argv` as a set of strings
QUESTIONS
- argv is storing a collection of strings, 
- what if we wanted a number?
- allow the user to do `./mario 8` and run the program!
	- we should look in `argv[1]`
- ![[CleanShot 2024-01-19 at 22.49.06.png]]
- not able to store a string inside of this variable was an integer
- have to convert `argv[1]` to an int
- We can do this using the library `stdlib.h`
	- `atoi` will initiate the conversion
- the code runs without errors!
	- ![[CleanShot 2024-01-19 at 22.50.29.png]]
- what if i run like this `$ ./mario`
	- this will result in segmentation fault
	- why would we be looking outside of the bounds of `argv`
	- `argv[1]` assumes we have a bracket of 1
		- therefore to correct, we could try `argv[0]`
	- I also want to tell my user that if there is no input, that they should enter an argument.
	- ![[CleanShot 2024-01-19 at 22.53.47.png]]
- `code argv.c`
`argc` the # of inputs for our program
`argv` the array of inputs to our program at the command line

- what counts as being at the command line
	- `./mario 8 carter`
	- we can also look at `cowsay -f dragon "RAWR"
		- in one single command i ran `cowsay`, then `"RAWR` for what the dragon should be saying

### More Array Notes
- Compiling
	- encryption is the act of hiding plain text from prying eyes
		- decrypting is the act of taking an encrypted piece of text and returning it to a human-readable form
	- recall that last week you learned about a compiler
		- specialized computer program that converts source code into machine code that can be understood by a computer
		- for example you might have a computer program that looks like this
```c
#include <stdio.h>

int main(void)
{
	printf("hello, world\n");
}
```
- a compiler will take the above code and turn into machine code, binary blocks!
-  *VS Code* the programming environment provided to you, utilizes a compiler called `clang` or *c language*
- if you were to type `make hello`, it runs a command that executes clang to create an output file that you can run as a user
- VS Code has been pre-programmed such that `make` will run numerous command line arguments along with clang for your convenience as a user.
- consider the following code:
```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
	string naem = get_string("What's your name? ");
	printf("hello, %s\n", name);
}
```
- you can attempt to enter into the terminal window: `clang -o hello hello.c`
	- you will be met by an error that indicates that clang does not know where to find the `cs50.h` library
- attempting to compile this code, run the following command in the terminal window:
	- `clang -o hello hello.c -lcs50`
	- this will enable the compiler to access the `cs50.h` library
	- running in the terminal window `./hello`, your program will run as intended
	- While the above is offered as an illustration, such that you can understand more deeply the process and concept of compiling code, 
		- using `make` in CS50 is perfectly fine and the expectation
	- Compiling involves major steps (notes above.)
- 1. *Preprocessing* is where the header files in your code are effectively copied and pasted into your file
	- during this step, the code from `cs50.h` is copied into your program
	- similarly, just as your code contains `#include <stdio.h>` code contained within `stdio.h` somewhere on your computer is copied to your program
```c
string get_string(string prompt);
int printf(string format, ...);

int main(void)
{
    string name = get_string("What's your name? ");
    printf("hello, %s\n", name);
}
```
 - 2. *compiling* is where your program is converted into assembly code. this step can be visualized as follows:
	 - ![[CleanShot 2024-01-19 at 23.34.36.png]]
- 3. *assembling* involves the compiler converting your assembly code into machine code; this step can be visualized as follows:
		- ![[CleanShot 2024-01-19 at 23.35.06.png]]
- 4. *linking*: code from your included libraries are converted also into machine code and combined with your code
	- the final executable file is then outputted
		- ![[CleanShot 2024-01-19 at 23.35.56.png]]
### Debugging
- everyone will make mistakes while coding
- basically
	- rubber duck
	- break points
	- debug50, win the game.

### Arrays
- in week 0, we talked about *data types* such as `bool`, `int`, `char`, `string`, etc.
- each data type requires a certain amount of system resources:
	- `bool` 1 byte
	- `int` 4 bytes
	- `long` 8 bytes
	- `float` 4 bytes
	- `double` 8 bytes
	- `char` 1 byte
	- `string` ? bytes
- inside of your computer, you have a finite amount of memory available
- Physically on the memory of your PC, you can imagine how specific types of data are stored on your computer
	- you might imagine that a `char` only requires 1 byte of memory may look as follows:
		- ![[CleanShot 2024-01-19 at 23.38.20.png]]
	- similarly, an `int`, which requires 4 bytes might look as follows:
		- ![[CleanShot 2024-01-19 at 23.38.34.png]]
	- we can create a program that explores these concepts.
		- inside your terminal, type `code scores.c` and write code as follows:
```c
#include <stdio.h>

int main(void)
{
	// Scores
	int score1 = 72;
	int score2 = 73;
	int score3 = 33;

	// Print average
	printf("Average: %f\n", (score1 + score2 + score3) / 3.0);
}
```
- notice that the number on the right is a floating point value of `3.0` such that the calculation is rendered as a floating point value in the end
- running `make scores`, the program runs
- You can imagine how these variables are stored in memory:
	- ![[CleanShot 2024-01-19 at 23.40.31.png]]
- *Arrays* are a way of storing data back-to-back in memory such that this data is easilya ccesible
- `int scores[3]` is a way of telling the compiler to provide you 3 back-to-back places in memory of size `int` to store 3 `scores`
	- considering our program, you can revise your code as follows:
```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
	// Get Scores
	int scores[3];
	scores[0] = get_int("Score: ");
	scores[1] = get_int("Score: ");
	scores[2] = get_int("Score: ");

	// Print average
	printf("Average: %f\n", (scores[0] + scores[1] + scores[2] / 3.0);
}
```
- notice that `score[0]` examines the value at this location of memory by `indexing into` the array called `scores` at location `0` to see what value is stored there
- You can see how while the above code works, there is still an opportunity for improving our code. Revise your code as follows:
```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
	// Get scores
	int scores[3];
	for (int i = 0; i < 3; i++)
	{
		scores[i] = get_int("Score: ");
	}

	// Print average
	printf("Average: %f\n", (scores[0] + scores[1] + scores[2]) / 3.0);
}
```
- notice how we can index into `scores` by using `scores[i]` where `i` is supplied by the `for` loop
- We can simplify or *abstract away* the calculation of the average. See below:

```c
#include <cs50.h>
#include <stdio.h>

// Constant
const int N = 3;

// Prototype
float average(int length, int array[]);

int main(void)
{
    // Get scores
    int scores[N];
    for (int i = 0; i < N; i++)
    {
        scores[i] = get_int("Score: ");
    }

    // Print average
    printf("Average: %f\n", average(N, scores));
}

float average(int length, int array[])
{
    // Calculate average
    int sum = 0;
    for (int i = 0; i < length; i++)
    {
        sum += array[i];
    }
    return sum / (float) length;
}

```

- Notice a new function called `average` has been declared
	- further, notice a `const` or constant value of `N` is declared
	- most importantly, notice how the `average` function takes `int array[]`, which means that the compiler passes an array to the function
- not only can arrays be containers: they can be passed between functions
### Strings
- a `string` is simply an array of variables of type `char`: an array of characters
- considering the following image, you can see how a string is an array of characters that begins with the first character
	- it also ends with a special character called a `NUL character:`
		- ![[CleanShot 2024-01-21 at 16.51.41@2x.png]]
	- imagining this in decimal, your array would look like the following:
		- ![[CleanShot 2024-01-21 at 16.52.04@2x.png]]
- Implementing this in your own code, type `code hi.c` in the terminal window and write code as follows:
```c
#include <stdio.h>

int main(void)
{
	char c1 = 'H';
	char c2 = 'I';
	char c3 = '!';

	printf("%c%c%c\n", c1, c2, c3);
}
```
- notice that this will output a string of characters
- Similarly, make the following modification to your code:
```c
#include <stdio.h>

int main(void)
{
	char c1 = 'H';
	char c2 = 'I';
	char c3 = '!';

	prinf(%i %i %i\n", c1, c2, c3);
}
```
notice that ASCII codes are printed by replacing `%c` with `%i`

To further understand how a `string` works, revise your code as follows:
```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
	string s = "HI!";
	printf("%c%c%c\n", s[0], s[1], s[2]);
}
```

As before, we can replace `%c` with `%i` as follows:
```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
	string s = "HI!";
	printf("%i %i %i %i\n", s[0], s[1], s[2], s[3]);
}
```

Let's imagine we want to say both `HI!` and `BYE!`, modify your code as follows:
```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
	string s = "HI!";
	string t = "BYE!";

	printf("%s\n", s);
	printf("%s\n", t);
}
```
notice that 2 strings are declared and used in this example.

- you can visualize this as follows:
	- ![[CleanShot 2024-01-21 at 16.59.06@2x.png]]

- we can further improve this code, modify your code as follows:

```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    string words[2];

    words[0] = "HI!";
    words[1] = "BYE!";

    printf("%s\n", words[0]);
    printf("%s\n", words[1]);
}
```

- notice that both strings are stored within a single array of type `string`.

#### String Length
- a common problem within programming, and perhaps C more specifically
	- & discovering the length of an array
- how could we implement this in code?
	- type `code length.c` in the terminal window and code as follows:

```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
	// Prompt for user's name
	string name = get_string("Name: ");

	// Conut number of characters up until '\0' (aka NUL)
	int n = 0;
	while (name[n] != '\0')
	{
		n++
	}
	printf("%i\n", n);
}
```
Notice that this code loops until the `NUL` character is found

- this code can be improved by abstracting away the counting as follows
```c
#include <cs50.h>
#include <stdio.h>

int string_length(string s);

int main(void)
{
    // Prompt for user's name
    string name = get_string("Name: ");
    int length = string_length(name);
    printf("%i\n", length);
}

int string_length(string s)
{
    // Count number of characters up until '\0' (aka NUL)
    int n = 0;
    while (s[n] != '\0')
    {
        n++;
    }
    return n;
}
```

Since string length is such a common problem within programming, other programmers have created code in the `string.h` library to find the length of a string
- you can find the length of a string by modifying your code as follows:

```c
#include <cs50.h>
#include <stdio.h>
#include <string.h>

int main(void)
{
    // Prompt for user's name
    string name = get_string("Name: ");
    int length = strlen(name);
    printf("%i\n", length);
}
```
#lowertoupper 
- `ctype.h` is another library that is useful
	- imagine we wanted to create a program that converted all lowercase char to uppercase
	- in the terminal window type `code uppercase.c` and write code as follows:
```c
#include <cs50.h>
#include <stdio.h>
#include <string.h>

int main(void)
{
    string s = get_string("Before: ");
    printf("After:  ");
    for (int i = 0, n = strlen(s); i < n; i++)
    {
        if (s[i] >= 'a' && s[i] <= 'z')
        {
            printf("%c", s[i] - 32);
        }
        else
        {
            printf("%c", s[i]);
        }
    }
    printf("\n");
}
```

- Notice that this code *iterates* through each value in the string
	- the program looks at each character
	- if the character is lowercase, it subtracts the value 32 from it to convert it to uppercase
- Recalling our previous work from last week, you might remember this ASCII values chart:
![[CleanShot 2024-01-21 at 17.05.21@2x.png]]
- when a lowercase character has `32` subtracted from it, it results in an uppercase version of that same character
- while the program does what we want, there is an easier way using the `ctype.h` library, modify the program as follows
```c
#include <cs50.h>
#include <ctype.h>
#include <stdio.h>
#include <string.h>

int main(void)
{
    string s = get_string("Before: ");
    printf("After:  ");
    for (int i = 0, n = strlen(s); i < n; i++)
    {
        if (islower(s[i]))
        {
            printf("%c", toupper(s[i]));
        }
        else
        {
            printf("%c", s[i]);
        }
    }
    printf("\n");
}
```
- Notice that the program iterates through each character of the string, the `toupper` function is passed to `s[i]`.
- each character, if lowercase, is converted to uppercase
- it's worth mentioning that `toupper` **automatically knows to uppercase only lowercase characters**. Hence, your code can be simplified as follows
```c
#include <cs50.h>
#include <ctype.h>
#include <stdio.h>
#include <string.h>

int main(void)
{
    string s = get_string("Before: ");
    printf("After:  ");
    for (int i = 0, n = strlen(s); i < n; i++)
    {
        printf("%c", toupper(s[i]));
    }
    printf("\n");
}
```
Notice how this code uppercases a string using the `ctype` library
- you can read about all the capabilities of the `ctype` library on the Manual Pages
# Week 1
## PSET1 
### LLamas
#CS50/pset/1/llamas 
Objective
- goal of *n* llamas
- each year, `n / 3` new llamas are born & `n / 4` llamas pass away
- If we were to start with `n = 1200` llamas, then in the 1st year, `1200 / 3 = 400`, new llamas would be born and `1200 / 4 = 300` llamas would pass away
- At the end of that year we would have `1200 = 400 - 300 = 1300` llamas

To try another example, we were to start with `n = 1000` llamas, we would have `1000 / 3 = 333.33` new llamas
can't have a decimal portion of a llama, we'll have to truncate decimal to get `333` new llamas born

`1000 / 4 = 250` llamas will pass away, we'll end up with a total of `1000 + 333 - 250 = 1083` llamas

Implementation details
Complete the implementation of population.c such that it calculates the number of years required for the population to grow from the start size to the end size

- your program should first prompt the user for a starting population size
	- if the user enters a number less than 9 (the minimum allowed population size) the user should be re-prompted to enter a starting population size until they enter a number that is greater than or equal to 9. (if we start with fewer than 9 llamas, the population of llamas will quickly become stagnant)
- Your program should then prompt the user for an ending population size
	- if the user enters a number less than the starting population size, the user should be re-prompted to enter an ending population size until they enter a number that is greater than or equal to the starting population size. (After all, we want the population of llamas to grow!)
- Your program should then calculate the (integer) number of years required for the population to reach at least the size of the end value
- Finally your program should print the number of years required for the llama population to reach that end size, as by printing to the terminal `Years: n` where `n` is the number of years.

Hints:
- if you want to repeatedly re-prompt the user for the value of a variable until some condition is met, you might want to use a `do ... while` loop. for example, recall the following code from lecture, which prompts the user repeatedly until they enter a positive integer

``` C
int n;
do
{
	n = get_int("Positive Integer: ");
}
while (n < 1);
```
How might you adapt this code to ensure a start size of at least 9, and an end size of at least the start size?

### PSET1 CS50- MARIO (MORE)

### PSET1 - CREDIT
#CS50/pset/1/credit 

2023-12-30

```
int credit(long) // get the # inputted by user? 

>> I need to apply Luhn's algorithm first
> Then I will validate whether or not the card is valid
> Then I will validate the card's provider
this will determine criteria

>> Luhn's
>> multiply every other digit by 2, starting with the number's second-to-last digit, 
>> 	then add those products' digits' together
>> add the sum to the sum of the digits that weren't multiplied by 2
>> if the total's last digit: 0 (or formally, if the total modulo 10 is congruent to 0), number is valid

AMEX: 15-digit numbers
Master Card: 16-digit numbers
Visa: 13 and 16-digit numbers

AMEX: starting 34 or 37
MasterCard = 
51,
52,
53,
54,
55

Visa numbers:
Starting with
"4"


int main(void)
take user input (get_long);


```

Credit/debit card
- plastic card which you can pay for goods/services
- printed on card = number that's stored in db somewhere
	- when card is buying something, creditor knows whom to bill
- lots of ppl w/ CC's in this wolrd
	- so numbers are long
	- AMEX: 15 digit numbers
	- MC: 16-digit Numbers
	- Visa: 13 & 16 digit numbers
- decimal numbers (0-9) not binary
- thism eans that AMEX could print as many as 10^15 unique cards
	- a quadrillion

- not really though
	- some CC# have some structure to them
	- All AMEX numbers start with 34 or 37
	- most MC # start with 51, 52, 53, 54, 55
		- or have other potential starting numbres which we won't concern ourselves with for this problem
	- all Visa numbers start with 4
	- credit card numbers have a checksum built into them, a mathematical relationship between at least one number and others.
		- that checksum enables computers to detect typos (e.g., transpositions), if not fraudulent numbers
		- wihtout having to query a db
		- dishonest mathematician could certain craft a fake number that nonetheless resepcts the mathematical constraint, so a database lookup is still necessary for more rigorous checks.
- LUHN'S ALGORITHM
	- most cards use an algorithm invented by Hans Peter Luhn of IBM
	- LuhnCC number is syntactically valid as follows:
	  1. multiply every other digit by 2, starting with the number's second-to-last digit, then
		  1. add those products' digits together
	  2. Add the sum of the digits that weren't multiplied by 2
	  3. if the total's last digit is 0 (or, put more formally, if the total modulo 10 is congruent to 0), the number is valid
- Take David's Visa for example: 4003600000000014
	1. For the sake of discussion, let’s first underline every other digit, starting with the number’s second-to-last digit:
    
    4003600000000014
    
    Okay, let’s multiply each of the underlined digits by 2:
    
    1•2 + 0•2 + 0•2 + 0•2 + 0•2 + 6•2 + 0•2 + 4•2
    
    That gives us:
    
    2 + 0 + 0 + 0 + 0 + 12 + 0 + 8
    
    Now let’s add those products’ digits (i.e., not the products themselves) together:
    
    2 + 0 + 0 + 0 + 0 + 1 + 2 + 0 + 8 = 13
    
2. Now let’s add that sum (13) to the sum of the digits that weren’t multiplied by 2 (starting from the end):
    
    13 + 4 + 0 + 0 + 0 + 0 + 0 + 3 + 0 = 20
    
3. Yup, the last digit in that sum (20) is a 0, so David’s card is legit!
Implementation Details
- in the file called `credit.c` in the `credit` directory, write a program that prompts the user for a credit card number and then reports (via `printf`) whether it is a valid American Express, MasterCard, or Visa card number, per the definitions of each's format herein
- so we can automate some tests of code, ask that program's last line of output be `AMEX\n` or `MASTERCARD\n` or `VISA\n` or `INVALID\n`
- Simplicity, we can assume that user's input will be entirely numeric (i.e. devoid of hyphens) and it won't have leading zeroes.
- do not assume that the user's input will fit in an `int` ! Best to use `get_long` from CS50's library to get users' input
Consider the below representative of how your own program should behave when passed a valid credit card number
```
$ ./credit
Number: 400360000000014
VISA
```
Now, `get_long` itself will reject hyphens (and more) anyway:
```
$ ./credit
Number: 4003-6000-0000-0014
Number: foo
Number: 40360000000000014
VISA
```

But up to us to catch inputs that are not credit card numbers (e.g. a phone number, even if numeric:
```
$ ./credit
Number: 6176292929
INVALID
```
test out your program with a whole bunch of inputs, both valid and invalid.

Here are a few card numbers that PayPal recommends for testing.


#### Stack Overflow:
--
```c
#include <stdio.h>
#include <cs50.h>

// Luhn's Algorithm

int main(void) 
{
    long cardNumber = get_long("Please, enter your card number: ");
    int sum1 = 0, num = 0, remainder = 0, sum2 = 0;
    long temp = cardNumber;
    
    while (temp > 0) 
    {
        num = ((temp / 10) % 10) * 2; // Multiplying every other digit by 2, starting with the number’s second-to-last digit
        while (num > 0) 
        {
            remainder = num % 10;
            sum1 += remainder; // Adding those products’ digits together
            num /= 10;
        }
        temp /= 100;
    }
    
    // So as to restore the initial values of remainder and temp for the use in next loop
    remainder = 0;
    temp = cardNumber;
    
    while (temp > 0) 
    {
        remainder = temp % 10;
        sum2 += remainder; // Sum of the digits that weren’t multiplied by 2
        temp /= 100;
    }
    
    ((sum1 + sum2) % 10) == 0 ? printf("Valid\n") : printf("Invalid\n");
    return 0;
}
```

```c
 while (temp > 0) 
    {
        num = ((temp / 10) % 10) * 2; // Multiplying every other digit by 2, starting with the number’s second-to-last digit
        while (num > 0) 
        {
            remainder = num % 10;
            sum1 += remainder; // Adding those products’ digits together
            num /= 10;
        }
        temp /= 100;
```

num = ((temp / 10)) % 10) * 2 = 2

remainder = 2 % 10 = 2
sum1 += remainder = sum1 + remainder = 0+2 = 2
num /= 10 = num = 2/10 = 0.2 END 

temp/= 100; = 4003600000000014/100 

temp = 40036000000000
since temp > 0


#### Finding the first digit, finding the last digit:



To find the last digit of a number, we use the modulo operator %. When the module divided by 10 returns its last digit.

Suppose that if n = 1234

the last digit = n% 10 => 4

Finding the first digit of a number is less expensive than the last digit. To find the first digit of a number, we divide the given number by 10 until the number is greater than 10. At the end, we are left with the first digit.


#CS50/pset/1/mario

- [x] Recreate pyramids using hashes for bricks
	- [x] 2 hash space between 1st and 2nd pyramids
- [x] Allow user to determine how tall the pyramids should be (1-8 inclusive integers)
- [x] User should be reprompted for a legal input



FINAL CODE: 
```c
#include <cs50.h>
#include <stdio.h>

int get_size(void);           // Prompt user for size of pyramid, must be 1 - 8 inclusive.
void print_pyramid(int size); // Create the actual pyramid

int main(void)
{
    int size = get_size();
    print_pyramid(size);
}

int get_size(void)
{
    int size;
    do
    {
        size = get_int("Size: ");
    }
    while (size < 1 || size > 8);
    return size;
}

void print_pyramid(int size)
{
    for (int row = 0; row < size; row++)
    {
        // Print spaces for left pyramid
        for (int space = 0; space < size - row - 1; space++)
        {
            printf(" ");
        }

        // Print hashes for left pyramid
        for (int hash = 0; hash <= row; hash++)
        {
            printf("#");
        }

        // Print gap between pyramids
        printf("  ");

        // Print hashes for right pyramid
        for (int hash = 0; hash <= row; hash++)
        {
            printf("#");
        }

        // End of row
        printf("\n");
    }
}
```

```
getinteger from user n
function: based on integer n
> assign number of rows = N
> function for pyramid 1, pyramid 2?
> printf hashes based on row N++
> printf spaces based on row N++
> Print 2 gap constant space between pyramid 1 pyramid 2
> inverse print?
```
OLD MARIO CODE:
```c
#include <stdio.h>
#include <cs50.h>

int get_size(void);
void print_grid(int n);

int main(void)
{
    int n = get_size();
    print_grid(n);
}

int get_size(void)
{
    int n;
    do
    {
        n = get_int("Size: ");
    }
    while (n < 1);
    return n;
}

void print_grid(int n)
{
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < n; j++)
        {
            printf("#");
        }
        printf("\n");
    }
}
```

row 0 = 3 spc | 1 h
row 1 = 2 spc | 2 h
row 2 = 1 spc | 3 h

4 = n; s = n-1, h = n-3
0th line, make 3 spaces, 1 h


Toward intro 1-1 in Super Mario Bros., Mario has to hop over adjacent pyramid of blocks like seen below:

![[Pasted image 20231228181651.png]]

- recreate pyramids in C, in text, using hashes (#) for bricks

```
   #  #
  ##  ##
 ###  ###
####  ####
```
- call it `mario`
	- allow user to decide how tall the pyramids should be
		- prompt for pos integer (1-8, inclusive)
- Here's how the program might work if the user inputs `8` when prompted:
 ```
-$ ./mario
Height: 8
       #  #
      ##  ##
     ###  ###
    ####  ####
   #####  #####
  ######  ######
 #######  #######
########  ########
```

```
$ ./mario
Height: 4
   #  #
  ##  ##
 ###  ###
####  ####
```

2:
```
$ ./mario
Height: 2
 #  #
##  ##
```
1:
```
$ ./mario
Height: 1
#  #
```
User should be reprompted:
```
$ ./mario
Height: -1
Height: 0
Height: 42
Height: 50
Height: 4
   #  #
  ##  ##
 ###  ###
####  ####
```
- Notice that width of the “gap” between adjacent pyramids is equal to the width of two hashes, irrespective of the pyramids’ heights.
- open `mario.c` to implement this problem as described


## Notes
#cs50/week/1
Today
- variables and types
- Input and Printing
- Functions, Loops, and Conditionals
- Problem Set 1

### Variables and Types
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

### Inputs and printing
`int calls = get_int("Calls: ");`

`get_int("Calls: ");`
- function call

``` c
int calls = 4;
printf("calls is %\n", calls);

// output: "calls is 4"

```

### Functions Loops and Variables

#### While loops
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
#### For Loops
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

### Mario - Example
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

`print_row` is up, `print_row` is towards the bottom of the srouce code
- it's on a separate function with the same name

- there's probably an assocaiton with the height and the row length
#### For loops (2)
- so long as you have declared the variable within the loop
	- you have access to  it
	- variable `i` for example , is declared and initialized in the loop
		- access to it within the culry braces
	- this concept is called "scope"


