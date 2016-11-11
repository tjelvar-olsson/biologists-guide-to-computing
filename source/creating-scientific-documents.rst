Creating scientific documents
=============================


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


The manuscript above won't make it into Nature, but it will do for
us to illustrate how to build up a manuscript using plain text files.

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

    ![Alternative text](path/to/image.png)

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

In the above the double asterix (``**``) is used as markup for bold text.
This will serve as a title for the figure caption.


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
    title={{S. coelicolor genome}},
    url={ftp://ftp.sanger.ac.uk/pub/project/pathogens/S_coelicolor/whole_genome/},
    urldate={2016-07-10}
    }

This is a so called BibTex record. In this particular case it is a BibTex record for
an online resource, as indicated by the ``@online`` type. You would also use the
``@online`` type to reference web pages.

The text ``S.coelicolor-genome`` is the "key" assigned to this record. The key could
have been called anything as long as it is unique. This key will be used within our
document when citing this record.

Now append the text below to the bottom of the ``references.bib`` file.

.. code-block:: none

    @article{Bentley2002,
    title={Complete genome sequence of the model actinomycete
           Streptomyces coelicolor A3 (2)},
    author={Bentley, Stephen D and Chater, Keith F and Cerdeno-Tarraga, A-M and
            Challis, Greg L and Thomson, NR and James, Keith D and
            Harris, David E and Quail, Michael A and Kieser, H and
            Harper, David and others},
    journal={Nature},
    volume={417},
    number={6885},
    pages={141--147},
    year={2002},
    publisher={Nature Publishing Group}
    }


Do not type in BibTex records by hand. The entire ``Bentley2002``
record was copied and pasted from `Google Scholar <https://scholar.google.co.uk/>`_.
Note that in the record above the identifier was changed from ``bentley2002complete``
(key used by Google Scholar) to ``Bentley2002``.

References managers such as `Mendeley <https://www.mendeley.com>`_ and `Zotero
<https://www.zotero.org/>`_ can also be used to export BibTex records. More
suggestions on how to access BitTex records can be found on the
`Tex StackExchange <http://tex.stackexchange.com/a/207>`_ site.

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

The ``--filter pandoc-citeproc`` argument results in automatically adding citations
and a bibliography to the document. However, this requires some knowledge
of where the bibliographic information is, this is specified using the
``--bibliography=references.bib`` argument.

"CiteProc" is in fact a generic name for a program that can be used to
produce citations and bibliographies based on formatting rules using
the Citation Style Langauge (CSL) syntax. Zotero
provides CSL styles for lots of journals in
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


Adding meta data
----------------

To turn this into a research article we need to add a title, authors, an
abstract and a date. In Pandoc this can be achieved by adding meta data to the
top of the file, using a YAML syntax (see :doc:`structuring-and-storing-data`
for information on YAML).

Add the header below to the top of the  ``manuscript.md`` file.

.. code-block:: none

    ---
    title: "*S. coelicolor* local GC content analysis"
    author: Tjelvar S. G. Olsson and My Friend
    abstract: |
      In 2002 the genome of *S. coelicolor*  A3(2), the model actinomycete
      organism, was sequenced.

      The local GC content was calculated using a sliding window of
      100 KB and a 50 KB step size.

      The mean of the local GC content was found to be 72.1% with a standard
      deviation of 1.1. We therefore conclude that there is little variation
      in the local GC content of *S. coelicolor* A3(2).
    date: 25 July 2016
    ---
    ## Introduction


Let's give some explanation of the meta data above.
The YAML meta data is encapsulated using ``---``. The title string is
quoted to avoid the ``*`` symbols confusing Pandoc. The pipe
symbol at the beginning of the abstract allows for multi-line input with
newlines, note that the multi-lines must be indented.

Let's generate the document again.

.. code-block:: none

    $ pandoc -f markdown -t latex -s manuscript.md -o manuscript.pdf   \
      --filter pandoc-citeproc --bibliography=references.bib  \
      --csl=nature.csl

The ``manuscript.pdf`` document is now looking pretty good!

Anther useful feature of Pandoc's meta data section is that we can add
information for some of the data that we previously had to specify on
the command line. Let's add items for the ``--bibliograpy`` and
``--csl`` options (these options are in fact short hand for
``--metadata bibliograpy=FILE`` and ``--metadata csl=FILE``).

.. code-block:: none
    :emphasize-lines: 2,3

    date: 25 July 2016
    bibliography: references.bib
    csl: nature.csl
    ---
    ## Introduction

Now we can generate the documentation using the command below.

.. code-block:: none

    $ pandoc -f markdown -t latex -s manuscript.md -o manuscript.pdf   \
      --filter pandoc-citeproc

This is a good point to commit a snapshot to version control. Let's
look at the status of our repository first.

.. code-block:: none

    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Untracked files:
      (use "git add <file>..." to include in what will be committed)

            bioinformatics.csl
            manuscript.html
            manuscript.md
            manuscript.pdf
            nature.csl
            references.bib

    nothing added to commit but untracked files present (use "git add" to track)

We have created many new files. We want to track all of them except
``manuscript.pdf`` and ``manuscript.html`` as they can be generated by Pandoc.
Let us therefore update the ``.gitignore`` file to look like the below.

.. code-block:: none
    :emphasize-lines: 1,2

    manuscript.*
    !manuscript.md

    Sco.dna
    local_gc_content.csv
    local_gc_content.png

In the above the first two lines are new.
Let's explain what they do. The first
line states that all files starting with ``manuscript.`` should be ignored.
This includes the file we want to track ``manuscript.md``. On the second line
we therefore add an exception for this file, the exclamation mark (``!``) is
used to indicate that the ``manuscript.md`` should be excluded from the
previous rule to ignore it.

.. code-block:: none

    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

            modified:   .gitignore

    Untracked files:
      (use "git add <file>..." to include in what will be committed)

            bioinformatics.csl
            manuscript.md
            nature.csl
            references.bib

    no changes added to commit (use "git add" and/or "git commit -a")

Now we can add the remaining files and commit the snapshot.

.. code-block:: none

    $ git add bioinformatics.csl manuscript.md nature.csl references.bib
    $ git commit -m "Added draft manuscript"
    [master 7b06d9d] Added draft manuscript
     4 files changed, 332 insertions(+)
     create mode 100644 bioinformatics.csl
     create mode 100644 manuscript.md
     create mode 100644 nature.csl
     create mode 100644 references.bib

Finally, let us also add and commit the updated ``.gitignore`` file.

.. code-block:: none

    $ git add .gitignore
    $ git commit -m "Updated gitignore to ignore generated manuscript files"
    [master bea89f4] Updated gitignore to ignore generated manuscript files
     1 file changed, 3 insertions(+)


Benefits of using Pandoc and plain text files
---------------------------------------------

Why go through all this trouble to produce a PDF document?
Would it not be easier to simply write it using a word processor
and export it as PDF?

There are three main advantages to the methodology outlined
in this chapter.

1. There are lots of awesome tools for working with plain text files.
   If you decide to create your manuscript using plain text files
   you can take advantage of them. Worthy of special mention is Git
   which is one of the most powerful collaboration tools in the world,
   with the added advantage of giving you unlimited undo functionality
   and a transparent audit trial.

2. Automation! We will go into this more in the next chapter,
   :doc:`automation-is-your-friend`. However, for now imagine that
   someone discovered that there was something wrong with the
   raw data that you had been using.  How long would it take you to
   update your manuscript? Using plain text files it is possible
   to create automated work flows to build entire documents from
   raw data.

3. Ability to convert to any file format. We have already seen how you
   can covert the document to HTML. What if your collaborator really
   needs Word? No problem.

   .. code-block:: none

        $ pandoc -f markdown -t docx -s manuscript.md -o manuscript.docx  \
          --filter pandoc-citeproc

Incidentally, another advantage of learning to use Pandoc is that it is not
limited in going from markdown to other formats. It can take almost any
file format and convert it to any other file format. For example, one could
convert the Word document we just created to TeX.

.. code-block:: none

    $ pandoc -f docx -t latex manuscript.docx -o manuscript.tex


Key concepts
------------

- Pandoc is a powerful tool that can be used to convert (almost) any document
  format to any other
- Markdown can be used to add document structure to plain text files
- The ``pandoc-citeproc`` filter can be used to add citations and a bibliography
- The Citation Style Language (CSL) can be used to format the citations and the bibliography
- You can access BibTex records from Google Scholar
- Mendeley and Zotero are a free reference managers that can be used to export BibTex records
  (try to avoid having to type out BibTex records by hand)
- Zotero provides CSL citation style files for lots and lots of journals
- It is possible to add YAML meta data to markdown files specifying attributes such
  as title, author and abstract
- Using plain text files for scientific documents allows you to make use of
  awesome tools such as Git
