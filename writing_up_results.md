# Writing up scientific results and literate programming

As an academic researcher, my primary purpose is to find some new insight, and subsequently communicate this insight to the general public. The process of doing this is traditionally thought to be:

  1. from observations of the world, generate a hypothesis
  * design experiments to test hypothesis
  * analyse results of the experiments to determine if hypothesis correct
  * write report to communicate results to others (academics and / or general public)

And then repeat.

This is the way people envision it happening. And I would imagine that in some rare cases, this is what actually happens. However, I think many researchers would agree that this is not what normally happens. In the process of doing steps **3** and **4**, your hypothesis in **1** will be modified, which modifies the experiments in **2**, and so on and so forth. This makes the process of scientific discovery a very iterative process, often times right up to the report writing. 

For some of this, it takes a long time to figure this out. I'll never forget a professor during my PhD who suggested that you write the paper, and then figure out what experiments you should do to generate the results that would support or disprove the hypothesis you made in the paper. At the time I thought he was nuts, but when you start writing stuff, and looking at how all the steps of experiment and reporting can become intertwined, it doesn't seem like a bad idea. <font size="3"><a href="note1">note1</a></font>

# Literate programming

What does this have to do with `literate programming`? For those who don't know, `literate programming` is a way to mix code and prose together in one document (in `R` we use `knitr` & `sweave`, `python` now has the `iPython` notebook as an option). This `literate programming` paradigm (combined with `markdown` as the markup language instead of `latex` thanks to `knitr`) is changing how I actually write my papers and do research in general. 

# How that changes writing

As I've previously described, `RStudio` makes the use of `knitr` and generation of literate documents using computations in `R` incredibly easy. Because my writing environment and programming environment are tightly coupled together, I can easily start writing what looks like a shareable, readable publication as soon as I start writing code. Couple this together with a CVS like `git`, and you have a way to follow the complete providence of a publication from start to finish.

Instead of commenting my code to explain **why** I am doing something, I explain what I am doing in the prose, and then write the code to carry out that analysis. This changes my writing and coding style, and makes the interplay among the steps of writing scientific reports above much more explicit. I think it is a good thing, and will hopefully make my writing and research more productive.


### Notes

  1. I am not suggesting that one only do experiments that will support the experiment, but writing out the paper at least gives you a framework for doing the experiments, and doing initial analysis. One should always be willing to modify the publication / hypothesis based on what the experiments tell you.

### Sources

Published 10.05.13 [here](http://robertmflight.blogspot.com/scientific-writing.html)

The source markdown for this document is available [here](https://github.com/rmflight/blogPosts/blob/master/writing_up_results.md)
