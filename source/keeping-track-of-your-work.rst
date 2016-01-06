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

Because this is a problem that software engineers have been faced with
for a long time there are now some excellent tools for dealing with it. The
solution is to use a version control system. Here we will use Git, one of the
most popular version control systems, to keep track of our work. In its most
basic form you can think of Git as a tool for providing you with an infinite
undo-button.


First time configuration of Git
-------------------------------

Although we won't go into it in this chapter, Git is fundamentally a collaboration
tool that helps people work on projects together. This means that we need to give
Git some information about us for it to be able to keep track of who has done what,
specifically our name and email address.

.. code-block:: none

    git config --global user.name "Tjelvar Olsson"
    git config --global user.email "tjelvar.olsson@example.com"

We will look at the collaboration aspect of git in the
:doc:`collaborating-on-manuscripts` chapter.


Initialise the project
----------------------

The first thing to do is to initialise the project using the ``git init`` command.
If run with no argument it will setup tracking of files in the current working
directory. If given an argument, such as ``protein-number-vs-size``, git will
create a new directory with this name and setup tracking of files within it.

.. code-block:: none

    git init protein-number-vs-size
    cd protein-number-vs-size/

Use your editor of choice and create the file ``README.md`` and add the content
below to it.

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


Files in a Git repository, the project directory, can be in one of four states:
untracked, unmodified, modified and staged. To view the state one can use the
command ``git status``.

.. code-block:: none

    $ git status
    On branch master

    Initial commit

    Untracked files:
      (use "git add <file>..." to include in what will be committed)

            README.md

    nothing added to commit but untracked files present (use "git add" to track)

This tells us that the ``README.md`` file is untracked. However, we would like to
track it, so we add it to the Git repository using the ``git add`` command.

.. code-block:: none

    $ git add README.md
    $ git status
    On branch master

    Initial commit

    Changes to be committed:
      (use "git rm --cached <file>..." to unstage)

            new file:   README.md

This stages the ``README.md`` file to be committed. To commit the current
snapshot of the project to the Git repository we use the ``git commit``
command.

.. code-block:: none

    $ git commit -m "Added readme file."
    [master (root-commit) e1dc880] Added readme file.
     1 file changed, 12 insertions(+)
     create mode 100644 README.md
    $ git status
    On branch master
    nothing to commit, working directory clean

.. note:: The ``-m`` option allows us to specify a commit message on
          the command line. If you do not use this option your default editor
          will be used, which is likely to be ``vim`` if you have not
          configured it to be something else.

That's all you need to know to get stared with Git. Start by initialising a
project using ``git init``. Then use ``git add`` and ``git commit`` iteratively
to stage and commit snapshots of your project to version control.


Create a script for downloading the SwissProt FASTA file
--------------------------------------------------------

We will now convert the command we used to download the SwissProt FASTA file
from :doc:`first-steps-towards-automation` into a script. To add some
organisation we will put this script in a directory named ``scripts``. We will
also create a directory named ``data`` for storing the downloaded file. By
specifying more than one argument to the ``mkdir`` command one can create
multiple directories.

.. code-block:: none

    mkdir scripts data

Using your favorite text editor enter the text below into the file
``scripts/get_data.bash``.

.. code-block:: none

    #!/bin/bash

    curl --location --output data/uniprot_sprot.fasta.gz http://bit.ly/1l6SAKb

The only difference between this script and the command we entered on the command
line is the first line ``#!/bin/bash``. This is a special construct, called the
shebang, and is used to specify the shell to use when executing the content of the
file.

However, in order to be able to execute the file, i.e. run it as a program, it
needs to have execute permissions. One can view the current set of permissions
of a file by using ``ls -l``.

.. code-block:: none

    $ ls -l scripts/get_data.bash
    -rw-r--r--  1 olssont  1340193827  88 29 Nov 10:45 scripts/get_data.bash

Note the first ten characters, the first specifies the file type and the
remaining nine relate to the permissions of the file.  There are three modes
that can  be turned on or off: read (``r``), write (``w``) and execute (``x``).
Furthermore, these can be specified for the owner (``u``), group (``g``) and
all users (``a`` or ``o``). The nine characters above state that the owner has
read and write permissions on the file ``rw-`` and whereas both the group and
all other users only have permission to read the file ``r--``.

Let us give the file execute permissions. This is achieved using the ``chmod``
command, mnemonic "change file modes".

.. code-block:: none

    $ chmod a+x scripts/get_data.bash
    $ ls -l scripts/get_data.bash
    -rwxr-xr-x  1 olssont  1340193827  88 29 Nov 10:45 scripts/get_data.bash

Let us test the script by running it.

.. code-block:: none

    $ ./scripts/get_data.bash
    $ ls data/
    uniprot_sprot.fasta.gz


The file was downloaded to the ``data`` directory, success!
This is a good time to add the script to version control.

.. code-block:: none

    $ git add scripts/get_data.bash
    $ git commit -m "Added script for downloading SwissProt FASTA file."
    [master f80731e] Added script for downloading SwissProt FASTA file.
     1 file changed, 3 insertions(+)
     create mode 100755 scripts/get_data.bash

Let us check the status of our project.

.. code-block:: none

    $ git status
    On branch master
    Untracked files:
      (use "git add <file>..." to include in what will be committed)

            data/

    nothing added to commit but untracked files present (use "git add" to track)

Git is telling us that there are files in the ``data`` directory that are
currently not being tracked. However, in this project the data directory will
contain files downloaded from a canonical resource and as the download script
is in version control we do not need or want to track the files in this
directory.

It is possible to tell Git to ignore files.  Using your text editor of choice
create the file ``.gitignore`` and add the content below to it.

.. sidebar:: Hidden files

    On Unix-like systems dot-files, files starting with a ".", are treated as
    hidden files. These files are usually used to store configuration settings.
    The ``~/.bashrc`` file, for example, is used to configure your Bash shell
    environment. To list hidden files use ``ls -a``.

.. code-block:: none

    data/*

In Bash the ``*`` symbol represents a wild card pattern that can match any
string.  The ``*`` symbol can be used in the same fashion in the ``.gitignore``
file. As such the line we added to our ``.gitignore`` file tells Git to ignore
all files in the ``data`` directory.

.. code-block:: none

    $ git status
    On branch master
    Untracked files:
      (use "git add <file>..." to include in what will be committed)

            .gitignore

    nothing added to commit but untracked files present (use "git add" to track)

Git now ignores the content of the ``data`` directory and tells us that the
``.gitignore`` file is untracked. Let us add this file.

.. code-block:: none

    $ git add .gitignore
    $ git commit -m "Added gitignore file."
    $ git status
    On branch master
    nothing to commit, working directory clean


Improve script for downloading SwissProt FASTA file
---------------------------------------------------

However, the current setup has got an issue in terms of reproducibility.
Depending on when the SwissProt FASTA file was downloaded one may obtain
different results. It would therefore be useful to include the date of
access in the file name. This can be achieved using the ``date`` command,
which can be configured to create custom output formats using the ``+``
argument.

.. code-block:: none

    $ date
    Thu 26 Nov 2015 09:20:32 GMT
    $ date +'%Y-%m-%d'
    2015-11-26

To get the output of the ``date`` command into the file name one
can use Bash's concept of command substitution. To see this in action
we can use the ``echo`` command, which simply echoes the input string.

.. code-block:: none

    $ echo "Today it is $(date +'%d')th"
    Today it is 26th

For this little script we will also introduce the concept of variables.
A variable is basically a means of storing a piece of information using
a descriptive name. In bash one can assign a variable using the ``=``
character and the value of the variable can then be accessed by prefixing
the variable name with a ``$`` character.

.. sidebar:: Don't Repeat Yourself

    The use of variables is a key concept in programming. It allows programmers to
    avoid having to repeat themselves. This is important as repetition increases
    the chances of introducing errors. Suppose, for example that you had a scaling
    factor of 1.35611 that you used at ten different places in your script. That
    presents ten opportunities for typing in the wrong number. Further, suppose
    that you, later on, needed to change the scaling factor. That presents ten
    opportunities for forgetting to update a value and another ten opportunities
    for mistyping the value. In this case it would have been better to create a
    variable named ``scaling_factor`` and use that variable in the ten places in
    your script. That way they are guaranteed to be the same value and you only
    need to edit one line if you need to change the value. In programming avoiding
    repetition is important enough to warrant it's own acronym :term:`DRY` (Don't
    Repeat Yourself).

.. code-block:: none

    $ PRIBNOV_BOX="TATAAT"
    $ echo $PRIBNOV_BOX
    TATAAT

We now have all the information we need to improve the script. Edit the
``script/get_data.bash`` file to look like the below.

.. code-block:: none

    #!/bin/bash

    FNAME="data/uniprot_sprot.$(date +'%Y-%m-%d').fasta.gz"
    curl --location --output $FNAME http://bit.ly/1l6SAKb

Now we can test that the script is working as expected.

.. code-block:: none

    $ ./scripts/get_data.bash
    $ ls data/
    uniprot_sprot.2015-11-26.fasta.gz uniprot_sprot.fasta.gz


We have added a piece of functionality and have tested that it works as expected.
This is a good time to commit our changes to Git. However, before we do that
let us examine the changes to the project since the last commit
using the ``git diff`` command.

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

The command above tells us that one line has been removed, the one prefixed by a
minus sign, and that two lines have been added, the ones prefixed by a plus sign.
In fact we have modified one line and added one, but the effect is the same.

Let us now add and commit the changes to Git.

.. code-block:: none

    $ git add scripts/get_data.bash
    $ git commit -m "Updated download script to include date in file name."
    [master 7512894] Updated download script to include date in file name.
     1 file changed, 2 insertions(+), 1 deletion(-)
    
By adding the date of download to the file name reproducibility is improved and
it means that we can download the file on different dates and ensure that no data
is overwritten.

However, it is still possible to accidentally delete or modify the data file.
To overcome this, and further improve reproducibility, it is good practise to
give the data file read-only permissions. This means that the file cannot be
modified or deleted, only read. To do this we will make use of the ``chmod``
command. In this instance we will make use of an absolute mode.  Absolute modes
encode the permissions using the numbers 1, 2 and 4 that represent execute, write
and read modes respectively. These numbers can be combined to create any
permission, for example 7 represents read, write and execute permissions and 5
represents read and execute permissions.

=====  ======================
Value  Permission
=====  ======================
1      execute
2      write
3      write & execute
4      read
5      read & execute
6      read & write
7      read & write & execute
=====  ======================

To set the permissions for the owner, group and all other users
one simply uses three such numbers. For example to give the owner read and
write permissions and the group and all other users read-only permissions one
would use the absolute mode 644.

In this instance we want to set the file to read-only for the owner, group and
all other users so we will use the absolute mode 444.

.. code-block:: none

    #!/bin/bash

    FNAME="data/uniprot_sprot.$(date +'%Y-%m-%d').fasta.gz"
    curl --location --output $FNAME http://bit.ly/1l6SAKb
    chmod 444 $FNAME

If you run the script now you will see that it changes the permissions of the
downloaded file.  If you run the script again, on the same day, you will
notice that the it complains that it has not got permissions to write to
the file. This is expected as the ``curl`` command is wanting to overwrite
the existing read-only file.

This is a good time to add and commit the changes to Git.

.. code-block:: none

    $ git add scripts/get_data.bash
    $ git commit -m "Added command to set permissions of data file to read only."
    [master a672257] Added command to set permissions of data file to read only.
     1 file changed, 1 insertion(+)


Create script for counting the number of proteins in a genome
-------------------------------------------------------------

Now that we have a script for downloading the SwissProt FASTA file let us
convert what we learnt in :doc:`first-steps-towards-automation` into a script
for counting the number of proteins for a particular species.

Add the lines below to the file ``scripts/protein_count.bash``.

.. code-block:: none

    #!/bin/bash

    gunzip -c data/uniprot_sprot.fasta.gz | grep 'OS=Homo sapiens' \
    | cut -d '|' -f 2 | wc -l

Make the file executable and test the script.

.. code-block:: none

    $ chmod +x scripts/protein_count.bash
    $ ./scripts/protein_count.bash
       20194

At the moment the path to the data file and the species are hard coded into the
script. It would be nice if we could turn these two parameters into command
line arguments. We can do this using the special variables ``$1`` and ``$2`` that
represent the first and second command line arguments, respectively.

.. code-block:: none

    #!/bin/bash

    DATA_FILE_PATH=$1
    SPECIES=$2
    echo "Input file: $DATA_FILE_PATH"
    echo "Species: $SPECIES"

    gunzip -c $DATA_FILE_PATH | grep "OS=$SPECIES" \
    | cut -d '|' -f 2 | wc -l

.. warning:: Bash makes a distinction between single and double quotes. To expand
             variables one needs to use double quotes. If not one will get the
             literal value of the string within the single quotes. For example,
             the command ``echo 'Species: $SPECIES'`` would print the literal
             string ``Species: $SPECIES``.


This is a good point to test if things are working as expected.

.. code-block:: none

    $ ./scripts/protein_count.bash data/uniprot_sprot.2015-11-26.fasta.gz "Homo sapiens"
    Input file: data/uniprot_sprot.2015-11-26.fasta.gz
    Species: Homo sapiens
       20194

Success! Let us add and commit the script to Git.

.. code-block:: none

    $ git add scripts/protein_count.bash
    $ git commit -m "Added script for counting the numbers of proteins."
    [master b9de9bc] Added script for counting the numbers of proteins.
     1 file changed, 9 insertions(+)
     create mode 100755 scripts/protein_count.bash


More useful git commands
------------------------

We've covered a lot of ground in this chapter. Can you remember everything that
we did and the motivation behind each individual step? If not, that is okay,
we can use Git to remind us using the ``git log`` command.

.. code-block:: none

    $ git log --oneline
    b9de9bc Added script for counting the numbers of proteins.
    a672257 Added command to set permissions of data file to read only.
    7512894 Updated download script to include date in file name.
    6c6f65b Added gitignore file.
    f80731e Added script for downloading SwissProt FASTA file.
    e1dc880 Added readme file.

Note that the comments above give a decent description of what was done. However,
it would have been useful to include more information about the motive behind some
changes. If one does not make use of the ``-m`` argument when using ``git commit``
one can use the default text editor to write a more comprehensive commit message.
For example, a more informative commit message for commit ``a672257`` could have
looked something along the lines of:

.. code-block:: none

    Added command to set permissions of data file to read only.

    The intention of this change is to prevent accidental deletion or
    modification of the raw data file.


Another useful feature of Git is that it allows us to inspect the changes
between individual and series of commits using the ``git diff`` command.  For
example to understand what changed in commit ``a672257`` we can compare it to
the previous commit ``7512894``.

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
- When programming it is particularly useful to be able to save working
  states the code
- This gives one the opportunity to roll-back to a previously working state if
  things go wrong
- Git is a powerful version control system
- To get started with Git one only needs to get familiar with a handful of
  commands
- The overhead of using Git whilst programming is minimal
- The benefits of using Git are great
- Start using Git in your day-to-day work right now
- In Unix-like systems files have write, read and execute permissions that can
  be turned on and off
- By making a file executable it can be run as an independent program
- By giving raw data files read only permissions one can ensure that they are
  not accidentally modified or deleted
