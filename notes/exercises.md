Exercises for lecture 4
=======================

In this class we are going to spend some time practicing using dplyr and
tidyr

We begin by loading `gapminder` and the `tidyverse`

if you cannot load `tidyverse`, please try loading just `dplyr` and
`tidyr`

Challenge 1
-----------

calculate the mean life expectancy, population, and gdpPercap for each
continent. Hint: use the group\_by() and summarize() functions we
learned in the dplyr lesson

### Challenge 1h.1

Rewrite the code above, but this time try using `tidyr::gather()`. HINT:
First, gather all of the variables (lifeExp, pop and gdpPercap) into a
new column called "variable\_name". Then, group by both continent and
variable\_name

### Challenge 1h.2

Rewrite the code above, using `summarize_each` and/or `summarize_all`.
To do so you might want to read the help file: `?summarize_each` \#\#\#

### Challenge 2.1

What is different about this organization of the dataset? What, if
anything, is *easier* to do in this format? what would be harder?

### Challenge 2.2

Reformat this data into "long" format, using `tidyr::gather`. Your new
dataset should have four columns: \* Continent \* Country \*
observation\_type \* observation\_value

### Challenge 2.3

The variable `observation_type` is really two variables combined
together. Use the function `separate` to split these into different
values. You may want to consult `?separate()`.

### Challenge 2.4

Now, practice reversing this process -- i.e., combine two columns into
one -- using `tidyr::unite`

### Challenge 2h.1

Take this 1 step further and create a gap\_ludicrously\_wide format data
by spreading over countries, year and the 3 metrics? Hint this new
dataframe should only have 5 rows.

### Challenge 3.1

Calculate the average life expectancy per country. Which has the longest
average life expectancy and which has the shortest average life
expectancy? \#\#\# Challenge 3.1h

Rank all the countries by their life expenctancy. Use one of the "window
functions" for calculating rank (e.g., see `dense_rank`)
