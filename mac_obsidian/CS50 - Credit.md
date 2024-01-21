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


Stack Overflow:
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


Finding the first digit, finding the last digit:
--


To find the last digit of a number, we use the modulo operator %. When the module divided by 10 returns its last digit.

Suppose that if n = 1234

the last digit = n% 10 => 4

Finding the first digit of a number is less expensive than the last digit. To find the first digit of a number, we divide the given number by 10 until the number is greater than 10. At the end, we are left with the first digit.
