Exercises for lecture 4
=======================

In this class we are going to spend some time practicing using dplyr and
tidyr

We begin by loading `gapminder` and the `tidyverse`

    library(gapminder)
    library(tidyverse)

    ## Loading tidyverse: ggplot2
    ## Loading tidyverse: tibble
    ## Loading tidyverse: tidyr
    ## Loading tidyverse: readr
    ## Loading tidyverse: purrr
    ## Loading tidyverse: dplyr

    ## Conflicts with tidy packages ----------------------------------------------

    ## filter(): dplyr, stats
    ## lag():    dplyr, stats

if you cannot load `tidyverse`, please try loading just `dplyr` and
`tidyr`

Challenge 1
-----------

calculate the mean life expectancy, population, and gdpPercap for each
continent. Hint: use the group\_by() and summarize() functions we
learned in the dplyr lesson

    gapminder %>% 
      group_by(continent) %>% 
      summarize(mean_life = mean(lifeExp),
                meanpop   = mean(pop),
                meanGDP   = mean(gdpPercap))

    ## # A tibble: 5 × 4
    ##   continent mean_life  meanpop   meanGDP
    ##      <fctr>     <dbl>    <dbl>     <dbl>
    ## 1    Africa  48.86533  9916003  2193.755
    ## 2  Americas  64.65874 24504795  7136.110
    ## 3      Asia  60.06490 77038722  7902.150
    ## 4    Europe  71.90369 17169765 14469.476
    ## 5   Oceania  74.32621  8874672 18621.609

### Challenge 1h.1

Rewrite the code above, but this time try using `tidyr::gather()`. HINT:
First, gather all of the variables (lifeExp, pop and gdpPercap) into a
new column called "variable\_name". Then, group by both continent and
variable\_name

    gapminder %>% 
      gather(key = "variable", value = "value", lifeExp:gdpPercap) %>% 
      group_by(continent, variable) %>% 
      summarize(mean_value = mean(value))

    ## Source: local data frame [15 x 3]
    ## Groups: continent [?]
    ## 
    ##    continent  variable   mean_value
    ##       <fctr>     <chr>        <dbl>
    ## 1     Africa gdpPercap 2.193755e+03
    ## 2     Africa   lifeExp 4.886533e+01
    ## 3     Africa       pop 9.916003e+06
    ## 4   Americas gdpPercap 7.136110e+03
    ## 5   Americas   lifeExp 6.465874e+01
    ## 6   Americas       pop 2.450479e+07
    ## 7       Asia gdpPercap 7.902150e+03
    ## 8       Asia   lifeExp 6.006490e+01
    ## 9       Asia       pop 7.703872e+07
    ## 10    Europe gdpPercap 1.446948e+04
    ## 11    Europe   lifeExp 7.190369e+01
    ## 12    Europe       pop 1.716976e+07
    ## 13   Oceania gdpPercap 1.862161e+04
    ## 14   Oceania   lifeExp 7.432621e+01
    ## 15   Oceania       pop 8.874672e+06

### Challenge 1h.2

Rewrite the code above, using `summarize_each` and/or `summarize_all`.
To do so you might want to read the help file: `?summarize_each` \#\#\#
Challenge 2.1

Begin by reading in a "wide" version of the dataset

    gap_wide <- read.csv("data/gapminder_wide.csv", stringsAsFactors = FALSE)
    str(gap_wide)

    ## 'data.frame':    142 obs. of  38 variables:
    ##  $ continent     : chr  "Africa" "Africa" "Africa" "Africa" ...
    ##  $ country       : chr  "Algeria" "Angola" "Benin" "Botswana" ...
    ##  $ gdpPercap_1952: num  2449 3521 1063 851 543 ...
    ##  $ gdpPercap_1957: num  3014 3828 960 918 617 ...
    ##  $ gdpPercap_1962: num  2551 4269 949 984 723 ...
    ##  $ gdpPercap_1967: num  3247 5523 1036 1215 795 ...
    ##  $ gdpPercap_1972: num  4183 5473 1086 2264 855 ...
    ##  $ gdpPercap_1977: num  4910 3009 1029 3215 743 ...
    ##  $ gdpPercap_1982: num  5745 2757 1278 4551 807 ...
    ##  $ gdpPercap_1987: num  5681 2430 1226 6206 912 ...
    ##  $ gdpPercap_1992: num  5023 2628 1191 7954 932 ...
    ##  $ gdpPercap_1997: num  4797 2277 1233 8647 946 ...
    ##  $ gdpPercap_2002: num  5288 2773 1373 11004 1038 ...
    ##  $ gdpPercap_2007: num  6223 4797 1441 12570 1217 ...
    ##  $ lifeExp_1952  : num  43.1 30 38.2 47.6 32 ...
    ##  $ lifeExp_1957  : num  45.7 32 40.4 49.6 34.9 ...
    ##  $ lifeExp_1962  : num  48.3 34 42.6 51.5 37.8 ...
    ##  $ lifeExp_1967  : num  51.4 36 44.9 53.3 40.7 ...
    ##  $ lifeExp_1972  : num  54.5 37.9 47 56 43.6 ...
    ##  $ lifeExp_1977  : num  58 39.5 49.2 59.3 46.1 ...
    ##  $ lifeExp_1982  : num  61.4 39.9 50.9 61.5 48.1 ...
    ##  $ lifeExp_1987  : num  65.8 39.9 52.3 63.6 49.6 ...
    ##  $ lifeExp_1992  : num  67.7 40.6 53.9 62.7 50.3 ...
    ##  $ lifeExp_1997  : num  69.2 41 54.8 52.6 50.3 ...
    ##  $ lifeExp_2002  : num  71 41 54.4 46.6 50.6 ...
    ##  $ lifeExp_2007  : num  72.3 42.7 56.7 50.7 52.3 ...
    ##  $ pop_1952      : num  9279525 4232095 1738315 442308 4469979 ...
    ##  $ pop_1957      : num  10270856 4561361 1925173 474639 4713416 ...
    ##  $ pop_1962      : num  11000948 4826015 2151895 512764 4919632 ...
    ##  $ pop_1967      : num  12760499 5247469 2427334 553541 5127935 ...
    ##  $ pop_1972      : num  14760787 5894858 2761407 619351 5433886 ...
    ##  $ pop_1977      : num  17152804 6162675 3168267 781472 5889574 ...
    ##  $ pop_1982      : num  20033753 7016384 3641603 970347 6634596 ...
    ##  $ pop_1987      : num  23254956 7874230 4243788 1151184 7586551 ...
    ##  $ pop_1992      : num  26298373 8735988 4981671 1342614 8878303 ...
    ##  $ pop_1997      : num  29072015 9875024 6066080 1536536 10352843 ...
    ##  $ pop_2002      : int  31287142 10866106 7026113 1630347 12251209 7021078 15929988 4048013 8835739 614382 ...
    ##  $ pop_2007      : int  33333216 12420476 8078314 1639131 14326203 8390505 17696293 4369038 10238807 710960 ...

What is different about this organization of the dataset? What, if
anything, is *easier* to do in this format? what would be harder?

    gap_wide %>% 
      select(-continent, -country) %>% 
      colMeans

    ## gdpPercap_1952 gdpPercap_1957 gdpPercap_1962 gdpPercap_1967 gdpPercap_1972 
    ##   3.725276e+03   4.299408e+03   4.725812e+03   5.483653e+03   6.770083e+03 
    ## gdpPercap_1977 gdpPercap_1982 gdpPercap_1987 gdpPercap_1992 gdpPercap_1997 
    ##   7.313166e+03   7.518902e+03   7.900920e+03   8.158609e+03   9.090175e+03 
    ## gdpPercap_2002 gdpPercap_2007   lifeExp_1952   lifeExp_1957   lifeExp_1962 
    ##   9.917848e+03   1.168007e+04   4.905762e+01   5.150740e+01   5.360925e+01 
    ##   lifeExp_1967   lifeExp_1972   lifeExp_1977   lifeExp_1982   lifeExp_1987 
    ##   5.567829e+01   5.764739e+01   5.957016e+01   6.153320e+01   6.321261e+01 
    ##   lifeExp_1992   lifeExp_1997   lifeExp_2002   lifeExp_2007       pop_1952 
    ##   6.416034e+01   6.501468e+01   6.569492e+01   6.700742e+01   1.695040e+07 
    ##       pop_1957       pop_1962       pop_1967       pop_1972       pop_1977 
    ##   1.876341e+07   2.042101e+07   2.265830e+07   2.518998e+07   2.767638e+07 
    ##       pop_1982       pop_1987       pop_1992       pop_1997       pop_2002 
    ##   3.020730e+07   3.303857e+07   3.599092e+07   3.883947e+07   4.145759e+07 
    ##       pop_2007 
    ##   4.402122e+07

### Challenge 2.2

Reformat this data into "long" format, using `tidyr::gather`. Your new
dataset should have four columns: \* Continent \* Country \*
observation\_type \* observation\_value

    gap_wide %>% 
      gather(key = "observation_type", value = "observation_value", 3:38) %>% 
      head

    ##   continent      country observation_type observation_value
    ## 1    Africa      Algeria   gdpPercap_1952         2449.0082
    ## 2    Africa       Angola   gdpPercap_1952         3520.6103
    ## 3    Africa        Benin   gdpPercap_1952         1062.7522
    ## 4    Africa     Botswana   gdpPercap_1952          851.2411
    ## 5    Africa Burkina Faso   gdpPercap_1952          543.2552
    ## 6    Africa      Burundi   gdpPercap_1952          339.2965

    gap_wide %>% 
      gather(key = "observation_type", value = "observation_value", gdpPercap_1952:pop_2007) %>% 
      head

    ##   continent      country observation_type observation_value
    ## 1    Africa      Algeria   gdpPercap_1952         2449.0082
    ## 2    Africa       Angola   gdpPercap_1952         3520.6103
    ## 3    Africa        Benin   gdpPercap_1952         1062.7522
    ## 4    Africa     Botswana   gdpPercap_1952          851.2411
    ## 5    Africa Burkina Faso   gdpPercap_1952          543.2552
    ## 6    Africa      Burundi   gdpPercap_1952          339.2965

    gap_wide %>% 
      gather(key = "observation_type", value = "observation_value", -continent, -country) %>% 
      head

    ##   continent      country observation_type observation_value
    ## 1    Africa      Algeria   gdpPercap_1952         2449.0082
    ## 2    Africa       Angola   gdpPercap_1952         3520.6103
    ## 3    Africa        Benin   gdpPercap_1952         1062.7522
    ## 4    Africa     Botswana   gdpPercap_1952          851.2411
    ## 5    Africa Burkina Faso   gdpPercap_1952          543.2552
    ## 6    Africa      Burundi   gdpPercap_1952          339.2965

    gap_wide %>% 
      gather(key = "observation_type", value = "observation_value", -continent, -country) %>% 
      head

    ##   continent      country observation_type observation_value
    ## 1    Africa      Algeria   gdpPercap_1952         2449.0082
    ## 2    Africa       Angola   gdpPercap_1952         3520.6103
    ## 3    Africa        Benin   gdpPercap_1952         1062.7522
    ## 4    Africa     Botswana   gdpPercap_1952          851.2411
    ## 5    Africa Burkina Faso   gdpPercap_1952          543.2552
    ## 6    Africa      Burundi   gdpPercap_1952          339.2965

### Challenge 2.3

The variable `observation_type` is really two variables combined
together. Use the function `separate` to split these into different
values. You may want to consult `?separate()`.

    gap_long_separated <- gap_wide %>% 
      gather(key = "variable_name", value = "Value", gdpPercap_1952:pop_2007) %>% 
      separate(variable_name, c("variable", "years"), sep = "_")
    head(gap_long_separated)

    ##   continent      country  variable years     Value
    ## 1    Africa      Algeria gdpPercap  1952 2449.0082
    ## 2    Africa       Angola gdpPercap  1952 3520.6103
    ## 3    Africa        Benin gdpPercap  1952 1062.7522
    ## 4    Africa     Botswana gdpPercap  1952  851.2411
    ## 5    Africa Burkina Faso gdpPercap  1952  543.2552
    ## 6    Africa      Burundi gdpPercap  1952  339.2965

### Challenge 2.4

Now, practice reversing this process -- i.e., combine two columns into
one -- using `tidyr::unite`

    gap_long_separated %>% 
      unite(col = "new_variable", country, years) %>% 
      head

    ##   continent      new_variable  variable     Value
    ## 1    Africa      Algeria_1952 gdpPercap 2449.0082
    ## 2    Africa       Angola_1952 gdpPercap 3520.6103
    ## 3    Africa        Benin_1952 gdpPercap 1062.7522
    ## 4    Africa     Botswana_1952 gdpPercap  851.2411
    ## 5    Africa Burkina Faso_1952 gdpPercap  543.2552
    ## 6    Africa      Burundi_1952 gdpPercap  339.2965

### Challenge 2h.1

Take this 1 step further and create a gap\_ludicrously\_wide format data
by spreading over countries, year and the 3 metrics? Hint this new
dataframe should only have 5 rows.

    gap_long_separated %>% 
      unite(col = "new_variable", country, years) %>% 
      head

    ##   continent      new_variable  variable     Value
    ## 1    Africa      Algeria_1952 gdpPercap 2449.0082
    ## 2    Africa       Angola_1952 gdpPercap 3520.6103
    ## 3    Africa        Benin_1952 gdpPercap 1062.7522
    ## 4    Africa     Botswana_1952 gdpPercap  851.2411
    ## 5    Africa Burkina Faso_1952 gdpPercap  543.2552
    ## 6    Africa      Burundi_1952 gdpPercap  339.2965

### Challenge 3.1

Calculate the average life expectancy per country. Which has the longest
average life expectancy and which has the shortest average life
expectancy? \#\#\# Challenge 3.1h

Rank all the countries by their life expenctancy. Use one of the "window
functions" for calculating rank

    ranked_by_life <- gapminder %>% 
      group_by(country) %>% 
      summarise(meanlife = mean(lifeExp)) %>% 
      mutate(rank_life_exp = dense_rank(desc(meanlife)))

    ranked_by_life %>% 
      filter(rank_life_exp %in% range(rank_life_exp))

    ## # A tibble: 2 × 3
    ##        country meanlife rank_life_exp
    ##         <fctr>    <dbl>         <int>
    ## 1      Iceland 76.51142             1
    ## 2 Sierra Leone 36.76917           142

Data cleaning!
--------------

Data are not always in good condition. We will talk about how to use
these same skills for cleaning data.
<http://www.zoology.ubc.ca/~krebs/downloads/monitor.xlsx>
