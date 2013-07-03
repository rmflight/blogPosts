# REST Interfaces in NAR Database Issue

The NAR database issue is a nice one-stop shop to find out what web-services are available for bioinformatics.

As I've been reading through some of the resources in this years issue, I was struck by how many of the tools do not seem to have a programmatic interface (either REST or SOAP). Although I realize that often times the goal is to build a tool that can be used by anyone regardless of programming ability, and to allow exploration. However, there is also a large movement to enable reproducible research using scripts and various utilities. 

A common problem with web-based tools is that they do not easily enable provenance tracking. One could argue that in many cases the command-line version of the tool could be downloaded and run instead, but that would sort of be undoing the point of providing a web-interface. Another option is to provide meta-data that records what settings (input, options, etc) were used to generate the results. Finally, one could provide a REST interface that allows one to interact with the tool using URL generators, and command line tools such as `curl`. This allows one to use scripts to generate results from a web-based tool. 

This would also allow one to **play** with the tool in the web-interface, and then generate the results that will be used in any reported results using a scripted version that records the parameters as part of the call to the interface.

## How many REST interfaces

Based on the above, I wanted to attempt to determine how many of the articles describe a web-interface that also has a REST interface. Note, I am not keeping the text, and only querying the **abstract** and **article** sections of the articles. 

[Database Issue](http://nar.oxfordjournals.org/content/41/D1.toc)

[Web Server Issue](http://nar.oxfordjournals.org/content/41/W1.toc)
