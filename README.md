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
