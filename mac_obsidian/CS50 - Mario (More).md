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