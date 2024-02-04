
---

# Week 3
## Notes
### Algorithms
- taking a break from syntax
- we have a lot of vocabulary now!
	- might not grasp functionality
- but instead, let's think about how to think 'algorithimically'
	- quantize real world problems so we can map puzzle pieces from week 0, with this new syntax to solve those problems
- recall the time to solve v. size of problem chart
	- how many units of measure -- whatever you're using
	- the first algorithm: going 1 page at a time
		- 1:1 slope!
	- the second algorithm, we started doing two pages at a time
		- might skip a page;
			- faster, but not yet 2/1
	- the third algorithm; logirithimc curve
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
- log2n
	- log base 2 of n
	- what is the valuetaking n and dividing it by 2 again and again and again and again
		- until left with 1 person/value standing
- being this precise, it's not that intelectually interesting
- generally a computer scientist is asked the efficiency of your algorithm
	- how good or bad?
		- will talk about it being on some order of steps
		- just wave your hand at it
		- broad strokes: all we care about is certain numbers
	- when computer scientists, they thowr away efifcineceis
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
	- Theta notation
	- $\theta$ 
		- same number of steps as Big O and Omega analysis
- you'll go more into these ideas with computer science
	- Big O: upper bound
	- omega: lower bound
- with thtat being said, let's see how we might translate this to actual code
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
					- (n - 1), + (n - 2) + (n - 3) ... + 1
					- this series here...
						- n(n - 1)/2
							- no matter what n is, this formula captures the sum of the series
							- that is how many steps taken again and again when implementing for 8 people
						- = n^2/2 - n/2
							- n^2 seems like the biggest term, the *dominant term*
							- dividing this quadratic formula by 2 is big... but as n gets larger and larger...
					- selection sort is on the order of *O(n^2)*
					- kind of a slow one, again
					- can we do actually better?
					- if selection sort is in the order of n^2?
						- best case...
						- no special conditional, if sorted already, exit early
				- this line of pseudocode tells us what...
					- not precise, but on the order of n^2
					- any number of doors, it's going to end up being omega of n^2
					- even in the best case where numbers are already sorted, there is nothing in the algo that says I'm done and exit prematurely
					- big o of n^2, and omega of n^2
						- by coincidence, because these boundaries are the same:
							- $\theta$ n^2
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
	- this could be simplified to n(n-1)/2 or more simply $O$ \n$^2$
- *Bubble sort* is another sorting algorithm that works by repeatedly swapping elements to "bubble" larger elements to the end
- the pseudocode for bubble sort is
```c
Repeat n-1 times
	For i from 0 to n-2
		If numbers[i] and numbers[i+1] our of order
			Swap them
		If no swaps
			Quit
```
- as we further sort the array, we



    
    
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


