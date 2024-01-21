

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