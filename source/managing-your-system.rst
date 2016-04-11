Managing your system
====================

Introduction
------------

This chapter will give a brief overview of how to install software on
Unix-like systems. Different systems operate in different ways and this
can lead to confusion. This chapter aims to help you an understanding of
the basic principles as well as an overview of the most common systems giving
you a solid foundation to build upon.


The file system
---------------

The file system on Unix-like systems is built up like a tree starting from
the root. You can view the content of your root system by typing in ``ls /``.

On a linux box this may look like the below.

.. code-block:: none

    $ ls /
    bin  dev   etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root
    run  sbin  srv  sys   tmp  usr    var

The files that you have been working on so far have been located in your home
directory, e.g. ``/home/olssont/``.

However, the programs that you have been running are also files. Software
fundamental to the operating system is located in ``/bin``. Other software
installed by the systems package manager tend to be located in ``/usr/bin``.

To find out the location of a program you can use the ``which`` command. For
example, let us find out the location of the ``ls`` program.

.. code-block:: none

    $ which ls
    /bin/ls

We can run the ``ls`` program using this full path, for example to view the content
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
tells the shell the locations to go looking for programs in. We can inspect the
content of the ``PATH`` environment variable using the ``echo`` command.

.. code-block:: none

    $ echo $PATH
    /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin

The ``PATH`` variable basically contains a list of colon (``:``) separated directories.
When you try to run a program the shell looks in these directories, in order, and uses
the first match it can find. If the program of interest is not located in any of these
directories you will see a ``command not found`` error issued from your shell.

.. code-block:: none

    $ this-program-does-not-exist
    bash: this-program-does-not-exist: command not found


The ``root`` user
-----------------

Now let us have a look at the permissions of the ``/usr/bin`` directory.
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


Installing software
-------------------

A consequence of the fact that only the ``root`` user can write files to the
``/bin`` and ``/usr/bin`` directories is that you need to have root privileges
to install software (write files) to these default locations.

All modern Linux distribution come with a so called package manager, which should
be your first of call when trying to install software. Package managers make it
easier to install software for two main reasons they deal with the dependencies
for you and they (usually) provide pre-compiled versions of software that are known
to play nicely with the other software available through the package manager.

Although Mac's come with the AppStore this is not the place to look for scientific
software. Instead two other options based on the idea of the Linux package managers
have evolved the first one is `Mac Ports <https://www.macports.org/>`_ and the
second one is `Homebrew <http://brew.sh/>`_. I would recommend using the latter as
it has thriving scientific user community.


- su and sudo
- installing software on debian based systems
- installing software on redhat based systems
- installing software on Macs
- compiling software from source
- installing Python packages using pip
- installing Latex packages using tmgr
- installing R packages
- installing Perl libraries
- installing Ruby libraries
- Ansible
