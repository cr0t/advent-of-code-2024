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
