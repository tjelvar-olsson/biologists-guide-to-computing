First steps towards automation
==============================

One of the first steps towards automating repetitive tasks is to become
familiar with the command line. In this chapter we will do this by using
the command line to extract the UniProt identifiers for all human proteins
Swiss-Prot.

You will get the most benefit from this chapter if you work through the example
as well as you go along. You are therefore encouraged to open up a terminal now!

![Terminal]()

On Macs the command line is available through the Terminal application. There
are a number of terminal emulators available for Linux. If you are using the
Gnome-based desktop the default is likely to be the Gnome Terminal.

A terminal emulator is an application that gives you access to another program
known as the shell. The shell allows you to interact with the operating systems
services and programs. Just like there is a diversity of terminal emulators
there are also a number of different shells. The most widely used shell and the
default on Mac and most Linux based distributions is ``bash``.


Creating a new directory for our project
----------------------------------------

First of all let us make sure that we are working in our home directory.

::

    cd

The ``cd`` command, short for change directory, is used to move between
directories. If called without a path to a directory of interest it will
move into your home directory.

We can print out the name of the current working directory using the ``pwd``
command. Furthermore we can list the contents of a directory using the ``ls``
command.

::

    pwd
    ls

Now that we know where we are and what files and directories are present let us
create a new directory for our project. This is achieved using the ``mkdir``
command, short for "make directory". After having created the directory make it
your working directory by moving into it using the ``cd`` command.

::

    mkdir first_step_towards_automation
    cd first_step_towards_automation

.. note:: When using the command line one learns to avoid using white spaces in
          file and directory names. This is because white spaces are used to separate
          arguments. In the example above we used underscores instead of white spaces.
          However, one could just as well have used hyphens. This comes down to personal
          preference. It is possible to represent file names with spaces in them on the
          command line by using the escape ``\`` character, for example
          ``first\ steps\ towards\ automation``. However, this is discouraged as it soon
          becomes very tedious.


Downloading the Swiss-Prot knowledge base
-----------------------------------------

It is time to download the Swiss-Prot knowledge base from the UniProt. We can
use the ``curl`` program to do this.  The ``curl`` command is a C program that
allows us to stream data from URLs and FTP sites.  By default the ``curl``
program prints the content of the URL to the standard output stream. To see
this in action try running the command::

    curl www.bbc.com

However, because we are going to be downloading a larger file we would like to
write it to disk for future use. Many command line programs allow the user to
specify additional options. In this particular case we can use the
``--remote-name`` option to specify that the output should be written to a file
named as the remote file. Let us now download the gzipped FASTA file from the
UniProt FTP site::

    curl --remote-name ftp://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/complete/uniprot_sprot.fasta.gz


The downloaded file ``uniprot_sprot.fasta.gz`` has been compressed using the ``gzip`` protocol. 
We can extract it using the ``gunzip`` command.  However, when extracted it
more than doubles in size. So rather than extract the file to disk we will
simply extract it on the fly when we need it.

Try running the command::

    gunzip --to-stdout uniprot_sprot.fasta.gz

You should see a lot of FASTA lines printed to your terminal, or more formally speaking
the standard output stream.


Creating a work flow using pipes
--------------------------------

Now it is time to introduce one of the greatest features of the command line: piping!
Explain what a pipe is...

To illustrate the use of pipes we will redirect the output to the word count program
``wc``. Try running the command below::

    gunzip --to-stdout uniprot_sprot.fasta.gz | wc

It should give you three numbers, these are the line, word and character counts. To
only see the line count one could use the ``-l`` option::

    gunzip --to-stdout uniprot_sprot.fasta.gz | wc -l

You may ask yourself why this is such a great feature.
Explanation...


Examining files, without modifying them
---------------------------------------


Finding FASTA identifiers corresponding to human proteins
---------------------------------------------------------


Extracting the UniProt identifiers
----------------------------------


Using pipes to create an output file
------------------------------------


Removing files and directories
------------------------------


More useful commands
--------------------

- history
- clear
- reset
- which
- whereis
- whoami
- find
- locate


Getting help
------------

-h, --help, man, info


Summary of useful commands
--------------------------


Key concepts
------------
