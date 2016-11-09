Managing your system
====================


This chapter will give a brief overview of how to install software on
Unix-like systems. Different systems operate in different ways and this
can lead to confusion. This chapter aims to help you get an understanding of
the basic principles as well as an overview of the most common systems giving
you a solid foundation to build upon.


The file system
---------------

The file system on Unix-like systems is built up like a tree starting from,
the so called, root. You can view the content of your root system by typing in ``ls /``.

On a linux box this may look like the below.

.. code-block:: none

    $ ls /
    bin  dev   etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root
    run  sbin  srv  sys   tmp  usr    var

The files that you have been working with so far have been located in your home
directory, e.g. ``/home/olssont/``.

However, the programs that you have been running are also files. Programs
fundamental to the operating system is located in ``/bin``. Other programs
installed by the systems package manager tend to be located in ``/usr/bin``.

To find out the location of a program you can use the ``which`` command. For
example, let us find out the location of the ``ls`` program.

.. code-block:: none

    $ which ls
    /bin/ls

We can run the ``ls`` program using this absolute path, for example to view the content
of the root directory again.

.. code-block:: none

    $ /bin/ls /
    bin  dev   etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root
    run  sbin  srv  sys   tmp  usr    var


The ``PATH`` environment variable
---------------------------------

If running ``ls`` is equivalent to running ``/bin/ls`` it is worth asking how
the shell knows how to translate ``ls`` to ``/bin/ls``. Or more correctly
how does the shell know to look for the ``ls`` program in the ``/bin`` directory?

The answer lies in the environment variable ``PATH``. This environment variable
tells the shell the locations where to go looking for programs. We can inspect the
content of the ``PATH`` environment variable using the ``echo`` command.

.. code-block:: none

    $ echo $PATH
    /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin

.. note:: Remember that in bash scripting we need to prefix variable names with a dollar
          sign (``$``) to access the value the variable is holding.

The ``PATH`` variable basically contains a list of colon (``:``) separated directories.
When you try to run a program the shell looks in these directories, in order, and uses
the first match it can find. If the program of interest is not located in any of these
directories you will see a ``command not found`` error issued from your shell.

.. code-block:: none

    $ this-program-does-not-exist
    bash: this-program-does-not-exist: command not found


The ``root`` user
-----------------

Now let's have a look at the permissions of the ``/usr/bin`` directory.
In the below we use the ``-l`` flag to list the result in "long" format, i.e.
to view the permissions etc, and the ``-d`` flag to state that we want to list the
directory itself rather than its content.

.. code-block:: none

    $ ls -ld /usr/bin/
    drwxr-xr-x  1056 root  wheel  35904 22 Mar 11:15 /usr/bin/

In the above the directory is owned by the ``root`` user and belongs to the
``wheel`` group. The permissions on the directory states that only ``root``,
the owner, is allowed to write to the directory.

The ``root`` user is a special user that is all powerful, sometimes referred to
as a superuser or "su" for short. These special "powers" of the superuser are
often needed to perform systems administration tasks, like installing software
and creating/deleting users.

On some systems you become the superuser, to perform systems administration
tasks, by using the *switch user* (``su``) command. This defaults to switching
to the superuser.

.. code-block:: none

    $ su
    Password:
    #

Note that this prompts you for the root password. However, depending on who
provisioned your machine you may or may not have access to the root password.
Note also that when you are logged in as the superuser the prompt tends to
change to a hash symbol (``#``). This is to warn you that things that you do
can have dire consequences.

A more modern approach to running commands with root privileges is to prefix
the command of interest with ``sudo``. This allows you to run a command as
another user, the ``root`` user by default.

The details of who can run commands using ``sudo`` are stored in the
``/etc/sudoers`` file.

.. code-block:: none

    $ ls -l /etc/sudoers
    -r--r-----  1 root  wheel  1275 10 Sep  2014 /etc/sudoers

Note that you need root privileges to be able to read this file. We can
therefore illustrate the use of the ``sudo`` command by trying to read the file
using the ``less`` pager.

.. code-block:: none

    $ sudo less /etc/sudoers

The only problem with the command above is that you won't be able to run it
unless you are on the sudoer's list in the first place.

A consequence of the fact that only the ``root`` user can write files to the
``/bin`` and ``/usr/bin`` directories is that you need to have root privileges
to install software (write files) to these default locations.


Using package managers
----------------------

All modern Linux distribution come with a so called package manager, which should
be your first port of call when trying to install software. Package managers make it
easier to install software for two main reasons they resolve dependencies
and they (usually) provide pre-compiled versions of software that are known
to play nicely with the other software available through the package manager.

There are countless numbers of Linux distributions. However, most main stream
distributions are derived from either Debian or RedHat.  Debian based Linux
distributions include amongst others Debian itself, Ubuntu, and Linux Mint. RedHat
based distributions include RedHat, CentOS and Fedora.

Although Mac OSX comes with the AppStore this is not the place to look for scientific
software. Instead two other options based on the idea of the Linux package managers
have evolved the first one is `Mac Ports <https://www.macports.org/>`_ and the
second one is `Homebrew <http://brew.sh/>`_. I would recommend using the latter as
it has thriving scientific user community.


Installing software on Debian based systems
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Debian-based systems come with a huge range of pre-package software available for
installation using the Advance Package Tool (APT). To search for a piece of software
package you would typically start off by updating the list of packages available
for download using the ``apt-get update`` command.

.. code-block:: none

    $ sudo apt-get update

One can then search for the specific software of interest, for example the multiple
sequence alignment tools `T-Coffee <http://www.tcoffee.org/Projects/tcoffee/>`_, using
the ``apt-cache search`` command.

.. code-block:: none

    $ sudo apt-cache search t-coffee
    t-coffee - Multiple Sequence Alignment
    t-coffee-examples - annotated examples for the use of T-Coffee

To install the software package one uses the ``apt-get install`` command.

.. code-block:: none

    $ sudo apt-get install t-coffee


To uninstall a package one can use the ``apt-get remove`` command.

.. code-block:: none

    $ sudo apt-get remove t-coffee

The command above leaves package configuration files intact in case you would
want to re-use them in the future. To completely remove a package from the system
one would use the ``apt-get purge`` command.

.. code-block:: none

    $ sudo apt-get purge t-coffee


Installing software on RedHat based systems
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

RedHat and its free clone CentOS come, with fewer software packages than Debian. The
T-Coffee software, is for example not available. However, on the other hand
RedHat is a super solid Linux distribution created by Red Hat Inc, the first billion
dollar open source company.

RedHat based systems use the YUM package manager. To search for software one can
use the ``yum search`` command. For example one could use the command below to search
for the Git version control package.

.. code-block:: none

    $ yum search git

.. sidebar:: What doe YUM stand for?

    YUM is an acronym for "Yellowdog Updater, Modified". This name symbolises that YUM
    is a rewritten and modified version of the "Yellowdog Updater" (YUP) tool, which
    was the package manager for the Yellowdog Linux distribution.

To install a package using YUM one uses the ``yum install`` command.

.. code-block:: none

    $ sudo yum install git

To uninstall a package one can use the ``yum remove`` command.

.. code-block:: none

    $ sudo yum remove git

RedHat based system also provide groups of software. One group that you will
want to install is the "Development Tools" group. This includes the Gnu C
Compiler (gcc) and the "make" tools that are often required to install other
software from source code.

.. code-block:: none

    $ sudo yum groupinstall "Development Tools"


There are far fewer packages available for Redhat based distributions compared
to Debian based distributions.  To make available more software packages for
the former it is worth adding the Extra Packages for Enterprise Linux (EPEL)
repository. This can be achieved by running the command below.

.. code-block:: none

    $ sudo yum install epel-release

.. warning:: YUM has also got an "update" command. However, unlike APT where
             ``apt-get update`` updates the list of available software packages
             YUM's ``yum update`` will update all the installed software packages
             to the latest version.


Installing software on Mac OSX
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section will illustrate how to install software using the
`Homebrew <http://brew.sh/>`_ package manager.

First of all we need to install Homebrew itself. This can be achieved using the
command below, taken from the Homebrew home page.

.. code-block:: none

    $ /usr/bin/ruby -e "$(curl \
    -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

Homebrew refers to packages as "formulae". That is because each
package/formulae is a ruby script describing how to install/brew a particular
piece of software.

Homebrew, just like APT, contains a local list of formulae that can be
syncronised with the online sources using the ``brew update`` command.

.. code-block:: none

    $ brew update

To search for a formulae one can use the ``brew search`` command. Let us for
example search for the Git version control package.

.. code-block:: none

    $ brew search git

To install a formulae using Homebrew one uses the ``brew install`` command.

.. code-block:: none

    $ brew install git

To uninstall a formulae one uses the ``brew uninstall`` command.

.. code-block:: none

    $ brew uninstall git

One of the things that you will want to do is to add another "tap" to Homebrew.
Namely, the ``science`` tap. In Homebrew a "tap" is an additional resource of
formulae.

.. code-block:: none

    $ brew tap homebrew/science

We can now search for scientific software such as T-Coffee.

.. code-block:: none

    $ brew search t-coffee

And install it.

.. code-block:: none

    $ brew install t-coffee


Compiling software from source
------------------------------

Many scientific software packages are only available as source code. This may mean
that you need to compile the software yourself in order to run it.

There are lots of different ways of compiling software from source. In all
likelihood you will need to read and follow instructions provided with the
software sources. The instructions are typically included in ``README`` or
``INSTALL`` text files.

The most common scenario is that you need to run three commands in the top level
directory of the downloaded software.

The first command is to run a script named ``configure`` provided with the software.

.. code-block:: none

    $ ./configure

The ``configure`` script makes sure that all the dependencies are present on your
system. For example if the software was written in ``C`` one of the tasks of the
``configure`` script would be to check that it could find a ``C`` compiler on your
system.

Another task that is commonly performed by the ``configure`` script is to create
a ``Makefile``. We already encountered the ``Makefile`` in
:doc:`automation-is-your-friend`. It is essentially a file describing how to build
the software.

Building the software, using the instructions in the ``Makefile``, is also the
next step of the process. This is typically achieved by running the ``make``
command.

.. code-block:: none

    $ make

The ``make`` command typically creates a number of executable files, often in
a subdirectory named ``build``.

The final step is to install the software. This is achieved by copying the
built executable files into a relevant directory present in your ``PATH``.
Since these directories are typically owned by root the final step typically
requires superuser privileges.

.. code-block:: none

    $ sudo make install


.. _installing_python_packages:

Installing Python packages
--------------------------

Python is a high-level scripting language that is relatively easy to read and
get to grips with.  We have already made use of Python in the previous
chapters.

It is possible to create re-usable software packages in Python. In fact
there are many such Python packages aimed at the scientific community.
Examples include `numpy <http://www.numpy.org/>`_
and `scipy <https://www.scipy.org/>`_ for numerical computing,
`sympy <http://www.sympy.org/en/index.html>`_ for symbolic mathematics,
`matplotlib <http://matplotlib.org/>`_ for figure generation,
`pandas <http://pandas.pydata.org/>`_ for data structures and analysis and
`scikit-learn <http://scikit-learn.org/stable/>`_ for machine learning.
There is also a package aimed directly at the biological community,
namely `biopython  <http://biopython.org/>`_.

Most packages are hosted on `PyPI <https://pypi.python.org>`_ and can
be installed using ``pip``. The ``pip`` command comes prepackaged with
Python since versions 2.7.9 and 3.4. If you have an older version of
Python you may need to install ``pip`` manually, see the
`pip installation notes <https://pip.pypa.io/en/latest/installing/#install-pip>`_
for more details.

Another really useful package is ``virtualenv``. I suggest installing it straight away.

.. code-block:: none

    $ sudo pip install virtualenv

Virtualenv is a tool that allows you to create
virtual Python environments.

.. sidebar:: What is a Python virtual environment?

    A Python virtual environment is essentially a local copy of your system's Python.
    The copy can live in your home directory.
    This means that you can install Python packages
    into your virtual environment without having root privileges. It also
    means that any packages that you install do not interfere with your system
    Python and vice versa. Furthermore, if you have multiple virtual
    environment they are all separate from each other. So if you have one
    virtual environment for each project that you are working on their Python
    packages can be of different versions without causing any problems.

Let's use ``virtualenv`` to create a virtual environment.

.. code-block:: none

    $ virtualenv env

Note that ``env`` is a directory containing all the required pieces for a
working Python system. To make use of our virtual environment we need to
activate it by sourcing the ``env/bin/activate`` script.

.. code-block:: none

    $ source ./env/bin/activate

This script basically mangles your ``PATH`` environment variable to ensure that
virtualenv's Python is found first. We can find out which version of Python
and ``pip`` is will be used by using the ``which`` command.

.. code-block:: none

    (env)$ which python
    /home/olssont/env/bin/python
    (env)$ which pip
    /home/olssont/env/bin/pip

.. note:: The ``./env/bin/activate`` script also changed the look of our prompt
          prefixing it with the name of the virtual environment.

Now let us install ``numpy`` into our virtual environment.

.. code-block:: none

    (env)$ pip install numpy

To list installed packages you can use the ``pip list`` command.

.. code-block:: none

    (env)$ pip list
    numpy (1.9.2)
    pip (6.0.8)
    setuptools (12.0.5)

When working on a Python project it can be useful to record the exact versions
of the installed packages to make it easy to reproduce the setup at a later
date. This is achieved using the ``pip freeze`` command.

.. code-block:: none

    (env)$ pip freeze
    numpy==1.9.2

Let us save this information into a file named ``requirements.txt``.

.. code-block:: none

    (env)$ pip freeze > requirements.txt

To show why this is useful let us deactivate the virtual environment.

.. code-block:: none

    (env)$ deactivate
    $ which python
    /usr/bin/python

.. note:: The ``deactivate`` command is created when you run the
          ``./env/bin/activate`` script.

Now let us create a new clean virtual environment, activate it and list
its packages.

.. code-block:: none

    $ virtualenv env2
    $ source ./env2/bin/activate
    (env2)$ pip list
    pip (6.0.8)
    setuptools (12.0.5)

Now we can replicate the exact same setup found in our initial virtual environment,
by running ``pip install -r requirements.text``.

.. code-block:: none

    (env2)$ pip install -r requirements.txt
    (env)$ pip list
    numpy (1.9.2)
    pip (6.0.8)
    setuptools (12.0.5)

This feature allows you make your data analysis more reproducible!

.. _installing_r_packages:

Installing R packages
---------------------

R is a scripting language with a strong focus on statistics and data
visualisation.

There are many packages available for R. These are hosted on
`CRAN <https://cran.r-project.org/>`_ (The Comprehensive R Archive Network).

To install an R package, for example ``ggplot2``, we need to start an R session.

.. code-block:: none

    $ R

    R version 3.2.2 (2015-08-14) -- "Fire Safety"
    Copyright (C) 2015 The R Foundation for Statistical Computing
    Platform: x86_64-apple-darwin14.5.0 (64-bit)

    R is free software and comes with ABSOLUTELY NO WARRANTY.
    You are welcome to redistribute it under certain conditions.
    Type 'license()' or 'licence()' for distribution details.

      Natural language support but running in an English locale

    R is a collaborative project with many contributors.
    Type 'contributors()' for more information and
    'citation()' on how to cite R or R packages in publications.

    Type 'demo()' for some demos, 'help()' for on-line help, or
    'help.start()' for an HTML browser interface to help.
    Type 'q()' to quit R.

    >

Then one can use the built-in ``install.packages`` function.
For exampel to install the ``ggplot2`` package one would use the
command below.

.. code-block:: R

    > install.packages("ggplot2")

This will prompt you for the selection of a mirror to download the package
from. Pick one close to you.

That's it, the ``ggplot2`` package is now available for you to use.
However, you need to load it using the ``library``
function to use it.

.. code-block:: R

    > library(ggplot2)
    

Installing Perl modules
-----------------------

Perl is a scripting language popular in the bioinformatics community. You may
therefore have to work with it.

There are a vast number of Perl modules available. These are hosted on
`CPAN <http://www.cpan.org/index.html>`_ (Comprehensive Perl Archive Network).

Traditionally, CPAN hosted packages are installed using the ``cpan`` command.
However, this can be quite cumbersome as it asks the user a lot of questions
with regards to how things should be configured. This resulted in people
developing a simpler tool to install Perl modules: ``cpanm`` (CPAN
Minus). You may be able to install ``cpanm`` using your distributions package
manager, if not you can install it using ``cpan``.

.. code-block:: none

    $ cpan App::cpanminus

When you run the command above you will notice that ``cpan`` prompts you for
a lot of information, accepting the defaults is fine. When it prompts you
to select an approach:

.. code-block:: none

    What approach do you want?  (Choose 'local::lib', 'sudo' or 'manual')

choose ``sudo``. This will install ``cpanm`` into a location that is immediately
available in your ``PATH``.

Now that you have installed ``cpanm`` you can use it to install Perl modules
more easily. For example to install the ``Bio::Tools::GFF`` module you can
simply use
the command below.

.. code-block:: none

    $ cpanm Bio::Tools::GFF



Installing Latex packages
-------------------------

TeX is a collection of programs and packages that allow you to typeset
documents. LaTeX is a number of macros built on top of TeX.  In
:doc:`collaborating-on-projects` we used Latex for producing a PDF version
of the document.

Confusingly there are many different distributions of TeX, for example the dominant
distribution of TeX on Windows' is `MiKTeX <http://miktex.org/>`_. On
Unix based systems the most commonly used TeX distribution is
`TeX Live <https://www.tug.org/texlive/>`_. And on Mac OSX it is
`MacTeX <https://www.tug.org/mactex/>`_.

In terms of package management Tex Live has got three different concepts: packages,
collections and schemes. A collection is a set of packages and a scheme is a group of
collections and packages. Scheme's can only be selected during the initial install
of TeX Live, whereas packages can be installed at any point.

One option is to use the ``scheme-full``, which includes everything meaning that
you are unlikely to need to install anything else. However, this can take a long
time and take up quite a lot of space on your system.

Another option is to start with a smaller scheme, for example
``scheme-basic``, ``scheme-minimal`` and ``scheme-small``. Other packages and
collections can then be installed as required.

Once you have install TeX Live you can manage it using the TeX Live Package
Manager (``tlmgr``).

To search for a package you can use the ``tlmgr search`` command.

.. code-block:: none

    $ tlmgr search fontsrecommended
    collection-fontsrecommended - Recommended fonts

To install a package/collection.

.. code-block:: none

    $ sudo tlmgr install collection-fontsrecommended


Automated provisioning
----------------------

As this chapter highlights managing software installations can be onerous and tedious.
What makes matters worse is that after you have installed a piece of software it can
be very easy to forget how one did it. So when you get a new computer you may find
yourself spending quite sometime configuring it so that all your analysis pipelines
work as expected.

Spending time configuring your system may be acceptable if you are the only
person depending on it. However, if other people depend on the machine
it is not. For example, you may end up responsible for a scientific web-service.
In these instances you should look into automating the configuration of
your system.

Describing how to do this is beyond the scope of this book. However, if you are
interested I highly recommend using `Ansible <https://www.ansible.com/>`_. To
get an idea of how Ansible works I suggest having a look at some of the blog
posts on my website, for example `How to create automated and reproducible work
flows for installing scientific software
<http://tjelvarolsson.com/blog/how-to-create-automated-and-reproducible-work-flows-for-installing-scientific-software/>`_.


Key concepts
------------

- The file system is structured like a tree staring from the root ``/``
- Programs are commonly located in ``/bin``, ``/usr/bin``, ``/usr/local/bin``
- The ``PATH`` environment variable defines where your shell looks for programs
- You need superuser privileges to write to many of default program locations
- The ``sudo`` command allows you to run another command with superuser privileges if you are in the sudoers list
- Package managers allow you to easily search for and install software packages
- Some software such as Python, R and Perl have their own built-in package managers
- It can be easy to forget how you how you configured your machine, do make notes
- Once you start finding it tedious making notes you should start thinking
  about automating the configuration of your system using a tool such as Ansible
