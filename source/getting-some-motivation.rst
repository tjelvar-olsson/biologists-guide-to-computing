Getting some motivation
=======================

Why you should care about computing and coding
----------------------------------------------

Computers can be powerful allies. They are ideal for automating repetitive tasks.
Furthermore, they can perform calculations and analysis that are too complex
for the human brain to process.

These days many parts of the biological sciences are becoming more and more data
driven. Technological advancements have led to a huge increase in the
generation of biological data. Data analysis is required to extract biological
insights from this data. To a large extent the rate limiting factor in
generating insight is the lack of appropriate data analysis tools.

However, as a biologist you may prefer to work in the lab over
sitting in front of the computer. You may even ask yourself if learning to
program has any benefit to you whatsoever.

Similar to learning a new language there are several benefits to learning
computer programming. Importantly it gives you a new perspective. In fact
professional programmers are encouraged to learn a new programming language
every year to allow them to experience new problem solving techniques.

More concretely, learning to program will help you get a better understanding
of data analysis. It may even expand your mind to allow you to see new
possibilities available to you.

Furthermore, by getting more familiar with computing your interactions with
programmers and data analysis folk will improve. So even if you discover that
you don't like programming learning to code will help you express your ideas to
people that do.


What you can expect to learn here
---------------------------------

Many books on computing assume that the reader has a basic knowledge of how a
computer operates and the associated jargon. This booklet attempts not to make any
such assumptions and important concepts are explained in
:doc:`how-to-think-like-a-computer`. Furthermore, important terms and their
background are explained throughout in attempt to demystify computing jargon.
This should stand you in good stead when faced with more advanced material on
computing.

Use of the command line is required for many bioinformatics tools and access to
high-performance computing clusters. The essence of the Unix command line, used
in both Linux and Macs, is introduced in :doc:`first-steps-towards-automation`.
After which the command line is used persistently to interact with a variety of
useful programs. At the end you should feel comfortable using the command line
to interact with your computer and to automate tasks.

Data forms a central part of the biological sciences. However, dealing with
large volumes of data can feel overwhelming. One way to regain some control is
to ensure that there is structure to the data. In
:doc:`structuring-and-storing-data` important data structures and file formats
for representing them are discussed. The chapter also highlights the value of
using plain text files to store data. After reading this chapter you should have
a good idea of how to store structured representations of your data.

Sooner or later one wishes that one had done a better
job of keeping records of one's work. What changes were made when? What
were the reasons behind the decisions to make those changes? These issues
becomes even more accentuated when working on scripts and programs where
tiny changes can have devastating effects. Because this is such an important
issue in software development there are great tools for mitigating against it.
In :doc:`keeping-track-of-your-work` the concept of version control is
introduced using the popular open source tool Git. After working through
the examples in this chapter you should feel comfortable using Git to keep
track of your work.

Being able to create data analysis scripts is empowering. In
:doc:`data-analysis` the Python scripting language is introduced and we
create a script for calculating the local GC content of the
*Streptomyces coelicolor*  A3(2) genome. After working through this
chapter you will have a basic knowledge of how to use Python to read
in data from a file, manipulate it and write out results to another file.
The chapter also introduces more general programming concepts such as
variables, functions and loops.

Data visualisation is an important part of scientific investigation.  In
:doc:`data-visualisation` the statistical scripting language R is used to look
at how one can explore data interactively from the command line and create
scripts that make the generation of figures reproducible. After working through
this chapter you will have a gained familiarity in working with R and an
understanding of how to use R's ggplot2 package to create beautiful and
informative figures.

The best science often involves collaboration. In
:doc:`collaborating-on-projects` you will learn how to use make use of
the powerful collaboration features of Git and you will learn how to
back up and share your project using the free hosting services on GitHub.

Communication is an important aspect of science and one aspect of scientific
communication is writing manuscripts. In
:doc:`creating-scientific-documents` you will learn how lightweight text
documents can be converted to beautiful scientific manuscripts with
citations and bibliographies using Pandoc. You will also learn how you
can use Pandoc to convert (almost) any document file format into any
other.

After having spent weeks and months analysing data, generating figures and
assembling all the information into a manuscript it can be devastating to find
that one needs to start all over again because an updated data set has become
available.  However, this needn't be the case.  If all your analysis, figure
generation and manuscript assembly was automated you would just need to replace
the raw data and press "Go".  In :doc:`automation-is-your-friend` you will
learn how to achieve this state of bliss.

When tackling more complex data analysis one needs to spend more time thinking
about the problem up front. In the :doc:`practical-problem-solving` chapter we
will look at techniques for breaking problems into smaller and more manageable
chunks. The chapter will also extend your familiarity with Python, and
introduces the concepts of string manipulation and regular expressions.

At some point you may need to work on a remote computer.  For many this may be
when one needs access to the institute's high performance computing cluster. In
:doc:`working-remotely` you will learn how to use the command line to log in to
a remote computer and how to copy data to and from the remote machine.

Installing software is not particularly exciting. However, it is a means
to an end. In :doc:`managing-your-system` we go over various methods of
installing software. The chapter also introduces some fundamental
Unix-based systems administration concepts required to understand what
is needed to install software successfully.

Finally the book ends with :doc:`next-steps`, a short chapter giving
some suggestions on how to continue building your knowledge of
scientific computing.
