Automation is your friend
=========================

Have you ever felt your heart sink upon reading the reviewer comments along the
lines of: *"very nice analysis, but it would be useful if it was performed on
species x, y and z as well"*? Or perhaps you have spent weeks analysing data
and generating figures only to find out that a newer and more complete data
set has become available?

In these circumstances would it not be nice if all you had to do was point
your project at a new URL from where the latest version of the data could be
downloaded and all the analyses, figures and the manuscript were magically
updated for you?

This is possible! In this chapter we will make use of a tool called ``make``
to automate all the steps from :doc:`data-analysis`, :doc:`data-visualisation`
and :doc:`creating-scientific-documents`.


Introduction to ``make``
------------------------

The ``make`` tool is commonly used to build software. As such ``make`` is
a tool for managing dependencies in a build system, where dependencies
are organised into a tree (as in a graph). People in the software industry
often talk about dependency graphs.

The ``make`` program makes use of a file named ``Makefile`` in
the current directory. Makefiles contain rules. Each rule consists of
a target (the stuff to be created), prerequisites (the stuff needed to
build the target) and a recipe (the instructions for how to create the
target from the prerequisites).

Creating rules in a file named ``Makefile`` is therefore a means to
create an automated build process.


Creating a ``Makefile``
-----------------------

First of all make sure that you are in the ``S.coelicolor-local-GC-content``
directory created in :doc:`data-analysis`.

.. code-block:: none

    $ cd S.coelicolor-local-GC-content

One of the first steps in :doc:`data-analysis` was to download the
*S. coelicolor* genome. Let's start by creating a rule for this step.
Using your favorite text editor create a file named ``Makefile`` and
add the content below to it.

.. code-block:: makefile

	Sco.dna:
		curl --location --output Sco.dna http://bit.ly/1Q8eKWT

.. warning:: The ``make`` program expects the lines to be indented using
             the Tab character so make sure that the ``curl`` command is
             preceded by a Tab character and not a bunch of white spaces.

In this first rule ``Sco.dna`` is the target. The target has no prerequisites
and the recipe has one instruction that involves downloading the file using
``curl``.

Now we can run the ``make`` command.

.. code-block:: none

    $ make
    make: `Sco.dna' is up to date.

Let's add another rule for cleaning up the working directory.

.. code-block:: makefile
	:emphasize-lines: 4,5

	Sco.dna:
		curl --location --output Sco.dna http://bit.ly/1Q8eKWT

	clean:
		rm Sco.dna

When invoking ``make`` it is possible to specify which rule to run.
Let's clean up.

.. code-block:: none

    $ make clean
    rm Sco.dna

If we run make now it will notice that the ``Sco.dna`` file does not
exist and will download it.

.. code-block:: none

    $ make
    curl --location --output Sco.dna http://bit.ly/1Q8eKWT

Now we need a rule to create the ``local_gc_content.csv`` (the target)
from the ``Sco.dna`` file (the prerequisite). Add the lines below to
the top of the ``Makefile``.

.. code-block:: makefile

	local_gc_content.csv: Sco.dna
		python dna2csv.py

Now update the ``clean`` rule.

.. code-block:: makefile
	:emphasize-lines: 3

	clean:
		rm Sco.dna
		rm local_gc_content.csv

Let's clean up again.

.. code-block:: none

    $ make clean
    rm Sco.dna
    rm local_gc_content.csv

Now we have removed ``Sco.dna`` and ``local_gc_content.csv``.
This is a good opportunity to show that ``make`` resolves dependencies.
We can do this by calling the ``local_gc_content.csv`` rule, this will
in turn call the ``Sco.dna`` rule.

.. code-block:: none

    $ make local_gc_content.csv
    curl --location --output Sco.dna http://bit.ly/1Q8eKWT
    python dna2csv.py

That's cool, ``make`` uses the information about requirements to
build any missing pieces.

Let's add another rule for generating the ``local_gc_content.png`` file
from the ``local_gc_content.csv`` file. Add the lines below to the top
of the ``Makefile``.

.. code-block:: makefile

	local_gc_content.png: local_gc_content.csv
		Rscript csv2png.R

Let's also remember to update the rule for cleaning up.

.. code-block:: makefile
	:emphasize-lines: 4

	clean:
		rm Sco.dna
		rm local_gc_content.csv
		rm local_gc_content.png

Finally, let's add a rule for building the PDF file and update
the ``clean`` rule to remove the ``manuscript.pdf`` file.

.. code-block:: makefile
	:emphasize-lines: 1-3, 18

	manuscript.pdf: local_gc_content.png
		pandoc -f markdown -t latex -s manuscript.md -o manuscript.pdf   \
		--filter pandoc-citeproc

	local_gc_content.png: local_gc_content.csv
		Rscript csv2png.R

	local_gc_content.csv: Sco.dna
		python dna2csv.py

	Sco.dna:
		curl --location --output Sco.dna http://bit.ly/1Q8eKWT

	clean:
		rm Sco.dna
		rm local_gc_content.csv
		rm local_gc_content.png
		rm manuscript.pdf

Let's try it.

.. code-block:: none

    $ make clean
    rm Sco.dna
    rm local_gc_content.csv
    rm local_gc_content.png
    rm manuscript.pdf

Double check that the files have actually been removed.

.. code-block:: none

    $ ls
    Makefile           csv2png.R          nature.csl
    README.md          dna2csv.py         references.bib
    bioinformatics.csl manuscript.md

Now let's build the ``manuscript.pdf`` file from scratch.

.. code-block:: none

    $ make
    curl --location --output Sco.dna http://bit.ly/1Q8eKWT
    python dna2csv.py
    Rscript csv2png.R
    pandoc -f markdown -t latex -s manuscript.md -o manuscript.pdf   \
            --filter pandoc-citeproc

Very cool!

Now suppose that for some reason we needed to use a different genome
file. In this case we would only need to update the URL used in the
Makefile's ``Sco.dna`` rule and run ``make`` again! Amazing!

This is a good point to commit a snapshot of the Git repository.
First of all let's clean up.

.. code-block:: none

    $ make clean
    rm Sco.dna
    rm local_gc_content.csv
    rm local_gc_content.png
    rm manuscript.pdf

Now let's check the status of the project.

.. code-block:: none

    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Untracked files:
      (use "git add <file>..." to include in what will be committed)

            Makefile

    nothing added to commit but untracked files present (use "git add" to track)

Great let's add and commit the ``Makefile``.

.. code-block:: none

    $ git add Makefile
    $ git commit -m "Added Makefile to build manuscript.pdf"
    [master 5d74b6a] Added Makefile to build manuscript.pdf
     1 file changed, 18 insertions(+)
     create mode 100644 Makefile

Finally, we push the changes to GitHub.

.. code-block:: none

    $ git push
    Counting objects: 3, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (3/3), 498 bytes | 0 bytes/s, done.
    Total 3 (delta 1), reused 0 (delta 0)
    To https://github.com/tjelvar-olsson/S.coelicolor-local-GC-content.git
       bea89f4..5d74b6a  master -> master

It is worth noting that the mean and standard deviation of the local GC content
is currently hard coded inside the manuscript.  To make this example completely
automated one would have to write scripts to extract these and use the
extracted numbers in the manuscript. This is left as an exercise for the
reader.


Key concepts
------------

- Automation is your friend
- The ``make`` command builds projects using rules specified in the ``Makefile``
- By specifying requirements for each target in the ``Makefile`` the ``make`` command
  can work out what the dependency graph is
- It is good practice to specify a ``clean`` rule for removing files that are built
  automatically
- Automation saves you time in the long run
