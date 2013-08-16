# Reproducible Methods

Science is built on the whole idea of being able to reproduce results, i.e. if I publish something, it should be possible for someone else to reproduce it, using the description of the methods used in the publication. As biological sciences have become increasingly reliant on computational methods, this has become a bigger and bigger issue, especially as the results of experiments become dependent on independently developed computational code, or use rather sophisticated computer packages that have a variety of settings that can affect output, and multiple versions. For further discussion on this issue, you might want to read [1](http://bytesizebio.net/index.php/2012/08/24/can-we-make-research-software-accountable/), [2](http://ivory.idyll.org/blog/anecdotal-science.html).

I recently read a couple of different publications that really made me realize how big a problem this is. I want to spend some time showing what the problem is in these publications, and why we should be concerned about the current state of computational analytical reproducibility in life-sciences.

In both the articles mentioned below, I do not believe that I, or anyone not associated with the project, would be able to generate even approximately similar results based solely on the raw data and the description of methods provided. Ultimately, this is a failure of both those doing the analysis, and the reviewers who reviewed the work, and is a rather deplorable situation for a field that prides itself verification of results. This is why I'm saying these are **bad bioinformatics methods sections**.

## Puthanveettil et al., Synaptic Transcriptome

[Puthanveettil et al, 2013](http://doi.org/10.1073/pnas.1304422110) had a paper out earlier titled ["A strategy to capture and characterize the synaptic transcriptome"](http://doi.org/10.1073/pnas.1304422110) in PNAS. Although the primary development reported is a new method of characterizing RNA complexes that are carried by kinesin, much of the following analysis is bioinformatic in nature.

For example, they used BLAST searches to identify the RNA molecules, a cutoff value is reported in the results. However, functional characterization using Gene Ontology (GO) was carried out by "Bioinformatics analyses" (see the top of pg3 in the PDF). No mention of where the GO terms came from, which annotation source was used, or any software mentioned. Not in the results, discussion, or methods, or the supplemental methods. The microarray data analysis isn't too badly described, but the 454 sequencing data processing isn't really described at all.

My point is, that even given their raw data, I'm not sure I would be able to even approximate their results based on the methods reported in the methods section.

## Gulsuner et al., Schizophrenia SNPs

[Gulsuner et al](http://dx.doi.org/10.1016/j.cell.2013.06.049) published a paper in Cell in August 2013 titled ["Spatial and Temporal Mapping of De Novo Mutations in Schizophrenia to a Fetal Prefrontal Cortical Network"](http://dx.doi.org/10.1016/j.cell.2013.06.049). This one also looks really nice, they look for *de novo* mutations (i.e. new mutations in offspring not present in parents or siblings) that mess up genes that are in a heavily connected network, and also examine gene co-expression over brain development time-scales. Sounds really cool, and the results seem like they are legit, based on my reading of the manuscript. I was really impressed that they even used **randomly generated** networks to control the false discovery rate!

However, almost all of the analysis again depends on a lot of different bioinformatic software. I do have to give the authors props, they actually give the **full** version of each tool used. But no mention of tool specific settings (which can generate vastly different results, see **Exome Sequencing** of the methods).

Then there is this bombshell: "The predicted functional impact of each candidate de novo missense variant was assessed with in silico tools." (near top of pg 525 of the PDF). Rrrreeeaaaalllly now. No actual quote of which tools were used, although the subsequent wording and references provided imply that they were [PolyPhen2](http://www.nature.com/nmeth/journal/v7/n4/full/nmeth0410-248.html), [SIFT](http://sift.jcvi.org/), and the [Grantham Method](http://www.sciencemag.org/content/185/4154/862.long). But shouldn't that have been stated up front? Along with any settings that were changed from default??

There is no raw data available, only their reported SNPs. Not even a list of **all** the SNPs that were potentially considered, so that I could at least go from those and re-run the later analysis. I have to take their word for it (although I am glad at least the SNPs they used in later analyses are reported). 

Finally, the random network generation. I'd like to be able to see that code, go over it, and see what exactly it was doing to verify it was done correctly. It likely was, based on the histograms provided, but still, these are where small errors creep in and result in invalid results.

As above, even if the raw data was available (didn't see an SRA accession or any other download link), I'm not sure I could reproduce or verify the results.

## What to do??

How do we fix this problem? I think scripts and workflows used to run any type of bioinformatic analyses have to become first class research objects. And we have to teach scientists to write them and use them in a way that makes them first class research objects. So in the same way that a biologist might ask for verification of immunostaining, etc, bioinformaticians should ask that given known input, a script generates *reasonable* output. 

I know there has been discussion on this before, and disagreement, especially with the exploratory nature of research. However, once you've got something working *right*, you should be able to *test* it. Reviewers should be asking if it is testable, or the code should be available for others to test.

I also think we as a community should do more to point out the problem. i.e. when we see it, point it out to others. I've done that here, but I don't know how much should be formal. Maybe we need a new hashtag, #badbioinfomethodsection, and point links to papers that do this. Conversely, we should also point to examples when it is done right (#goodbioinfomethodsection??), and if you are bioinformatician or biologist who does a lot of coding, share your code, and at least supply it as supplemental materials. Oh, and maybe take a [SoftwareCarpentry](http://softwarecarpentry.org) class, and look up [git](http://git-scm.com/).


Posted on August 16, 2013 at http://robertmflight.blogspot.com/2013/08/reproducible-methods-or-bad.html, raw markdown at https://github.com/rmflight/blogPosts/blob/master/reproducible_methods.md
