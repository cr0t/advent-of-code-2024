# Advent of Code 2024

Use [Livebook](https://livebook.dev/) server to run these `.livemd` files.

```console
$ livebook server
[Livebook] Application running at http://localhost:8080/?token=***
```

Open the given link and then `.livemd` file you want.

Input can be found in the similarly-named files.

## Solutions' Notes

### [Day 1](day01.livemd): Historian Hysteria

- **Input:** Split all the input by whitespace (this gives a list of ALL the given numbers), chunk the list by 2, and transpose it right away to get two lists (one for the 1st column, another for the 2nd column).
- **Part 1:** Sort both lists, zip together (to get both {left, right} numbers as one item), find the difference, and sum it up.
- **Part 2:** Convert the right list into a Map structure with keys of how many times each number appears in the list (frequencies); then take each item from the left list, find the number of occurrences in the right (using the converted data structure), and sum it up.

### [Day 2](day02.livemd): Red-Nosed Reports

- **Input:** Every line is a single piece of work; split it line-by-line, then split each line into items and convert them to integers.
- **Part 1:** Express what `safe?/1` means for a line of numbers (with an “early exit” based on `Enum.reduce_while/3`)
- **Part 2:** Re-use the same `safe?/1` from the first part, but make all possible inputs one step before and select those that are safe.

### [Day 3](day03.livemd): Mull It Over

- **Input:** Use Regex to scan the input for the instructions and arguments (they all have a well-defined format).
- **Part 1:** Reduce the list of instructions, considering only `mul(x, y)` ones.
- **Part 2:** The similar reduce, but with more rules (expressed as multiple clauses of the reducer function).

### [Day 4](day04.livemd): Ceres Search

- **Input:** Convert the input to a map with keys as {x, y} coordinates pointing to values (what's on the given field).
- **Part 1:** Find all the starting points for the `X` character, then (using starting points) build lists of coordinates for possible positions of XMAS letters on the board; using these coordinate, get real values from the map, and compare if we get an 'X', 'M', 'A', 'S' then (if so, add 1 to the hits sum).
- **Part 2:** Similar to Part 1, but build X-shaped coordinates for each 'A' starting point (as the middle one), collect letters and compare that both diagonals are 'MAS' (or 'SAM'), and count only those that fully match the pattern.

_Visually, it might be represented as sitting next to a board, and applying some form to it (XMAS, or X-shaped MAS) to each starting point and seeing what we get. Like playing in a sandbox and “baking” stuff._

### [Day 5](day05.livemd): Print Queue

- **Input:** Input has two sections: rules and updates; rules are better to save as Map, while updates are just a list of lists of integers; we also define a couple of helpers — `correct/2` and `sum_middles/1` which we use later.
- **Part 1:** Filter all correct items, sum their middles…
- **Part 2:** Reject all correct items and fix incorrect ones; to fix them, we calculate a “weight” of each item in the list; we take the item's rules, and compare to siblings — more siblings in the rules, more weight; later we just order by weight; and, eventually, sum the middles of the fixed updates.

### [Day 6](day06.livemd): Guard Gallivant

- **Input:** It's a simple map, convert it to a Map with keys-coordinates pointing to actual value of the field it represents; in addition max_x and max_y included; we also define helpers to “move” into some direction from a stating position and detecting if we hit an obstacle in the process, or went into edge — we return visited coordinates, including some metadata (where to turn next, for example).
- **Part 1:** Find the starting position, feed it to the `track/3` function that collects all the visited coordinates until we approach an “edge,”  then we flatten, find unique fields, and count them.
- **Part 2:** Use the original path to put random obstacles on (not sure why it works, but it is!). It works ~3x times faster in comparison to check EVERY “.” field for possible loops); then take every possible new obstacle and check if we in a loop or not; for that, we have changed the original data structure to check if we are in the same coordinate and same direction easier (there is a Map with keys-directions and values — list of visited previously fields having the same direction).

### [Day 7](day07.livemd): Bridge Repair

- **Input:** Regex helps to find all the numbers in the lines; due to format, the very first number in each “equation” is its result, the rest are operands; in addition to parsing, we also place combinations generator which takes a list of possible values and an expecting length and generates combinations of that length with all positions of values.
- **Part 1:** Filter equations with the function that (1) generates all possible combinations of operators, (2) “inserts” them between the operands, (3) does the equation, and (4) compares to the result; then find the sum of results.
- **Part 2:** Do basically everything as in the 1st part, but (due to the increase in possible combinations) parallelize it with `Task.async_stream`; also use `Stream.map` on combinations to reduce the number of eventual computations.

### [Day 8](day08.livemd): Resonant Collinearity

- **Input:** Input can be represented using Map (with `max_x`, and `max_y`).
- **Part 1:** Find all antennas' positions and group them by their “frequency” (label); find all the unique pairs of these antennas and then calculate list of antinodes' coordinates (using Pythagorean theorem we can determine deltas of x and y, and find opposite points on the grid; points' coordinates depend on the mutual position of first and second antennas); we filter only those coordinates that are in the map; moreover, due to antinodes can take the same coordinate, we have to find unique only, and count them.
- **Part 2:** We do similar thing as in the 1st part, but now we find not just 2 antinode coordinates for each antenna's pair; we find ALL the antinodes in each direction, starting from each antenna's point and until we reach the edge of the map; the antennas coordinates are also added to the list of antinodes; the rest is similar — uniq, count.

### [Day 9](day09.livemd): Disk Fragmenter

- **Input:** It's a very long line of digits, which we must split into single ones, convert to integers, and then chunk every 2 (we can add 0 to the last if needed). We also give an index to each of the pair, because later it will become a file ID.
- **Part 1:** To simplify data lookups, we make a list of file blocks (and reverse it). Then we go over each block in the disk map, and try to fit a piece of a file from the reversed blocks list (this helps us to get files' blocks from the farthest side easily). We do so until out (reversed) stash is empty or we processed all the data blocks from the original disk map (excluding trailing empty space). Then calculate checksum.
- **Part 2:** It's a trickier version of first, because we have to find a proper hole to move file as a whole. We made a `files_meta` map with some extra info (for quicker data retrieval). Though, we store the disk map as a list of two-element tuples (with file_id and index of that block - we will need this data at the checksum step). We go over the files (in the descending order for file ID), try to find a proper hole for it and move if hole is found; if hole's not found, we check next file, and so on. When we find a hole, we cut the disk maps list into a few pieces (like in a film editing process), generate new file and hole blocks, and glue them all again. At the end, checksum.
