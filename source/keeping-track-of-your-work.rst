Keeping track of your work
==========================

Introduction
------------

In this and the following couple of chapters we will investigate whether or not
the average size of a protein increases with the number of proteins in a
genome. Intuitively one might think that as organisms become more complex they
need a larger variety of proteins and that these proteins themselves become
larger. However, an organism's "complexity" is a rather subjective concept. We
will therefore ignore that term and simply try to determine if there is a
correlation between the number of proteins in a genome and the average size of
those proteins across all species characterised in SwissProt.

We will do this by developing scripts to analyse the data in the SwissProt
FASTA file. When developing scripts it is often beneficial to build them up one
step at a time, adding functionality as one goes along. However, as scripts
become more complex one often accidentally break them whilst trying to add
more functionality. To make things worse it can often be difficult to remember
what changes were introduced since the last working state of the code.

Because this is a problem that people developing software have been faced with
for a long time there are now some excellent tools for dealing with it. The
solution is to use a version control system. Here we will use Git, one of the
most popular version control systems, to keep track of our work.


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



Improve script for downloading SwissProt FASTA file
---------------------------------------------------

Date the file

.. code-block:: none

    $ date
    Thu 26 Nov 2015 09:20:32 GMT
    $ date +'%Y-%m-%d'
    2015-11-26

.. code-block:: none

    #!/bin/bash

    FNAME="data/uniprot_sprot.$(date +'%Y-%m-%d').fasta.gz"
    curl --location --output $FNAME http://bit.ly/1l6SAKb

Test.

.. code-block:: none

    $ ./scripts/get_data.bash
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   192  100   192    0     0    108      0  0:00:01  0:00:01 --:--:--   108
    100 79.3M  100 79.3M    0     0   150k      0  0:09:00  0:09:00 --:--:--   99k
    $ ls data/
    uniprot_sprot.2015-11-26.fasta.gz uniprot_sprot.fasta.gz


Check what we have changed.

.. code-block:: none

    $ git diff
    diff --git a/scripts/get_data.bash b/scripts/get_data.bash
    index d8e9bda..338d82c 100755
    --- a/scripts/get_data.bash
    +++ b/scripts/get_data.bash
    @@ -1,3 +1,4 @@
     #!/bin/bash

    -curl --location --output data/uniprot_sprot.fasta.gz http://bit.ly/1l6SAKb
    +FNAME="data/uniprot_sprot.$(date +'%Y-%m-%d').fasta.gz"
    +curl --location --output $FNAME http://bit.ly/1l6SAKb

Commit to git.

.. code-block:: none

    $ git add scripts/get_data.bash
    $ git commit -m "Updated download script to include date in file name."
    [master 7512894] Updated download script to include date in file name.
     1 file changed, 2 insertions(+), 1 deletion(-)
    
Make it read only

.. code-block:: none

    #!/bin/bash

    FNAME="data/uniprot_sprot.$(date +'%Y-%m-%d').fasta.gz"
    curl --location --output $FNAME http://bit.ly/1l6SAKb
    chmod 444 $FNAME

Commit it to git.

.. code-block:: none

    $ git add scripts/get_data.bash
    [-- olssont@ exit=0 ~/sandbox/protein-number-vs-size --]
    $ git commit -m "Added command to set permissions of data file to read only."
    [master a672257] Added command to set permissions of data file to read only.
     1 file changed, 1 insertion(+)

- Illustrate ``git diff``


File permissions recap
----------------------


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


At the moment the path to the data file and the species are hard coded into the
script. It would be nice if we could turn these two parameters into command
line arguments.

.. code-block:: none

    #!/bin/bash

    DATA_FILE_PATH=$1
    SPECIES=$2
    echo "Input file: $DATA_FILE_PATH"
    echo "Species: $SPECIES"

    gunzip -c $DATA_FILE_PATH | grep "OS=$SPECIES" \
    | cut -d '|' -f 2 | uniq | wc -l

.. note:: We needed to use double quotes to access the value stored in the
          ``$SPECIES`` variable, if we were to use single quotes the ``grep``
          command would search for the literal string ``$SPECIES``.

Let us test the script again.

.. code-block:: none

    $ ./scripts/protein_count.bash data/uniprot_sprot.2015-11-26.fasta.gz "Homo sapiens"
    Input file: data/uniprot_sprot.2015-11-26.fasta.gz
    Species: Homo sapiens
       20194

Let us save the script to version control.

.. code-block:: none

    $ git add scripts/protein_count.bash
    $ git commit -m "Added script for counting the numbers of proteins."
    [master b9de9bc] Added script for counting the numbers of proteins.
     1 file changed, 9 insertions(+)
     create mode 100755 scripts/protein_count.bash


More useful git commands
------------------------

.. code-block:: none

    $ git log --oneline
    b9de9bc Added script for counting the numbers of proteins.
    a672257 Added command to set permissions of data file to read only.
    7512894 Updated download script to include date in file name.
    6c6f65b Added gitignore file.
    f80731e Added script for downloading SwissProt FASTA file.
    e1dc880 Added readme file.

.. code-block:: none

    $ git diff 7512894 a672257
    diff --git a/scripts/get_data.bash b/scripts/get_data.bash
    index 338d82c..0bbc17b 100755
    --- a/scripts/get_data.bash
    +++ b/scripts/get_data.bash
    @@ -2,3 +2,4 @@

     FNAME="data/uniprot_sprot.$(date +'%Y-%m-%d').fasta.gz"
     curl --location --output $FNAME http://bit.ly/1l6SAKb
    +chmod 444 $FNAME


Key concepts
------------

- When working with files it is often desirable to be able to track changes
- Whilst programming it is particularly useful to be able to save working
  states the code
- This gives one the opportunity to roll-back to a previously working state if
  things go wrong
- Git is a powerful version control system
- To get started with Git one only needs to get familiar with a handful of
  commands
- The overhead of using Git whilst programming is minimal
- The benefits of using Git are great
- Start using Git in your day-to-day work right now
- Files files have write, read and execute permissions that can be turned on
  and off
- By making a file executable it can be run as an independent program
- By giving raw data files read only permissions one can ensure that they are
  not accidentally modified or deleted
