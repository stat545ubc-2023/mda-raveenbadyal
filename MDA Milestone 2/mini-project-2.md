Mini Data Analysis Milestone 2
================

*To complete this milestone, you can either edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to the rest of your mini data analysis project!

In Milestone 1, you explored your data. and came up with research
questions. This time, we will finish up our mini data analysis and
obtain results for your data by:

- Making summary tables and graphs
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

We will also explore more in depth the concept of *tidy data.*

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 50 points: 45 for your analysis, and
5 for overall reproducibility, cleanliness, and coherence of the Github
submission.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

- Understand what *tidy* data is, and how to create it using `tidyr`.
- Generate a reproducible and clear report using R Markdown.
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
library(here)
```

# Task 1: Process and summarize your data

From milestone 1, you should have an idea of the basic structure of your
dataset (e.g. number of rows and columns, class types, etc.). Here, we
will start investigating your data more in-depth using various data
manipulation functions.

### 1.1 (1 point)

First, write out the 4 research questions you defined in milestone 1
were. This will guide your work through milestone 2:

<!-------------------------- Start your work below ---------------------------->

1.  *How does diameter of the tree change if there is or is not a root
    barrier installed?*
2.  *Does the number of trees in each neigborhood differ?*
3.  *Do trees planted in 1995 and 2015 have a large difference in
    height_range_id?*
4.  *What is the distribution of tree diameter in each neighborhood?*
    <!----------------------------------------------------------------------------->

Here, we will investigate your data using various data manipulation and
graphing functions.

### 1.2 (8 points)

Now, for each of your four research questions, choose one task from
options 1-4 (summarizing), and one other task from 4-8 (graphing). You
should have 2 tasks done for each research question (8 total). Make sure
it makes sense to do them! (e.g. don’t use a numerical variables for a
task that needs a categorical variable.). Comment on why each task helps
(or doesn’t!) answer the corresponding research question.

Ensure that the output of each operation is printed!

Also make sure that you’re using dplyr and ggplot2 rather than base R.
Outside of this project, you may find that you prefer using base R
functions for certain tasks, and that’s just fine! But part of this
project is for you to practice the tools we learned in class, which is
dplyr and ggplot2.

**Summarizing:**

1.  Compute the *range*, *mean*, and *two other summary statistics* of
    **one numerical variable** across the groups of **one categorical
    variable** from your data.
2.  Compute the number of observations for at least one of your
    categorical variables. Do not use the function `table()`!
3.  Create a categorical variable with 3 or more groups from an existing
    numerical variable. You can use this new variable in the other
    tasks! *An example: age in years into “child, teen, adult, senior”.*
4.  Compute the proportion and counts in each category of one
    categorical variable across the groups of another categorical
    variable from your data. Do not use the function `table()`!

**Graphing:**

6.  Create a graph of your choosing, make one of the axes logarithmic,
    and format the axes labels so that they are “pretty” or easier to
    read.
7.  Make a graph where it makes sense to customize the alpha
    transparency.

Using variables and/or tables you made in one of the “Summarizing”
tasks:

8.  Create a graph that has at least two geom layers.
9.  Create 3 histograms, with each histogram having different sized
    bins. Pick the “best” one and explain why it is the best.

Make sure it’s clear what research question you are doing each operation
for!

<!------------------------- Start your work below ----------------------------->

### Task 1.2

**Question 1: How does diameter of the tree change if there is or is not
a root barrier installed?**

Summarizing Task 1: This task helps determine different measure of
diameter based on whether or not there is a root barrier installed. This
will allow me to see if the diameter differs in terms of multiple
statistics or just one.

``` r
mean_diam_barr<- vancouver_trees %>% group_by(root_barrier) %>% summarise(mean_diameter = mean(diameter))
mean_diam_barr
```

    ## # A tibble: 2 × 2
    ##   root_barrier mean_diameter
    ##   <chr>                <dbl>
    ## 1 N                    12.0 
    ## 2 Y                     4.40

``` r
range_diam_barr<- vancouver_trees %>% group_by(root_barrier) %>% reframe(range_diameter = range(diameter), .groups = 'drop')
range_diam_barr
```

    ## # A tibble: 4 × 3
    ##   root_barrier range_diameter .groups
    ##   <chr>                 <dbl> <chr>  
    ## 1 N                       0   drop   
    ## 2 N                     435   drop   
    ## 3 Y                       0.5 drop   
    ## 4 Y                      86   drop

``` r
med_diam_barr<- vancouver_trees %>% group_by(root_barrier) %>% summarise(med_diameter = median(diameter))
med_diam_barr
```

    ## # A tibble: 2 × 2
    ##   root_barrier med_diameter
    ##   <chr>               <dbl>
    ## 1 N                      10
    ## 2 Y                       3

``` r
max_diam_barr<- vancouver_trees %>% group_by(root_barrier) %>% summarise(max_diameter = max(diameter))
max_diam_barr
```

    ## # A tibble: 2 × 2
    ##   root_barrier max_diameter
    ##   <chr>               <dbl>
    ## 1 N                     435
    ## 2 Y                      86

``` r
min_diam_barr<- vancouver_trees %>% group_by(root_barrier) %>% summarise(min_diameter = min(diameter))
min_diam_barr
```

    ## # A tibble: 2 × 2
    ##   root_barrier min_diameter
    ##   <chr>               <dbl>
    ## 1 N                     0  
    ## 2 Y                     0.5

Graphing Task 2: I created a bar graph to show the difference in mean
diameter when comparing whether a tree has a root barrier or not. The
bar graph also includes labels on each bar, which denote the diameter.

``` r
ggplot(mean_diam_barr, aes(root_barrier, mean_diameter)) + geom_col() + geom_text(aes(label = mean_diameter)) + xlab("Presence of Root Barrier") + ylab("Mean Diameter") + ggtitle("Mean Diameter Based on Presence of Root Barrier")
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

**Question 2: Does the number of trees in each neigborhood differ?**

Summarizing Task 2: I chose this task because it would allow me to see
how many trees have been planted in each neighborhood.

``` r
trees_by_neigh <- vancouver_trees %>% group_by(neighbourhood_name) %>% summarise(n = n())
trees_by_neigh
```

    ## # A tibble: 22 × 2
    ##    neighbourhood_name           n
    ##    <chr>                    <int>
    ##  1 ARBUTUS-RIDGE             5169
    ##  2 DOWNTOWN                  5159
    ##  3 DUNBAR-SOUTHLANDS         9415
    ##  4 FAIRVIEW                  4002
    ##  5 GRANDVIEW-WOODLAND        6703
    ##  6 HASTINGS-SUNRISE         10547
    ##  7 KENSINGTON-CEDAR COTTAGE 11042
    ##  8 KERRISDALE                6936
    ##  9 KILLARNEY                 6148
    ## 10 KITSILANO                 8115
    ## # ℹ 12 more rows

Graphing Task 2: I chose this task because it will easily allow me to
see how many trees are in each neighbourhood in Vancouver. The text at
the bar is helpful to see the true value when two neighborhoods are
similar in number.

``` r
ggplot(trees_by_neigh, aes(neighbourhood_name, n)) + geom_col() + geom_text(aes(label = n)) + xlab("Neighbourhood") + ylab("Number of Trees") + ggtitle("Number of Trees in Vancouver Neighbourhoods") + theme(axis.text.x = element_text(angle = 90)) 
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

**Question 3: How many small, medium, and large trees are there in each
neighborhood?**

(I changed this question from Milestone 1, the question from Milestone 1
is specified in the task above. I changed this question because I wanted
to do Task 3 with my dataset but I wouldn’t be able to do it with the
original question.)

Summarizing Task 3: I chose this task to see what the distribution of
tree size is within a neighbourhood and if certain neighborhoods have
smaller or larger trees based on diameter.

``` r
vancouver_trees3 <- vancouver_trees %>% filter(na.rm = TRUE) %>% group_by(neighbourhood_name) %>% mutate(diameter_cat = cut(diameter,
breaks = c(0, 14, 19, 20), labels = c("small", "medium", "large"), include.lowest = TRUE)) 
```

Graphing Task 3: I chose this task because I wanted to see which
category has the most trees, and the spread of diameter in each
category. I did a google search to see what typical small, medium, and
large trees are sized at. I like this way of looking at the data because
I am able to see the tree in the data set that weren’t considered small,
medium, or large.

I think the best histogram is the one with 100 bins because it is more
clear if there is a range of diameters within the categories small,
medium and large.

``` r
ggplot(vancouver_trees3, aes(diameter, after_stat(density) )) +
   geom_histogram(bins = 30) + facet_wrap(('diameter_cat')) + scale_x_continuous(name="Diameter", limits=c(0, 30)) 
```

    ## Warning: Removed 6282 rows containing non-finite values (`stat_bin()`).

    ## Warning: Removed 8 rows containing missing values (`geom_bar()`).

![](mini-project-2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

``` r
ggplot(vancouver_trees3, aes(diameter, after_stat(density) )) +
   geom_histogram(bins = 5) + facet_wrap(('diameter_cat')) + scale_x_continuous(name="Diameter", limits=c(0, 30))
```

    ## Warning: Removed 6282 rows containing non-finite values (`stat_bin()`).
    ## Removed 8 rows containing missing values (`geom_bar()`).

![](mini-project-2_files/figure-gfm/unnamed-chunk-7-2.png)<!-- -->

``` r
ggplot(vancouver_trees3, aes(diameter, after_stat(density) )) +
   geom_histogram(bins = 100) + facet_wrap(('diameter_cat')) + scale_x_continuous(name="Diameter", limits=c(0, 30))
```

    ## Warning: Removed 6282 rows containing non-finite values (`stat_bin()`).
    ## Removed 8 rows containing missing values (`geom_bar()`).

![](mini-project-2_files/figure-gfm/unnamed-chunk-7-3.png)<!-- -->

**Question 4: How many trees of the Alba species were planted every
year?**

(I changed this question from Milestone 1, the question from Milestone 1
is specified in the task above. I changed this question because I wanted
to make a graph using a geom_path and this option made the most choice,
to see how a certain species may be planted over the years and if there
is a visible trend.)

Summarizing Task 4: I chose this task to first count the number of trees
in each species which would have to be done in order to further figure
out and plot how many trees were planted per year.

``` r
vancouver_trees %>% group_by(species_name) %>% summarise(n = n())
```

    ## # A tibble: 283 × 2
    ##    species_name       n
    ##    <chr>          <int>
    ##  1 ABIES            139
    ##  2 ACERIFOLIA   X  1724
    ##  3 ACUMINATA          7
    ##  4 ACUTISSIMA       526
    ##  5 AILANTHIFOLIA      5
    ##  6 ALBA              26
    ##  7 ALBA-SINENSIS      3
    ##  8 ALNIFOLIA        274
    ##  9 ALPINUM            1
    ## 10 ALTISSIMA          4
    ## # ℹ 273 more rows

Graphing Task 4: I chose this task because I wanted to see how the trend
in how many trees are planted per year changes. I think it may have been
interesting to see how many trees were planted in general per year and
to see if that number has grown to counteract the effects of climate
change.

``` r
alba_per_yr <- vancouver_trees %>% filter(species_name == "ALBA") %>% group_by(date_planted) %>% summarise(n = n())

ggplot(alba_per_yr, aes(date_planted, n )) + scale_x_continuous(name="Year") + scale_y_continuous(name="Number of Trees Planted", limits = c(0,3)) + geom_point() + geom_path()
```

    ## Warning: Removed 1 rows containing missing values (`geom_point()`).

    ## Warning: Removed 1 row containing missing values (`geom_path()`).

![](mini-project-2_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->
<!----------------------------------------------------------------------------->

### 1.3 (2 points)

Based on the operations that you’ve completed, how much closer are you
to answering your research questions? Think about what aspects of your
research questions remain unclear. Can your research questions be
refined, now that you’ve investigated your data a bit more? Which
research questions are yielding interesting results?

<!------------------------- Write your answer here ---------------------------->

I am much closer to answering my research questions. I changed some of
my questions to try to complete the tasks from the lists before I
realized I could have kept my questions the same, but gone about the
tasks differently. So I did refine the questions that I had based on the
tasks. If I didn’t change the questions before, I probably would have
refined the questions at this point. I think seeing the trend per year
for how many Alba trees were grown was interesting. I think seeing how
many trees are planted per year in general will be more interesting.
Also, it was specifed to use dplyr and ggplot for Task 1, so I hope to
use the tidyverse data to clean up the data. One thing I can do is
separate the date_planted column into day month and year, to get a more
accurate version of the number of trees planted per year. Also, it was
interesting to see that having a root barrier may be causing a smaller
diameter of the tree on average. For this question, I don’t think I
would want to refine it. Using density plots are a bit harder for this
dataset as there are quite variable values entered for some columns.
Possibly filtering the data further will allow for more informative
visuals.

<!----------------------------------------------------------------------------->

# Task 2: Tidy your data

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

- Each row is an **observation**
- Each column is a **variable**
- Each cell is a **value**

### 2.1 (2 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

1.  date_planted: This isn’t as tidy as can be for my needs because it
    is hard to use year as a variable.

2.  species_name: I would say this is tidy because it is separate from
    the genus_name column, and it would allow the user to filter for a
    specific species or genus if needed.

3.  tree_id: This seems tidy as each observation is given a tree id,
    which makes each row an observation.

4.  root_barrier: This is tidy because it is a yes or no column
    containing a single variable.

5.  std_street: This entry could be split into three columns to divide
    the cardinal direction, street name, and the type of street it is,
    whether its a Avenue, Street, or Road etc.

6.  neighbourhood_name: This seems tidy because there is consistency
    between how each neighbourhood is specified as each neighbourhood
    has its own string.

7.  curb: Thisseems tidy enough because there is a Yes or No only for
    this variable/column.

8.  cultivar_name: This may not be tidy because the common_name also
    includes this data. The data in common_name can probably be split to
    avoid the double presence of the cultivar_name.

Overall, the dataset is tidy, but it is untidy in some cases depending
on the research question. Depending on the user’s needs, they can make
the data more tidy if needed. Each row is an observation as there is a
tree id for each entry. Each column is a variable, and each cell has
value unless it is specified as NA where there is no record of that
variable for that observation.
<!----------------------------------------------------------------------------->

### 2.2 (4 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

I want to make the data more tidy by separating the date_planted column
into day_planted, month_planted, and year_planted. This will be useful
for me because I would like to see how the number of trees planted per
year changes, and this was doable but not as straightforward as when the
columns for the date are separated out and year is more accessible.

``` r
vancouver_trees_datesep <- vancouver_trees %>% separate(col = date_planted, into = c('year_planted', 'month_planted', 'day_planted'))
vancouver_trees_datesep
```

    ## # A tibble: 146,611 × 22
    ##    tree_id civic_number std_street    genus_name species_name cultivar_name  
    ##      <dbl>        <dbl> <chr>         <chr>      <chr>        <chr>          
    ##  1  149556          494 W 58TH AV     ULMUS      AMERICANA    BRANDON        
    ##  2  149563          450 W 58TH AV     ZELKOVA    SERRATA      <NA>           
    ##  3  149579         4994 WINDSOR ST    STYRAX     JAPONICA     <NA>           
    ##  4  149590          858 E 39TH AV     FRAXINUS   AMERICANA    AUTUMN APPLAUSE
    ##  5  149604         5032 WINDSOR ST    ACER       CAMPESTRE    <NA>           
    ##  6  149616          585 W 61ST AV     PYRUS      CALLERYANA   CHANTICLEER    
    ##  7  149617         4909 SHERBROOKE ST ACER       PLATANOIDES  COLUMNARE      
    ##  8  149618         4925 SHERBROOKE ST ACER       PLATANOIDES  COLUMNARE      
    ##  9  149619         4969 SHERBROOKE ST ACER       PLATANOIDES  COLUMNARE      
    ## 10  149625          720 E 39TH AV     FRAXINUS   AMERICANA    AUTUMN APPLAUSE
    ## # ℹ 146,601 more rows
    ## # ℹ 16 more variables: common_name <chr>, assigned <chr>, root_barrier <chr>,
    ## #   plant_area <chr>, on_street_block <dbl>, on_street <chr>,
    ## #   neighbourhood_name <chr>, street_side_name <chr>, height_range_id <dbl>,
    ## #   diameter <dbl>, curb <chr>, year_planted <chr>, month_planted <chr>,
    ## #   day_planted <chr>, longitude <dbl>, latitude <dbl>

Now, I can put the componenets of the date back together.

``` r
vancouver_trees_datetog <- vancouver_trees_datesep %>% unite(col = date_planted, c(year_planted, month_planted, day_planted), sep = "-")
vancouver_trees_datetog
```

    ## # A tibble: 146,611 × 20
    ##    tree_id civic_number std_street    genus_name species_name cultivar_name  
    ##      <dbl>        <dbl> <chr>         <chr>      <chr>        <chr>          
    ##  1  149556          494 W 58TH AV     ULMUS      AMERICANA    BRANDON        
    ##  2  149563          450 W 58TH AV     ZELKOVA    SERRATA      <NA>           
    ##  3  149579         4994 WINDSOR ST    STYRAX     JAPONICA     <NA>           
    ##  4  149590          858 E 39TH AV     FRAXINUS   AMERICANA    AUTUMN APPLAUSE
    ##  5  149604         5032 WINDSOR ST    ACER       CAMPESTRE    <NA>           
    ##  6  149616          585 W 61ST AV     PYRUS      CALLERYANA   CHANTICLEER    
    ##  7  149617         4909 SHERBROOKE ST ACER       PLATANOIDES  COLUMNARE      
    ##  8  149618         4925 SHERBROOKE ST ACER       PLATANOIDES  COLUMNARE      
    ##  9  149619         4969 SHERBROOKE ST ACER       PLATANOIDES  COLUMNARE      
    ## 10  149625          720 E 39TH AV     FRAXINUS   AMERICANA    AUTUMN APPLAUSE
    ## # ℹ 146,601 more rows
    ## # ℹ 14 more variables: common_name <chr>, assigned <chr>, root_barrier <chr>,
    ## #   plant_area <chr>, on_street_block <dbl>, on_street <chr>,
    ## #   neighbourhood_name <chr>, street_side_name <chr>, height_range_id <dbl>,
    ## #   diameter <dbl>, curb <chr>, date_planted <chr>, longitude <dbl>,
    ## #   latitude <dbl>

<!----------------------------------------------------------------------------->

### 2.3 (4 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the remaining tasks:

<!-------------------------- Start your work below ---------------------------->

1.  How has the number of trees planted changed per year per
    neighbourhood over the years that this data has been collected?
2.  How many different genuses of trees are there in each neighbourhood?

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

1.  For this question, it stems from my curiosity of trends in tree
    planting and its benefit for climate change. Also, I think it would
    be interesting to see if there are more trees planted in different
    neighbourhoods or if each neighbourhood plants the same amount of
    trees. If one neighbourhood already has a lot of trees, this visual
    will show that that neighbourhood already has enough trees planted.
2.  I picked this question because I am curious to see how the diversity
    of trees in each neighbourhood may vary.For my inital question, I
    graphed how many trees were in each neighbourhood, but determining
    the diversity is more indicative of diversity of environments.
    <!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidy’ing,
dropping irrelevant columns, etc.).

(If it makes more sense, then you can make/pick two versions of your
data, one for each research question.)

<!--------------------------- Start your work below --------------------------->

Data Set for Question 1

``` r
vancouver_trees_q1 <- vancouver_trees %>% separate(col = date_planted, into = c('year_planted', 'month_planted', 'day_planted')) %>% group_by(neighbourhood_name, year_planted) %>% summarise(n = n())
```

    ## `summarise()` has grouped output by 'neighbourhood_name'. You can override
    ## using the `.groups` argument.

``` r
vancouver_trees_q1
```

    ## # A tibble: 693 × 3
    ## # Groups:   neighbourhood_name [22]
    ##    neighbourhood_name year_planted     n
    ##    <chr>              <chr>        <int>
    ##  1 ARBUTUS-RIDGE      1989            41
    ##  2 ARBUTUS-RIDGE      1990            76
    ##  3 ARBUTUS-RIDGE      1991            16
    ##  4 ARBUTUS-RIDGE      1992            81
    ##  5 ARBUTUS-RIDGE      1993            18
    ##  6 ARBUTUS-RIDGE      1994            58
    ##  7 ARBUTUS-RIDGE      1995           151
    ##  8 ARBUTUS-RIDGE      1996            95
    ##  9 ARBUTUS-RIDGE      1997            61
    ## 10 ARBUTUS-RIDGE      1998            59
    ## # ℹ 683 more rows

Data Set for Question 2

``` r
vancouver_trees_q2 <- vancouver_trees %>% group_by(neighbourhood_name) %>% summarise(distinct_genus = n_distinct(genus_name))
vancouver_trees_q2
```

    ## # A tibble: 22 × 2
    ##    neighbourhood_name       distinct_genus
    ##    <chr>                             <int>
    ##  1 ARBUTUS-RIDGE                        52
    ##  2 DOWNTOWN                             38
    ##  3 DUNBAR-SOUTHLANDS                    68
    ##  4 FAIRVIEW                             55
    ##  5 GRANDVIEW-WOODLAND                   59
    ##  6 HASTINGS-SUNRISE                     71
    ##  7 KENSINGTON-CEDAR COTTAGE             64
    ##  8 KERRISDALE                           58
    ##  9 KILLARNEY                            55
    ## 10 KITSILANO                            73
    ## # ℹ 12 more rows

# Task 3: Modelling

## 3.0 (no points)

Pick a research question from 1.2, and pick a variable of interest
(we’ll call it “Y”) that’s relevant to the research question. Indicate
these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: How does diameter of the tree change if there is
or is not a root barrier installed?

**Variable of interest**: diameter

<!----------------------------------------------------------------------------->

## 3.1 (3 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

- **Note**: It’s OK if you don’t know how these models/tests work. Here
  are some examples of things you can do here, but the sky’s the limit.

  - You could fit a model that makes predictions on Y using another
    variable, by using the `lm()` function.
  - You could test whether the mean of Y equals 0 using `t.test()`, or
    maybe the mean across two groups are different using `t.test()`, or
    maybe the mean across multiple groups are different using `anova()`
    (you may have to pivot your data for the latter two).
  - You could use `lm()` to test for significance of regression
    coefficients.

<!-------------------------- Start your work below ---------------------------->

``` r
model_diam <- lm(diameter ~ root_barrier, vancouver_trees)
print(model_diam)
```

    ## 
    ## Call:
    ## lm(formula = diameter ~ root_barrier, data = vancouver_trees)
    ## 
    ## Coefficients:
    ##   (Intercept)  root_barrierY  
    ##        11.962         -7.562

<!----------------------------------------------------------------------------->

## 3.2 (3 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

- Be sure to indicate in writing what you chose to produce.
- Your code should either output a tibble (in which case you should
  indicate the column that contains the thing you’re looking for), or
  the thing you’re looking for itself.
- Obtain your results using the `broom` package if possible. If your
  model is not compatible with the broom function you’re needing, then
  you can obtain your results by some other means, but first indicate
  which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

After using the linear regression model for my two variables, I wanted
to know if there was a p-value associated with the model fitting. I’m
not exactly sure how this model worked with the inputted data, so I’m
not sure why the p-value is 0.

``` r
fitted_diam <- broom::tidy(model_diam)
print(fitted_diam)
```

    ## # A tibble: 2 × 5
    ##   term          estimate std.error statistic p.value
    ##   <chr>            <dbl>     <dbl>     <dbl>   <dbl>
    ## 1 (Intercept)      12.0     0.0243     491.        0
    ## 2 root_barrierY    -7.56    0.0974     -77.6       0

<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 4.1 (3 points)

Take a summary table that you made from Task 1, and write it as a csv
file in your `output` folder. Use the `here::here()` function.

- **Robustness criteria**: You should be able to move your Mini Project
  repository / project folder to some other location on your computer,
  or move this very Rmd file to another location within your project
  repository / folder, and your code should still work.
- **Reproducibility criteria**: You should be able to delete the csv
  file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
path_output_1 <- here("Output")
write_csv(trees_by_neigh, file = here::here(path_output_1, "mda_task4.3.csv"))
```

<!----------------------------------------------------------------------------->

## 4.2 (3 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

- The same robustness and reproducibility criteria as in 4.1 apply here.

<!-------------------------- Start your work below ---------------------------->

``` r
path_output_1 <- here("Output")
saveRDS(fitted_diam, file = here::here(path_output_1, "fitted_diam.rds"))
fitted_diam_2 <- readRDS("/Users/raveenbadyal/MDA Raveen Badyal/Output/fitted_diam.rds")
fitted_diam_2
```

    ## # A tibble: 2 × 5
    ##   term          estimate std.error statistic p.value
    ##   <chr>            <dbl>     <dbl>     <dbl>   <dbl>
    ## 1 (Intercept)      12.0     0.0243     491.        0
    ## 2 root_barrierY    -7.56    0.0974     -77.6       0

<!----------------------------------------------------------------------------->

# Overall Reproducibility/Cleanliness/Coherence Checklist

Here are the criteria we’re looking for.

## Coherence (0.5 points)

The document should read sensibly from top to bottom, with no major
continuity errors.

The README file should still satisfy the criteria from the last
milestone, i.e. it has been updated to match the changes to the
repository made in this milestone.

## File and folder structure (1 points)

You should have at least three folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (1 point)

All output is recent and relevant:

- All Rmd files have been `knit`ted to their output md files.
- All knitted md files are viewable without errors on Github. Examples
  of errors: Missing plots, “Sorry about that, but we can’t show files
  that are this big right now” messages, error messages from broken R
  code
- All of these output files are up-to-date – that is, they haven’t
  fallen behind after the source (Rmd) files have been updated.
- There should be no relic output files. For example, if you were
  knitting an Rmd to html, but then changed the output to be only a
  markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

## Tagged release (0.5 point)

You’ve tagged a release for Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
