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
    organism, was sequenced.  In this study we investigate the local GC content
    of this organism.


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

Alternatively, we could have used the ``-o`` option to specify the name
of an output file. The command below produces the same outcome as the
previous command.

.. code-block:: none

    $ pandoc -f markdown -t html -s manuscript.md -o manuscript.html

Adding a figure
---------------

At this point it would be good to add the figure produced in
:doc:`data-visualisation` to the "Results" section of the manuscript.

In markdown images can be added using the syntax below.

.. code-block:: none

    [Alternative text](path/to/image.png)

In HTML the intention of the alternative text (the "alt" attribute) is to
provide a descriptive text in case the image cannot be displayed for some
reason. Pandoc makes use of the alternative text attribute to create
a caption for the image.


.. code-block:: none
    :emphasize-lines: 5-7

    ## Results

    The mean of the local GC content is 72.1% with a standard deviation of 1.1.

    ![**Variation in the local GC content of *S. coelicolor* A3(2).** Using a
    window size of 100 KB and a step size of 50 KB the local GC content has a
    mean of 72.1% and a standard deviation of 1.1.](local_gc_content.png)


Converting the document to PDF
------------------------------

HTML is great for websites. However, scientific documents tend to be read as
PDF. Let us use Pandoc to convert our document to PDF.

However, before we can do this we need to install
`LaTeX <https://www.latex-project.org>`_. On Mac install
`MacTeX <http://www.tug.org/mactex>`_. On Linux use you package manager
to install LaTeX, possibly known as "TeX Live". See the
section on `Obtaining LaTeX <https://latex-project.org/ftp.html>`_ on
the LaTeX project website for more information.

Now that you have installed LaTeX you can convert the ``manuscript.md``
markdown file to PDF using the command below.
     
.. code-block:: none

    $ pandoc -f markdown -t latex -s manuscript.md -o manuscript.pdf

In the above we use the ``-t latex`` option to specify that the
``manuscript.pdf`` output file should be built using LaTeX.

Reference management
--------------------

Reference management is a particularly prominent feature of scientific
writing. Let us therefore look at how we can include references to
websites and papers in our document.

Let's start by creating a bibliography file. Copy and paste the 
content below into a file named ``references.bib``.

.. code-block:: none

    @online{S.coelicolor-genome,
    title = {{S. coelicolor genome}},
    url = {ftp://ftp.sanger.ac.uk/pub/project/pathogens/S_coelicolor/whole_genome/},
    urldate = {2016-07-10}
    }
    @article{Bentley2002,
    abstract = {Streptomyces coelicolor is a representative of the group of soil-dwelling, filamentous bacteria responsible for producing most natural antibiotics used in human and veterinary medicine. Here we report the 8,667,507 base pair linear chromosome of this organism, containing the largest number of genes so far discovered in a bacterium. The 7,825 predicted genes include more than 20 clusters coding for known or predicted secondary metabolites. The genome contains an unprecedented proportion of regulatory genes, predominantly those likely to be involved in responses to external stimuli and stresses, and many duplicated gene sets that may represent 'tissue-specific' isoforms operating in different phases of colonial development, a unique situation for a bacterium. An ancient synteny was revealed between the central 'core' of the chromosome and the whole chromosome of pathogens Mycobacterium tuberculosis and Corynebacterium diphtheriae. The genome sequence will greatly increase our understanding of microbial life in the soil as well as aiding the generation of new drug candidates by genetic engineering.},
    author = {Bentley, S D and Chater, K F and Cerde{\~{n}}o-T{\'{a}}rraga, A-M and Challis, G L and Thomson, N R and James, K D and Harris, D E and Quail, M A and Kieser, H and Harper, D and Bateman, A and Brown, S and Chandra, G and Chen, C W and Collins, M and Cronin, A and Fraser, A and Goble, A and Hidalgo, J and Hornsby, T and Howarth, S and Huang, C-H and Kieser, T and Larke, L and Murphy, L and Oliver, K and O'Neil, S and Rabbinowitsch, E and Rajandream, M-A and Rutherford, K and Rutter, S and Seeger, K and Saunders, D and Sharp, S and Squares, R and Squares, S and Taylor, K and Warren, T and Wietzorrek, A and Woodward, J and Barrell, B G and Parkhill, J and Hopwood, D A},
    doi = {10.1038/417141a},
    issn = {0028-0836},
    journal = {Nature},
    keywords = {Bacterial Proteins,Chromosomes, Bacterial,Corynebacterium diphtheriae,Genes, Bacterial,Genes, Duplicate,Genome, Bacterial,Genomics,Molecular Sequence Data,Multigene Family,Mycobacterium tuberculosis,Protein Isoforms,Streptomyces,Synteny},
    month = {may},
    number = {6885},
    pages = {141--7},
    pmid = {12000953},
    title = {{Complete genome sequence of the model actinomycete Streptomyces coelicolor A3(2).}},
    url = {http://www.ncbi.nlm.nih.gov/pubmed/12000953},
    volume = {417},
    year = {2002}
    }


Do not type in bibliography records by hand. The entire ``Bentley2002``
record was exported from my `Mendeley <https://www.mendeley.com>`_
desktop client.

Now let's add some references to our ``manuscript.md`` file.

.. code-block:: none
    :emphasize-lines: 7, 15

    ## Introduction

    *Streptomyces coelicolor* is a bacteria that can produce a range of natural
    products of pharmaceutical relevance.

    In 2002 the genome of *S. coelicolor*  A3(2), the model actinomycete
    organism, was sequenced [@Bentley2002].

    In this study we investigate the local GC content of this organism.


    ## Methodology

    The genome of *S. coelicolor*  A3(2) was downloaded from the Sanger Centre
    ftp site [@S.coelicolor-genome].


Now we can add referenes using Pandoc's built in ``pandoc-citeproc``
filter.

.. code-block:: none

    $ pandoc -f markdown -t latex -s manuscript.md -o manuscript.pdf   \
      --filter pandoc-citeproc --bibliography=references.bib

The ``--filter pandoc-citeproc`` filter automatically generates citations
and a bibliography to the document. However, this requires some knowledge
of where the bibliographic information is, this is specified using the
``--bibliography=references.bib`` argument.

"CiteProc" is in fact a generic name for a program that can be used to
produce citations and bibliographies based on formatting rules using
the Citation Style Langauge (CSL) syntax. `Zotero <https://www.zotero.org/>`_
(another free reference manager) provides CSL styles for lots of journals in
the `Zotero Style Repository <https://www.zotero.org/styles>`_.

Let's download Zotero's CSL file for Nature, copy and paste this text into
a file named ``nature.csl``.

.. code-block:: none

    $ curl https://www.zotero.org/styles/nature > nature.csl

We can now produce our document using Nature's citation style.

.. code-block:: none

    $ pandoc -f markdown -t latex -s manuscript.md -o manuscript.pdf   \
      --filter pandoc-citeproc --bibliography=references.bib  \
      --csl=nature.csl

Have a look at the generated PDF file. Pretty neat right?! One thing that
is missing is a title for the reference section. Let's add that to the
``manuscript.md`` file.

.. code-block:: none
    :emphasize-lines: 6

    ## Conclusion

    There is little variation in the local GC content of *S. coelicolor* A3(2).


    ## References


Title, authors and abstract
---------------------------


Benefits of using Git and plain text files
------------------------------------------

- version control
- painless merging of changes
- transparency and audit trial
- automation