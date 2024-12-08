# Advent of Code 2024

Use [Livebook](https://livebook.dev/) server to run these `.livemd` files.

```console
$ livebook server
[Livebook] Application running at http://localhost:8080/?token=***
```

Open the given link and then `.livemd` file you want.

Input can be found in the similarly-named files.

## Solutions' Notes

[day01.livemd](day01.livemd)

- **Input:** split all the input by whitespace (this give a list of ALL the given numbers), chunk the list by 2 and transpose it right away to get two lists (one for 1st column, another for the 2nd column)
- **Part 1:** sort both lists, zip together (to get both {left, right} nums as one item), find the difference, sum it up
- **Part 2:** convert right list into a Map with keys of how many time each number appears in the list (frequencies); then take each item from the left list, find number of occurrences in the right (using converted data struct), sum it up

[day02.livemd](day02.livemd)

- **Input:** every line is a single piece of work, split it line-by-line, then split each line into items and convert them to Integers
- **Part 1:** express what is `safe?/1` means for a line of numbers (with an "early exit" based on `Enum.reduce_while/3`)
- **Part 2:** re-use the same `safe?/1` from the first part, but make all possible inputs one step before and select those that safe

[day03.livemd](day03.livemd)

- **Input:** use Regex to scan the input for the instructions and arguments (they all have well-defined format)
- **Part 1:** reduce the list of instructions, considering only `mul(x, y)` ones
- **Part 2:** the similar reduce but with more rules (expressed as multiple clauses of the reducer-function)

[day04.livemd](day04.livemd)

- **Input:** convert the input to a Map with keys as {x, y} coordinates pointing to values (what's on the given field)
- **Part 1:** find all starting points for the `X` char, then (using starting points) build lists of coordinates for possible positions of XMAS letters on the board; using these coordingate, get real values from the map, and compare if we get an 'X', 'M', 'A', 'S' then (if so, add 1 to the hits sum)
- **Part 2:** similar to day 1, but build X-shaped coords for each 'A' starting point (as middle one), collect letters and compare that both diagonals are 'MAS' (or 'SAM'), count only those that fully match the pattern

> Visually, it might be represented as sitting next to a board, and applying some form to it (XMAS, or X-shaped MAS) to each starting point and see what we get. Like playing in a sandbox and "baking" stuff.

[day05.livemd](day05.livemd)

- **Input:** input has two sections: rules and updates; rules better to save as Map, while updates is just a list of lists of integers; we also define a couple of helpers - `correct/2` and `sum_middles/1` which we use later
- **Part 1:** filter all correct items, sum their middles...
- **Part 2:** reject all correct items and fix incorrect ones; to fix them, we calculate a "weight" of each item in the list – we take item's rules, and compare to siblings - more siblings in the rules, more weight; later we just order by weight; and, eventually, sum the middles of the fixed updates

[day06.livemd](day06.livemd)

- **Input:** it's a simple 2D map, convert it to a Map with keys-coords pointing to actual value of the field it represents; in addition max_x and max_y included; we also define helpers to "move" into some direction from a stating position and detecting if we hit an obstacle in the process, or went into edge - we return visited coordinates, including some meta data (where to turn next, for example)
- **Part 1:** find starting position, feed it to the `track/4z` function that collects all the visited coordinates until we approach an "edge"; then we flatten, find unique fields and count them
- **Part 2:** use original path to put random obstacles on (not sure why it works, but it is! and it works ~3x times faster in comparison to check EVERY "." field for possible loops); then take every possible new obstacle and check if we in a loop or not; for that we have changed the original data structure to check if we are in the same coordinate and same direction easier (there is a Map with keys-directions and values - list of visited previously fields having the same direction).

[day07.livemd](day07.livemd)

- **Input:** Regex helps to find all the numbers in the lines; due to format, the very first number in each "equation" is its result, the rest – operands; in addition to parsing, we also place combinations generator which takes a list of possible values and a expecting length and generates combinations of that length with all positions of values
- **Part 1:** filter equations with the function that (1) generates all possible combinations of operators, (2) "inserts" them between the operands, (3) does the equation, and (4) compares to the result; then find the sum of results
- **Part 2:** do basically everything as in the 1st part, but (due to the amount of possible combinations increase) parallelize it with Task.async_stream; also use Stream.map on combinations to reduce number of eventual computations
