# Loving Markdown!

Ok, so for those who don't know, the guys from RStudio recently teamed up with
Yihui to add some really nice report authoring options in RStudio using the 
packages [knitr][kLink] ability to turn a combination of [markdown][mLink] and
[R][rLink] code into html. 

I have to admit, this has really changed how I work. Previously, I generally had
R scripts, that I would then run, and summarize the results in a separate document
as a report on what I had done. I know, many like to talk about [Sweave][sLink], 
the language that R uses to generate vignettes demonstrating package functionality,
but have you ever tried to write a Sweave document?

You need to know a fair amount about Latex, and even then it can be difficult to
get the output you want. In addition, reading the raw file can be quite painful 
(I know, I have my own [Bioconductor package][bLink] that I wrote a Sweave
vignette for).

Writing R Markdown documents just feels different. When I read the raw source
of a Markdown document, I can actually read it, code and all. What is really
sweet is that instead of writing about what I am doing in the comments, I write
it out in full in the document, and then have the code blocks doing the actual
calculations. What is really great is to regenerate the report, I simply re-knit
it to generate a new html file.

It is so much easier to work with, that I am probably going to switch even how
I write my blog posts, using a Markdown document as the source. For right now,
that means writing a .md file, and then converting it to html using the R Markdown
package, and then writing in the html to Blogger. You can see a good explanation
of that process from Jeffrey Horner's blog [here][jhLink1] and [here][jhLink2].

When I combine this with a [github repo][ghLink] for storage, it also means I have 
some other place to keep the raw source of my blog posts, as well as easily read
and edit the text.

Source of this post is at https://github.com/rmflight/blogPosts/rmarkdown_post_150812.md.
Published at http://robertmflight.blogspot.com


[kLink]: http://yihui.name/knitr
[mLink]: http://daringfireball.net/projects/markdown/
[rLink]: http://www.r-project.org
[sLink]: http://www.statistik.lmu.de/~leisch/Sweave/
[bLink]: https://github.com/rmflight/categoryCompare
[jhLink1]: http://jeffreyhorner.tumblr.com/post/25804518110/blog-with-r-markdown-and-tumblr-part-i
[jhLink2]: http://jeffreyhorner.tumblr.com/post/25943954723/blog-with-r-markdown-and-tumblr-part-ii
[ghLink]: https://github.com/rmflight/blogPosts