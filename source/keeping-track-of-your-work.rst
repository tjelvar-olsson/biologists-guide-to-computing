Keeping track of your work
==========================

Introduction
------------

- Describe the biological problem of interest
- Describe the problem of revision control
- Introduce git


Initialise the project
----------------------

.. code-block:: none

    git init protein-number-vs-size
    cd protein-number-vs-size/

Use your new found editor of choice and create the file ``README.md`` and add
the content below to it.

.. code-block:: none

    # Protein number versus size

    Mini-project to investigate whether or not the average size of a protein
    increases with the number of proteins in a genome.

    Intuitively one might think that as organisms become more complex they need a
    larger variety of proteins and that these proteins themselves become larger.

    However, the complexity of an organism is a rather subjective concept. Let us
    therefore ignore that term and simply try to determine if there is a
    correlation between the number of proteins in a genome and the average size of
    those proteins across all species.



.. code-block:: none

    git status


.. code-block:: none

    On branch master

    Initial commit

    Untracked files:
      (use "git add <file>..." to include in what will be committed)

            README.md

    nothing added to commit but untracked files present (use "git add" to track)

.. code-block:: none

    git add README.md
    git commit -m "Added readme file."

.. code-block:: none

    [master (root-commit) e1dc880] Added readme file.
     1 file changed, 12 insertions(+)
     create mode 100644 README.md


Create a script for downloading the SwissProt FASTA file
--------------------------------------------------------

.. code-block:: none

    mkdir scripts data



.. code-block:: none

    #!/bin/bash

    curl --location --output data/uniprot_sprot.fasta.gz http://bit.ly/1l6SAKb


.. code-block:: none

    $ chmod +x scripts/get_data.bash
    $ ./scripts/get_data.bash

Make sure that the script downloaded the file to the intended destination
directory.

.. code-block:: none

    $ ls data

Add the script to version control.

.. code-block:: none

    $ git add scripts/get_data.bash
    $ git commit -m "Added script for downloading SwissProt FASTA file."
    [master f80731e] Added script for downloading SwissProt FASTA file.
     1 file changed, 3 insertions(+)
     create mode 100755 scripts/get_data.bash

.. code-block:: none

    git status

.. code-block:: none

    On branch master
    Untracked files:
      (use "git add <file>..." to include in what will be committed)

            data/

    nothing added to commit but untracked files present (use "git add" to track)

Using your editor of choice create the file ``.gitignore`` and add the content
below to it.

.. code-block:: none

    data/*

Explain glob pattern...

.. code-block:: none

    $ git status

.. code-block:: none

    On branch master
    Untracked files:
      (use "git add <file>..." to include in what will be committed)

            .gitignore

    nothing added to commit but untracked files present (use "git add" to track)


.. code-block:: none

    $ git add .gitignore
    $ git commit -m "Added gitignore file."
    $ git status

.. code-block:: none

    On branch master
    nothing to commit, working directory clean


Create script for counting the number of proteins in a genome
-------------------------------------------------------------

Add the lines below to the file ``scripts/protein_count.bash``.

.. code-block:: none

    #!/bin/bash

    gunzip -c data/uniprot_sprot.fasta.gz | grep 'OS=Homo sapiens' \
    | cut -d '|' -f 2 | uniq | wc -l

Make the file executable and test the script.

.. code-block:: none

    $ chmod +x scripts/protein_count.bash
    $ ./scripts/protein_count.bash



Improve script for downloading SwissProt FASTA file
---------------------------------------------------

- Date the file
- Make it read only
- Illustrate ``git diff``


File permissions recap
----------------------


Improve script for counting the number of proteins in a genome
--------------------------------------------------------------

- Allow user to specify input file and species as a command line arguments

At the moment the species in hard coded into the script. It would be nice
if we could turn this into a command line argument.

Note that we needed to use double quotes to access the value stored in the
``$SPECIES`` variable, if we were to use single quotes the ``grep`` command
would search for the literal string ``$SPECIES``.

.. code-block:: none

    #!/bin/bash

    SPECIES=$1
    echo $SPECIES

    gunzip -c data/uniprot_sprot.fasta.gz | grep "OS=$SPECIES" \
    | cut -d '|' -f 2 | uniq | wc -l

Let us test the script again.

.. code-block:: none

    $ ./scripts/protein_count.bash "Homo sapiens"
    Homo sapiens
       20194
    $ ./scripts/protein_count.bash "Escherichia coli (strain K12)"
    Escherichia coli (strain K12)
        4433


More useful git commands
------------------------

- ``git log``
