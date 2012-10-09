# AbsIDconvert: New method for converting genomic identifiers

Today our paper ["AbsIDconvert: An absolute approach for converting genetic identifiers at different granularities"](http://dx.doi.org/10.1186/1471-2105-13-229)
finally hit BMC Bioinformatics. I'm really excited, because I've been wanting to tell people outside our primary
collaborators about it. The website for the tool is http://bioinformatics.louisville.edu/abid. There will eventually be
a downloadable virtual machine for local analyses, with no restrictions on the number of items submitted.

The basic premise is that every genetic type of identifier, whether it is an Entrez Gene, Refseq, Ensembl (gene,
transcript), and microarray probe (or probeset for Affy) can be reduced to a DNA sequence that can subsequently be 
placed on a reference genome as a genomic interval. Conversion between different types of identifiers then becomes
a problem of finding overlapping genomic intervals. 

We have a large number of different types of identifiers for different organisms and genome assemblies stored as 
genomic intervals, including many Affy and Agilent microarrays. However, if your favorite array is not present, or your
identifier doesn't seem to work (as of Sept 12, 2012 there is at least one Agilent array that seems to be missing IDs),
you can submit the sequences and find corresponding genomic intervals and translate to other identifiers.

Note that there is a limit on how many sequences / IDs can be uploaded at one time (you will get a message to "Select
Genome Version!!!!!!" when you try to upload too many sequences for example). This is removed in the virtual machine 
version.

The code behind the website uses [`R`](http://r-project.org), [`RApache`](http://rapache.net/) and the [`GenomicRanges`](http://www.bioconductor.org/packages/2.10/bioc/html/GenomicRanges.html) package for storing and querying intervals. Alignment is carried out using [`Bowtie2`](http://bowtie-bio.sourceforge.net/index.shtml)

I hope others find this resource (and or approach) useful!

Next week I hope to put up a post with more examples, although you can probably get a good idea of how it works and
the possibilities from the publication and the website.

Source hosted at https://github.com/rmflight/blogPosts/blob/master/absidConvert_live.md

Posted to http://robertmflight.blogspot.com/2012/09/absidconvert-new-method-for-converting_12.html