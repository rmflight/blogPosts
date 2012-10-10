I've been using the `snowfall` package for a while to enable parallel processing in `R` on my Windows machine, where I 
have an 8 core processor. I discovered today that the function `sfLapply` will only work with with an object that has a 
class of `list`. This is really important, because there are many things that are "list-like", and are actually lists at 
heart, but `sfLapply` probably won't like it.

Lets whip up an example. 


```r
require(snowfall)
require(GenomicRanges)

gr <- GRanges(seqnames = Rle(c("chr1", "chr2", "chr1", "chr3"), c(1, 3, 2, 4)), 
    ranges = IRanges(1:10, width = 10:1, names = head(letters, 10)), strand = Rle(strand(c("-", 
        "+", "*", "+", "-")), c(1, 2, 2, 3, 2)), score = 1:10, GC = seq(1, 0, 
        length = 10))

gr
```

```
## GRanges with 10 ranges and 2 elementMetadata cols:
##     seqnames    ranges strand |     score                GC
##        <Rle> <IRanges>  <Rle> | <integer>         <numeric>
##   a     chr1  [ 1, 10]      - |         1                 1
##   b     chr2  [ 2, 10]      + |         2 0.888888888888889
##   c     chr2  [ 3, 10]      + |         3 0.777777777777778
##   d     chr2  [ 4, 10]      * |         4 0.666666666666667
##   e     chr1  [ 5, 10]      * |         5 0.555555555555556
##   f     chr1  [ 6, 10]      + |         6 0.444444444444444
##   g     chr3  [ 7, 10]      + |         7 0.333333333333333
##   h     chr3  [ 8, 10]      + |         8 0.222222222222222
##   i     chr3  [ 9, 10]      - |         9 0.111111111111111
##   j     chr3  [10, 10]      - |        10                 0
##   ---
##   seqlengths:
##    chr1 chr2 chr3
##      NA   NA   NA
```

```r

class(gr)
```

```
## [1] "GRanges"
## attr(,"package")
## [1] "GenomicRanges"
```

```r

grList <- split(gr, seqnames(gr))
grList
```

```
## GRangesList of length 3:
## $chr1 
## GRanges with 3 ranges and 2 elementMetadata cols:
##     seqnames    ranges strand |     score                GC
##        <Rle> <IRanges>  <Rle> | <integer>         <numeric>
##   a     chr1   [1, 10]      - |         1                 1
##   e     chr1   [5, 10]      * |         5 0.555555555555556
##   f     chr1   [6, 10]      + |         6 0.444444444444444
## 
## $chr2 
## GRanges with 3 ranges and 2 elementMetadata cols:
##     seqnames  ranges strand | score                GC
##   b     chr2 [2, 10]      + |     2 0.888888888888889
##   c     chr2 [3, 10]      + |     3 0.777777777777778
##   d     chr2 [4, 10]      * |     4 0.666666666666667
## 
## $chr3 
## GRanges with 4 ranges and 2 elementMetadata cols:
##     seqnames   ranges strand | score                GC
##   g     chr3 [ 7, 10]      + |     7 0.333333333333333
##   h     chr3 [ 8, 10]      + |     8 0.222222222222222
##   i     chr3 [ 9, 10]      - |     9 0.111111111111111
##   j     chr3 [10, 10]      - |    10                 0
## 
## ---
## seqlengths:
##  chr1 chr2 chr3
##    NA   NA   NA
```

```r
class(grList)
```

```
## [1] "GRangesList"
## attr(,"package")
## [1] "GenomicRanges"
```


So we have the `GRanges` object `gr`, and a `GRangesList` in `grList`. Now lets try to do some parallel execution of
finding overlaps of itself.

This is the function we will use in parallel:


```r
returnOverlaps <- function(inObj1, inObj2) {
    findOverlaps(inObj1, inObj2, type = "any")
}
```



```r
sfInit(parallel = T, cpus = 2)
```

```
## Warning: Unknown option on commandline: options(encoding
```

```
## R Version:  R version 2.15.0 (2012-03-30)
```

```
## snowfall 1.84 initialized (using snow 0.3-10): parallel execution on 2
## CPUs.
```

```r
sfLibrary(GenomicRanges)
```

```
## Library GenomicRanges loaded.
```

```
## Library GenomicRanges loaded in cluster.
```

```
## Warning: 'keep.source' is deprecated and will be ignored
```

```r

overlap <- sfLapply(grList, returnOverlaps, gr)
```

```
## Error: 2 nodes produced errors; first error: no method for coercing this
## S4 class to a vector
```

```r

sfStop()
```

```
## Stopping cluster
```


Ok, we get the error `no method for coercing this S4 class to a vector`. Seems kind of cryptic, at least it did to me.
What about using normal `lapply`?


```r
overlap <- lapply(grList, returnOverlaps, gr)

overlap
```

```
## $chr1
## Hits of length 7
## queryLength: 3
## subjectLength: 10
##   queryHits subjectHits 
##    <integer>   <integer> 
##  1         1           1 
##  2         1           5 
##  3         2           1 
##  4         2           5 
##  5         2           6 
##  6         3           5 
##  7         3           6 
## 
## $chr2
## Hits of length 9
## queryLength: 3
## subjectLength: 10
##   queryHits subjectHits 
##    <integer>   <integer> 
##  1         1           2 
##  2         1           3 
##  3         1           4 
##  4         2           2 
##  5         2           3 
##  6         2           4 
##  7         3           2 
##  8         3           3 
##  9         3           4 
## 
## $chr3
## Hits of length 8
## queryLength: 4
## subjectLength: 10
##   queryHits subjectHits 
##    <integer>   <integer> 
##  1         1           7 
##  2         1           8 
##  3         2           7 
##  4         2           8 
##  5         3           9 
##  6         3          10 
##  7         4           9 
##  8         4          10
```


This works without any errors. Odd. It was only when I was trying to get this to work using `llply` from the `plyr` 
package that I saw a message about `as.default.list` or something like that. So maybe we have to convert the `grList` to 
a good and proper `list` first?


```r
sfInit(parallel = T, cpus = 2)
```

```
## Warning: Unknown option on commandline: options(encoding
```

```
## snowfall 1.84 initialized (using snow 0.3-10): parallel execution on 2
## CPUs.
```

```r
sfLibrary(GenomicRanges)
```

```
## Library GenomicRanges loaded.
```

```
## Library GenomicRanges loaded in cluster.
```

```
## Warning: 'keep.source' is deprecated and will be ignored
```

```r

grList <- as.list(grList)
overlap2 <- sfLapply(grList, returnOverlaps, gr)

sfStop()
```

```
## Stopping cluster
```

```r

overlap2
```

```
## $chr1
## Hits of length 7
## queryLength: 3
## subjectLength: 10
##   queryHits subjectHits 
##    <integer>   <integer> 
##  1         1           1 
##  2         1           5 
##  3         2           1 
##  4         2           5 
##  5         2           6 
##  6         3           5 
##  7         3           6 
## 
## $chr2
## Hits of length 9
## queryLength: 3
## subjectLength: 10
##   queryHits subjectHits 
##    <integer>   <integer> 
##  1         1           2 
##  2         1           3 
##  3         1           4 
##  4         2           2 
##  5         2           3 
##  6         2           4 
##  7         3           2 
##  8         3           3 
##  9         3           4 
## 
## $chr3
## Hits of length 8
## queryLength: 4
## subjectLength: 10
##   queryHits subjectHits 
##    <integer>   <integer> 
##  1         1           7 
##  2         1           8 
##  3         2           7 
##  4         2           8 
##  5         3           9 
##  6         3          10 
##  7         4           9 
##  8         4          10
```


It works! I'm not exactly sure why there is this difference, but I thought perhaps I can save someone else a few hours
time figuring it out.

SessionInfo:


```r
sessionInfo()
```

R version 2.15.0 (2012-03-30)
Platform: x86_64-pc-mingw32/x64 (64-bit)

locale:
[1] LC_COLLATE=English_United States.1252 
[2] LC_CTYPE=English_United States.1252   
[3] LC_MONETARY=English_United States.1252
[4] LC_NUMERIC=C                          
[5] LC_TIME=English_United States.1252    

attached base packages:
[1] stats     graphics  grDevices utils     datasets  methods   base     

other attached packages:
[1] GenomicRanges_1.8.13 IRanges_1.14.4       BiocGenerics_0.2.0  
[4] snowfall_1.84        snow_0.3-10          knitr_0.8.1         

loaded via a namespace (and not attached):
[1] digest_0.5.2   evaluate_0.4.2 formatR_0.6    plyr_1.7.1    
[5] stats4_2.15.0  stringr_0.6.1  tools_2.15.0  


Posted on [Blogger](http://rflight.blogspot.com/2012/10/sflapply-vs-lapply.html). [Rmd](https://github.com/rmflight/blogPosts/blob/master/sflapplyVlapply.Rmd), [md](https://github.com/rmflight/blogPosts/blob/master/sflapplyVlapply.md)
