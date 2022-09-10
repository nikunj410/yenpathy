
<!-- README.md is generated from README.Rmd. Please edit that file -->

# yenpathy

[![License:
Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Travis build
status](https://travis-ci.org/ecohealthalliance/yenpathy.svg?branch=master)](https://travis-ci.org/ecohealthalliance/yenpathy)
[![Codecov test
coverage](https://codecov.io/gh/ecohealthalliance/yenpathy/branch/master/graph/badge.svg)](https://codecov.io/gh/ecohealthalliance/yenpathy?branch=master)

## Overview

**yenpathy** is an R package to quickly find *k* shortest paths through
a weighted directed graph using [Yen’s
Algorithm](https://dx.doi.org/10.1287/mnsc.17.11.712). This algorithm
has numerous applications in network analysis, such as transportation
planning.

There are already comprehensive network analysis packages in R, notably
[**igraph**](http://igraph.org/r/) and its tidyverse-compatible
interface [**tidygraph**](https://github.com/thomasp85/tidygraph).
**yenpathy** complements these by doing one thing well.

**yenpathy** provides the function `k_shortest_paths()`, which returns
*k* shortest paths between two nodes in a graph, ordered from shortest
to longest, with length determined by the sum of weights of consecutive
edges.

For more examples and documentation, visit this repo’s [GitHub Pages
site](https://ecohealthalliance.github.io/yenpathy/).

## Installation

Install **yenpathy** from GitHub with
[remotes](https://github.com/r-lib/remotes):

``` r
library(remotes)
install_github("https://github.com/nikunj410/yenpathy", build_vignettes = TRUE)
```

## Usage

Pass `k_shortest_paths()` a data frame (`graph_df`) with rows
representing the edges of a graph, as well as a `from` and `to`,
specifying starting and ending nodes in `graph_df`.

``` r
library(nycflights13)
library(yenpathy)

small_graph <- data.frame(
  start = c(1, 4, 5, 1, 1, 8, 1, 2, 7, 3),
  end = c(4, 5, 6, 6, 8, 6, 2, 7, 3, 6),
  weight = c(1, 1, 1.5, 5, 1.5, 2.5, 1.5, 0.5, 0.5, 0.5)
)

k_shortest_paths(small_graph, from = 1, to = 6)
#> [[1]]
#> [1] 1 2 7 3 6
```

The function will return a list containing up to `k` (default 1) vectors
representing paths through the graph.

``` r
k_shortest_paths(small_graph,
                 from = 1, to = 6, k = 4)
#> [[1]]
#> [1] 1 2 7 3 6
#> 
#> [[2]]
#> [1] 1 4 5 6
#> 
#> [[3]]
#> [1] 1 8 6
#> 
#> [[4]]
#> [1] 1 6
```

You can also pass an `edge_penalty`, a number which is added to all of
the edge weights, effectively penalizing paths consisting of more edges.

``` r
k_shortest_paths(small_graph,
                 from = 1,
                 to = 6,
                 k = 4,
                 edge_penalty = 1)
#> [[1]]
#> [1] 1 6
#> 
#> [[2]]
#> [1] 1 8 6
#> 
#> [[3]]
#> [1] 1 4 5 6
#> 
#> [[4]]
#> [1] 1 2 7 3 6
```

Further examples can be found in the Package’s vignette. This can be
found on the package’s GitHub Pages site, or viewed in R;

``` r
browseVignettes("yenpathy")
#> No vignettes found by browseVignettes("yenpathy")
vignette("using-yenpathy")
#> Warning: vignette 'using-yenpathy' not found
```

## Implementation

The package wraps a C++ implementation of Yen’s algorithm created by
[Yan Qi](https://github.com/yan-qi). The original C++ code is available
on GitHub at
[yan-qi/k-shortest-paths-cpp-version](https://github.com/yan-qi/k-shortest-paths-cpp-version).

## Contributing

This project is released with a [Contributor Code of
Conduct](https://github.com/ecohealthalliance/yenpathy/blob/master/CONDUCT.md).
By participating in this project, you agree to abide by its terms.

Feel free to submit feedback, bug reports, or feature suggestions
[here](https://github.com/ecohealthalliance/yenpathy/issues), or to
submit fixes as pull requests.
