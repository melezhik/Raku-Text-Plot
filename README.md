# Raku Text::Plot

[![SparkyCI](http://ci.sparrowhub.io/project/gh-antononcube-Raku-Text-Plot/badge)](http://ci.sparrowhub.io)

This repository has a Raku package for textual (terminal) plots.

Here is the list of functions:

- [X] `text-list-plot`
- [ ] `text-plot`
- [ ] `text-bar-chart`
- [ ] `text-pareto-principle-plot`

***Currently only `text-list-plot` is implemented.***

It would be nice to also have the functions:

- [ ] `text-density-plot`
- [ ] `text-box-plot`

But that would require dependency on a certain statistical package.
(I think it is best to keep this package simple.)

-------

## Installation

From zef-ecosystem:

```
zef install Text::Plot
```

From GitHub:

```
zef install https://github.com/antononcube/Raku-Text-Plot.git
```

------

## Usage examples

### `text-list-plot`

Simple plot with y-values only:

```perl6
use Text::Plot;
say text-list-plot((^30)>>.sqrt);
```
```
# +---+-------+--------+--------+--------+--------+--------+-+      
# |                                                          |      
# +                                               * * * **   +  5.00
# |                                      * * * **            |      
# +                               * * **                     +  4.00
# |                        * * **                            |      
# +                   ** *                                   +  3.00
# |               * *                                        |      
# +          ** *                                            +  2.00
# |        *                                                 |      
# +    * *                                                   +  1.00
# |                                                          |      
# +   *                                                      +  0.00
# |                                                          |      
# +---+-------+--------+--------+--------+--------+--------+-+      
#     0.00    5.00     10.00    15.00    20.00    25.00    30.00
```

Plot using both x- and y-values, and with specified axes labels, y-tick-labels format, and plot width, height, and
title:

```perl6
my @xs = (0, 0.2 ... 5);
say text-list-plot(@xs, @xs>>.sin,
        x-label => 'x-points',
        y-label => 'value',
        y-tick-labels-format => '%10.2e',
        width => 80,
        height => 18,
        title => 'SINE PLOT');
```
```
# SINE PLOT                                    
# +---+-------------+-------------+-------------+-------------+-------------+----+            
# |                                                                              |            
# +                    *  *  *  * *                                              +  1.00e+00  
# |                 *                *                                           |            
# |               *                     *                                        |            
# +            *                           *                                     +  5.00e-01  
# |         *                                 *                                  |           v
# |      *                                                                       |           a
# |                                             *                                |           l
# +   *                                            *                             +  0.00e+00 u
# |                                                   *                          |           e
# |                                                                              |            
# +                                                      *                       + -5.00e-01  
# |                                                         *                    |            
# |                                                           *  *               |            
# +                                                                 *  *  * *    + -1.00e+00  
# |                                                                              |            
# +---+-------------+-------------+-------------+-------------+-------------+----+            
#     0.00          1.00          2.00          3.00          4.00          5.00            
#                                     x-points
```

Smallish plot with custom point character spec:

```perl6
my @xs = (0, 0.05 ... 10);
say text-list-plot(@xs, -1 <<*>> @xs>>.sqrt,
        point-char => '·',
        width => 40,
        height => 12);
```
```
# +--+-----+------+------+------+-----+--+      
# +  ·                                   +  0.00
# |  ·                                   |      
# +  ···                                 + -0.50
# +    ···                               + -1.00
# +       ····                           + -1.50
# |          ······                      |      
# +               ······                 + -2.00
# +                    ········          + -2.50
# +                           ········   + -3.00
# +                                   ·  + -3.50
# +--+-----+------+------+------+-----+--+      
#    0.00  2.00   4.00   6.00   8.00  10.00
```

Plot a list of two-element lists:

```perl6
say text-list-plot((^@xs.elems Z @xs>>.cos).List, title => 'Some list of lists'),
```
```
# Some list of lists                     
# +---+------------+-----------+------------+------------+---+      
# |                                                          |      
# +   ***                          *******                   +  1.00
# |      **                       **     ***                 |      
# +       **                    ***        **                +  0.50
# |        **                  **           **               |      
# |         **                 *             **              |      
# +          **               *               *              +  0.00
# |           **             *                 *             |      
# |            **           *                   *            |      
# +             **         *                     **          + -0.50
# |              ***     **                       **         |      
# +                *******                         *******   + -1.00
# |                                                          |      
# +---+------------+-----------+------------+------------+---+      
#     0.00         50.00       100.00       150.00       200.00
```

Here is a more complicated example using a randomly generated dataset, [AAp1, AAp2]:

```perl6
use Data::Generators;
use Data::Summarizers;
my @dsRand = random-tabular-dataset(70, <x y>,
        generators => [{ random-variate(NormalDistribution.new(4, 2), $_) },
                       { random-variate(NormalDistribution.new(12, 3), $_) }]);
records-summary(@dsRand);
```
```
# +-------------------------------+------------------------------+
# | x                             | y                            |
# +-------------------------------+------------------------------+
# | Min    => 0.32408825955040177 | Min    => 5.9931871245751775 |
# | 1st-Qu => 2.7843876207792415  | 1st-Qu => 9.876678419163488  |
# | Mean   => 4.08462575674799    | Mean   => 11.745888178167041 |
# | Median => 4.0537648984346815  | Median => 11.802070475718413 |
# | 3rd-Qu => 5.234788626034089   | 3rd-Qu => 13.148085341313088 |
# | Max    => 10.57130574492772   | Max    => 18.389289264928514 |
# +-------------------------------+------------------------------+
```

```perl6
text-list-plot(@dsRand.map({ $_<x y> })>>.List,
        x-limit => (-2, 10), y-limit => (0, 25),
        title => 'Random Normal distribution variates')
```
```
# Random Normal distribution variates             
# ++---------+--------+---------+--------+---------+--------++       
# +                                                          +  25.00
# |                                                          |       
# +                                                          +  20.00
# |                        * *               *               |       
# |                          *     * *   *                   |       
# +                *     * *   **  **   **                   +  15.00
# |             * *    * * * ***** * * * * **   *            | *     
# +           *  *  *  * ** **  * * * *   *                  +  10.00
# |             *       * **      * *    *  *                |       
# |                *   *  *      * *     *                   |       
# +                                                          +   5.00
# |                                                          |       
# +                                                          +   0.00
# ++---------+--------+---------+--------+---------+--------++       
#  -2.00     0.00     2.00      4.00     6.00      8.00     10.00
```

**Remark:** The function `text-list-plot` has camel case aliases for the multi-word named arguments.
For example, `xLimit` for `x-limit` and `xTickLabelsFormat` for `x-tick-labels-format`.


Here is an example of a multi-list plot:

```perl6
say text-list-plot([([1,1], [2,5], [3,2], [4,5]),
                    ([1,1], [3,3], [3,2]),
                    ([1,3], [2,1], [5,2])], point-char => Whatever);
```
```
# +---+------------+-----------+------------+------------+---+      
# |                                                          |      
# +                *                        *                +  5.00
# |                                                          |      
# +                                                          +  4.00
# |                                                          |      
# |                                                          |      
# +   ▽                        □                             +  3.00
# |                                                          |      
# |                                                          |      
# +                            □                         ▽   +  2.00
# |                                                          |      
# +   □            ▽                                         +  1.00
# |                                                          |      
# +---+------------+-----------+------------+------------+---+      
#     1.00         2.00        3.00         4.00         5.00
```

**Remark:** Note that the points `[1,1]` and `[3,2]` of the second list overlay the same points of first list.

### `text-pareto-principle-plot`

Assume we have a data vector with all numeric or with all string elements.
The adherence of the data vector to the Pareto principle can be easily verified with the plots of
`text-pareto-principle-plot`. 

Here is an example with a numeric vector: 

```perl6
text-pareto-principle-plot( random-real(10, 300), title => 'Random reals')
```
```
# Random reals                        
#     0.00    0.17     0.33     0.50    0.67     0.83    1.00 
# +---+-------+--------+--------+-------+--------+-------+---+      
# |                                                          |      
# +                                        ***************   +  1.00
# |                                  *******                 |      
# +                             ******                       +  0.80
# |                        ******                            |      
# +                     ****                                 +  0.60
# |                 ****                                     |      
# +              ****                                        +  0.40
# |           ****                                           |      
# +        ****                                              +  0.20
# |     ****                                                 |      
# |   ***                                                    |      
# +                                                          +  0.00
# +---+-------+--------+--------+-------+--------+-------+---+      
#     0.00    50.00    100.00   150.00  200.00   250.00  300.00
```

Here is an example with a vector of strings: 

```perl6
text-pareto-principle-plot( random-pet-name(500), title => 'Random pet names')
```
```
# Random pet names                      
#     0.00     0.19      0.37      0.56     0.75      0.94    
# +---+--------+---------+---------+--------+---------+------+      
# |                                                          |      
# +                                               ********   +  1.00
# |                                       *********          |      
# +                              *********                   +  0.80
# |                      *********                           |      
# +               ********                                   +  0.60
# |           ****                                           |      
# |       ****                                               |      
# +     ***                                                  +  0.40
# |   ***                                                    |      
# +   *                                                      +  0.20
# |   *                                                      |      
# +                                                          +  0.00
# +---+--------+---------+---------+--------+---------+------+      
#     0.00     50.00     100.00    150.00   200.00    250.00
```

-------

## Command Line Interface (CLI)

The package function `text-list-plot` can be used through the corresponding CLI:

```shell
text-list-plot --help
```
```
# Usage:
#   text-list-plot [<points> ...] [-p|--point-char=<Str>] [-w|--width[=Int]] [-h|--height[=Int]] [-t|--title=<Str>] [--xLabel|--x-label=<Str>] [--yLabel|--y-label=<Str>] [--xTickLabelsFormat|--x-tick-labels-format=<Str>] [--yTickLabelsFormat|--y-tick-labels-format=<Str>] -- Makes textual (terminal) plots.
#   text-list-plot <words> [-p|--point-char=<Str>] [-w|--width[=Int]] [-h|--height[=Int]] [-t|--title=<Str>] [--xLabel|--x-label=<Str>] [--yLabel|--y-label=<Str>] [--xTickLabelsFormat|--x-tick-labels-format=<Str>] [--yTickLabelsFormat|--y-tick-labels-format=<Str>] -- Makes textual (terminal) plots by splitting a string of data points.
#   text-list-plot [-p|--point-char=<Str>] [-w|--width[=Int]] [-h|--height[=Int]] [-t|--title=<Str>] [--xLabel|--x-label=<Str>] [--yLabel|--y-label=<Str>] [--xTickLabelsFormat|--x-tick-labels-format=<Str>] [--yTickLabelsFormat|--y-tick-labels-format=<Str>] -- Makes textual (terminal) plots from pipeline input
#   
#     [<points> ...]                                      Data points.
#     -p|--point-char=<Str>                               Plot points character. [default: '*']
#     -w|--width[=Int]                                    Width of the plot. (-1 for Whatever.) [default: -1]
#     -h|--height[=Int]                                   Height of the plot. (-1 for Whatever.) [default: -1]
#     -t|--title=<Str>                                    Title of the plot. [default: '']
#     --xLabel|--x-label=<Str>                            Label of the X-axis. If Whatever, then no label is placed. [default: '']
#     --yLabel|--y-label=<Str>                            Label of the Y-axis. If Whatever, then no label is placed. [default: '']
#     --xTickLabelsFormat|--x-tick-labels-format=<Str>    X-axis tick labels format. [default: '']
#     --yTickLabelsFormat|--y-tick-labels-format=<Str>    Y-axis tick labels format. [default: '']
#     <words>                                             String with data points.
```

Here is an example of a simple, y-axis values only call:

```shell
text-list-plot 33 12 21 10 3 4 
```
```
# +---+-------------+------------+-------------+------------+------------+----+         
# +                                                                           +  35.00  
# |   *                                                                       |         
# +                                                                           +  30.00  
# |                                                                           |         
# +                                                                           +  25.00  
# |                              *                                            |         
# +                                                                           +  20.00  
# |                                                                           |         
# +                                                                           +  15.00  
# |                 *                                                         |         
# +                                            *                              +  10.00  
# |                                                                           |         
# +                                                         *            *    +   5.00  
# |                                                                           |         
# +---+-------------+------------+-------------+------------+------------+----+         
#     0.00          1.00         2.00          3.00         4.00         5.00
```

Here is an example of 2D points call:

```shell
text-list-plot "22,32 10,39 13,32 14,20"
```
```
# +---+-----------+----------+----------+----------+----------+----------+----+         
# +                                                                           +  40.00  
# |   *                                                                       |         
# |                                                                           |         
# +                                                                           +  35.00  
# |                                                                           |         
# |                    *                                                 *    |         
# +                                                                           +  30.00  
# |                                                                           |         
# |                                                                           |         
# +                                                                           +  25.00  
# |                                                                           |         
# |                                                                           |         
# +                          *                                                +  20.00  
# |                                                                           |         
# +---+-----------+----------+----------+----------+----------+----------+----+         
#     10.00       12.00      14.00      16.00      18.00      20.00      22.00
```

Here is an example pipeline:

```shell
raku -e 'say (^1000).roll(21)' | text-list-plot
```
```
# +---+----------+-----------+-----------+-----------+-------+           
# +                                                          +  1000.00  
# |       *         *               *             *          |           
# |                                                          |           
# +                                      *                   +   800.00  
# |            *                                     *       |           
# +     *                    *                               +   600.00  
# |          *        *                                      |           
# +                            *           *    *            +   400.00  
# |                               *                          |           
# +              *      *  *                                 +   200.00  
# |                                           *              |           
# +   *                               *                * *   +     0.00  
# |                                                          |           
# +---+----------+-----------+-----------+-----------+-------+           
#     0.00       5.00        10.00       15.00       20.00
```

**Remark:** Attempt is made plot's width and height are determined automatically, using terminal's number of columns and
lines. If that fails `width=60` is used. In the pipeline example above `text-list-plot` fails to automatically determine
the width and height. (The other example do succeed.)

-------

## Implementation notes

- The package functions and their signatures design are easy to come up with, but it is very helpful to have a "good
  example" to follow.

    - I consider the R-package "txtplot", [BB1], to be such good example.

    - There are at least three Python packages for text plots, but only tried them out once. None was as complete and "nice"
      as the R-package "txtplot".

- The points and ticks are rescaled with a version of the Mathematica-function
  [`Rescale`](https://reference.wolfram.com/language/ref/Rescale.html).

- The axes ticks are computed with a version of the R-function
  [`pretty`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/pretty.html).

-------

## TODO

- [X] Plotting a list of two-element lists.

- [X] Optional tick labels format specs.

- [X] CLI design and implementation.

- [X] Make use kebab-case for named arguments and make corresponding camel-case aliases.

- [X] Multi-list plot support.

- [X] Plot title.

- [ ] Proper respect of width and height.

    - Currently, the width and height are for the plot frame -- title, axes- and tick labels are "extra."

- [ ] Make the axes ticks to be on the left.

    - It was just much easier to put them on the right.

    - BTW, this is probably a bug -- the width of the "total plot" is larger than the specified.

- [ ] Optional placement of tick values.

- [ ] `text-pareto-principle-plot`

- [ ] `text-plot`

    - Easy to implement inlined with `text-plot`, but it might give a simpler interface.

- [ ] `text-bar-chart`

- [ ] `text-histogram`


-------

## References

[AAp0] Anton Antonov,
[Text::Plot Raku package](https://github.com/antononcube/Raku-Text-Plot),
(2022),
[GitHub/antononcube](https://github.com/antononcube).

[AAp1] Anton Antonov,
[Data::Generators Raku package](https://github.com/antononcube/Raku-Data-Generators),
(2021),
[GitHub/antononcube](https://github.com/antononcube).

[AAp2] Anton Antonov,
[Data::Summarizers Raku package](https://github.com/antononcube/Raku-Data-Summarizers),
(2021),
[GitHub/antononcube](https://github.com/antononcube).

[BB1] Bjoern Bornkamp,
[txtplot R package](https://github.com/bbnkmp/txtplot),
([CRAN](https://github.com/cran/txtplot)),
(2020),
[GitHub/bbnkmp](https://github.com/bbnkmp).
