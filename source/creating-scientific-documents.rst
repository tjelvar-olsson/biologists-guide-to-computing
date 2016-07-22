Creating scientific documents
=============================


Introduction
------------

Communication forms an important part of the scientific process.
In :doc:`collaborating-on-projects` we added a ``README.md`` file
describing the purpose of the project and how to perform the
analysis. This is a good thing. However it may not be clear how
this process can be extended to produce scientific documents
with figures, equations and reference management. In this chapter
we will learn how to do this.


Starting the manuscript
-----------------------

Here we will build on the work from
:doc:`data-analysis`, :doc:`data-visualisation` and
:doc:`collaborating-on-projects` chapters. Let's start by
going into the ``S.coelicolor-local-GC-content`` directory.

.. code-block:: none

    $ cd S.coelicolor-local-GC-content

Now use your favorite text editor to add the text below to
a file named ``manuscipt.md``.

.. code-block:: none

    ## Introduction

    *Streptomyces coelicolor* is a bacteria that can produce a range of natural
    products of pharmaceutical relevance.

    In 2002 the genome of *S. coelicolor*  A3(2), the model actinomycete
    organism, was sequenced.

    In this study we investigate the local GC content of this organism.


    ## Methodology

    The genome of *S. coelicolor*  A3(2) was downloaded from the Sanger Centre
    ftp site.

    The local GC content was then calculated using a sliding window of 100 KB
    and a 50 KB step size.


    ## Results

    The mean of the local GC content is 72.1% with a standard deviation of 1.1. 


    ## Conclusion

    There is little variation in the local GC content of *S. coelicolor* A3(2).


The manuscript above is unlikely to make it into Nature, but it will do for
us to illustrate how to build up a scientific manuscript using plain text
files.

In terms of the markdown syntax the two hashes ``##`` are used to mark level 2
headings and the words surrounded by asterisk symbols (``*``) will be
emphasized using italic font.


Converting the document to HTML using pandoc
--------------------------------------------

Markdown is a lightweight syntax that can easily be converted to HTML.
Here we will use of the tool `Pandoc <http://pandoc.org/>`_ to convert the
markdown document to HTML.

First of all you will need to install Pandoc. Generic information on how to
install software can be found in :doc:`managing-your-system`. On a Mac
you can use Homebrew and on Linux based systems it should be available
for install using the distributions package manager. For more detail have
a look at the `Pandoc installation notes <http://pandoc.org/installing.html>`_. 

Now that we have installed Pandoc, let's use it to convert our ``manuscript.md``
file to a standalone HTML file.

.. code-block:: none

    $ pandoc -f markdown -t html -s manuscript.md > manuscript.html

In the above the ``-f markdown`` option means *from markdown* and the ``-t
html`` option means *to html*. The ``-s`` option means *standalone*, i.e.
encapsulate the content with appropriate appropriate headers and footers.
Pandoc writes to the standard output stream so we redirect it (``>``) to a file
named ``manuscript.html``. Have a look at the ``manuscript.html`` file
using a web browser.

Adding a figure
---------------

Crafting scientific manuscripts using Pandoc
--------------------------------------------

- Basic structure
- Including figures
- Reference management
- Title, author and abstract
- Pandoc
- Citeproc


Benefits of using Git and plain text files
------------------------------------------

- version control
- painless merging of changes
- transparency and audit trial
- automation
