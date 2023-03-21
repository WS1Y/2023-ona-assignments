For the following assignment I looked at the network of the bus to
better create social connections.

Based on the review the best seat to choose would be B. This would be
because it is tied with C for degree and closeness centrality, but is
the highest for the betweenness centrality.

However, looking beyond the quantitative, it would be awkward to sit in
seat B when seat C is open. Someone would have to climb over you to sit
in seat C, or it would remain open. Because of this seat C would be the
seat that I would sit in.

``` r
# Load the igraph package
library(igraph)
```

    ## Warning: package 'igraph' was built under R version 4.2.2

    ## 
    ## Attaching package: 'igraph'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     decompose, spectrum

    ## The following object is masked from 'package:base':
    ## 
    ##     union

``` r
#create the nodes
nodes <- data.frame("A", "B", "C", "D",1,2,3,4,5,6)

# Create a data frame of edges
edges <- data.frame(from = c(1, 2, "A", "A", "B", "B", "B", "B", "C", "C", "C", 6, 6, "D", "D", 3, 3),
                    to = c(2, "A", "B", "C", "C", 6, "D", 3, "D", 3, 4, "D", 5, 3, 5, 4, 5))
```

``` r
# Create a graph object
graph <- graph_from_data_frame(edges, directed = FALSE)
plot(graph, vertex.label = V(graph)$name)
```

![](Assignment_2_files/figure-markdown_github/unnamed-chunk-2-1.png)

``` r
# Calculate degree centrality
degree_centrality <- degree(graph)

# Calculate closeness centrality
closeness_centrality <- closeness(graph)

# Calculate betweenness centrality
betweenness_centrality <- betweenness(graph)
```

``` r
# Print the results
degree_centrality
```

    ## 1 2 A B C 6 D 3 4 5 
    ## 1 2 3 5 5 3 5 5 2 3

``` r
# Print the results
closeness_centrality
```

    ##          1          2          A          B          C          6          D 
    ## 0.03333333 0.04545455 0.06250000 0.07142857 0.07142857 0.05263158 0.06250000 
    ##          3          4          5 
    ## 0.06250000 0.05000000 0.04761905

``` r
# Print the results
betweenness_centrality
```

    ##          1          2          A          B          C          6          D 
    ##  0.0000000  8.0000000 14.0000000  9.0333333  8.6000000  0.9333333  3.2666667 
    ##          3          4          5 
    ##  4.6333333  0.0000000  0.5333333

``` r
# Add centrality scores as node attributes
V(graph)$degree_centrality <- degree_centrality
V(graph)$closeness_centrality <- closeness_centrality
V(graph)$betweenness_centrality <- betweenness_centrality

# Set node labels to name and centrality scores
V(graph)$label <- sprintf("%s\nDeg: %.2f\nCls: %.2f\nBet: %.2f", V(graph)$name, V(graph)$degree_centrality, V(graph)$closeness_centrality, V(graph)$betweenness_centrality)

# Set layout and adjust parameters to make all labels visible
par(mar = c(0, 0, 0, 0))
plot(graph, vertex.label = V(graph)$label, vertex.size = V(graph)$size, vertex.color = V(graph)$color, edge.arrow.size = 0.5)
```

![](Assignment_2_files/figure-markdown_github/unnamed-chunk-7-1.png)

``` r
#less cluttered
# Set node colors based on degree centrality
degree_colors <- colorRampPalette(c("red", "yellow", "green"))(length(V(graph)$degree_centrality))
V(graph)$color <- degree_colors[rank(V(graph)$degree_centrality)]

# Set node sizes based on closeness centrality
V(graph)$size <- closeness_centrality * 500

# Set node labels to name and centrality scores
V(graph)$label <- sprintf("%s\nBet: %.2f", V(graph)$name, V(graph)$betweenness_centrality)

# Set layout and adjust parameters to make all labels visible
par(mar = c(0, 0, 0, 0))
plot(graph, vertex.label = V(graph)$label, vertex.size = V(graph)$size, vertex.color = V(graph)$color, edge.arrow.size = 0.5)

legend("topleft", legend = c("Low Degree Centrality", "Medium Degree Centrality", "High Degree Centrality", "size increases with higher closeness centrality"), fill = c("red", "yellow","green", "white") , border = NA)
```

![](Assignment_2_files/figure-markdown_github/unnamed-chunk-8-1.png)
