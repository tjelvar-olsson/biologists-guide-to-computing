Working remotely
================

Introduction
------------

Sometimes you need to work on a computer different to the one that is in front
of you. This may be because you are working from home and need to log into your
work computer. Alternatively it may be because you need to do some work on a
high-performance computing cluster.

In this chapter we will cover tools for logging into remote machines using the
SSH (secure shell) protocol. SSH is an open standard that is available on all
UNIX based systems. In fact it has gained so much traction that Microsoft have
decided to add support for it in Windows.

.. warning:: All of the remote machines in this chapter are fictional. This
             means that if you try to run the commands verbatim you will see
             errors along the lines of the below.

             .. code-block:: none

                $ ssh olssont@bishop
                ssh: Could not resolve hostname bishop: nodename nor servname provided, or not known


Jargon busting
--------------

Although, it is relatively simple to log into a remote machine using SSH explaining
it requires the use of some networking terminology (as we are logging into the
remote machine over a network). Let us therefore start off with some jargon busting
before we get into the practical details.

When a computer is connected to a network it can communicate with other computers on
the network using various ports. Ports are numbered and a famous one is port 80, which
is used for HTTP (the HyperText Transfer Protocol) a.k.a. web-browsing.

Machines on a network can be identified and accessed via their IP (Internet
Protocol) address. Your local machine can be accessed via the IP address
``127.0.0.1``.

Although very useful IP addresses can be difficult to remember. In order to overcome
this internet service providers as well as the people responsible for looking
after your organisations network make use of domain name servers (DNS). The purpose
of a DNS server is to translate unique resource locations (URLs) to IP addresses. A
DNS server can also be used to provide a means to lookup the IP address of a
machine given a simpler more memorable name. To illustrate this we can find the
IP address(es) of Google using the command ``host``.

.. code-block:: none

    $ host www.google.com
    www.google.com has address 62.164.169.148
    www.google.com has address 62.164.169.185
    www.google.com has address 62.164.169.163
    www.google.com has address 62.164.169.177
    www.google.com has address 62.164.169.166
    www.google.com has address 62.164.169.152
    www.google.com has address 62.164.169.155
    www.google.com has address 62.164.169.154
    www.google.com has address 62.164.169.176
    www.google.com has address 62.164.169.159
    www.google.com has address 62.164.169.165
    www.google.com has address 62.164.169.181
    www.google.com has address 62.164.169.187
    www.google.com has address 62.164.169.170
    www.google.com has address 62.164.169.144
    www.google.com has address 62.164.169.174
    www.google.com has IPv6 address 2a00:1450:4009:810::2004

If you copy and paste one of the IP addresses above into a web browser you
should see the Google home page.



Logging in using the Secure Shell protocol
------------------------------------------

In its most basic form the ``ssh`` command takes on the form ``ssh hostname``.
Where ``hostname`` is the name of the remote machine that you want to login to.
This assumes that you want to login to the remote machine using the same user
name as that on the local machine. This is often not the case and it is much
more common to see the ``ssh`` command in the form ``ssh user@hostname``, where
``user`` is the Unix user name you want to login as on the remote machine.

Let us illustrate this with an example. Suppose that one wanted to login to a
remote computer named ``hpc``, this could for example be the head node on your
institutes high-performance computing cluster. Assuming that your user name on
the head node was ``olssont`` then you could log using the command below.

.. code-block:: none

    $ ssh olssont@hpc

If the machine that you are trying to login to has a port open on port 22 (the
default SSH port) you will be prompted for your password.

.. sidebar:: What is a head node?

    A computing clusters is basically a collection of computers. Computing
    clusters tend to make use of a scheduler to distribute jobs on the cluster.
    A scheduler is basically a piece of software that has an understanding of the
    computing resources available on the cluster. A user submits a job to the
    scheduler and the scheduler puts the job in a queue and when appropriate resources
    become available it starts the job on the cluster.

    Most schedulers require a so
    called head node, also known as a master node, which acts as the control centre.
    A user of a cluster would therefore login to the head node and from there the user
    would submit a job to the scheduler. The scheduler would then dispatches
    the job on one or more cluster nodes when the appropriate resources became
    available.

The above assumes that your DNS server can resolve the name ``hpc`` into an IP
address. Depending on how things are setup in your organisation you may need to
use a more explicit name, for example
``hpc.awesomeuni.ac.uk``.

.. code-block:: none

    $ ssh olssont@hpc.awesomeuni.ac.uk

If your password authentication passes login the shell in your terminal will
now be from the remote machine. To find out the name of the machine that you
are logged into you can run the command ``hostname``.

.. code-block:: none

    $ hostname
    hpc

To disconnect from the remote host you can use :kbd:`Ctrl-D` or the ``exit``
command.

.. code-block:: none

    $ exit
    $ hostname
    olssont-laptop

In the above the ``hostname`` command prints out the host name of the local machine.

By default port 22 is used for the SSH protocol. However, sometimes a machine
may expose its SSH server on a different port. For example if
we had a machine called ``bishop`` that exposed its SSH server on port 2222
one could login to it using the command below.

.. code-block:: none

    $ ssh -p 2222 olssont@bishop

Sometimes you want to be able to run software that makes use of graphics, for
example to run the statistical software package ``R``. Most UNIX-based system
make use of the X11 protocol for drawing graphics on the computer screen. We
therefore need to enable X11-forwarding in SSH to be able to run programs that
require graphics. This is achieved using the ``-X`` flag. Below we use this
flag to login to a machine named ``pawn``.

.. code-block:: none

    $ ssh -X olssont@pawn


Copying files using Secure Copy
-------------------------------

Now that we know how to login to a remote machine we need to work out how to
copy data to it. This is achieved using the ``scp``, *secure copy*, command.

Suppose that we wanted to copy the file ``mydata.csv`` over to ``olssont``'s
home directory on the ``hpc`` head node, we could achieve this using the
command below.

.. code-block:: none

    $ scp mydata.csv olssont@hpc:

Note the colon (``:``) after the host name. It demarcates the end of the host name
and the beginning of the file specification to copy the file to on the remote machine.
In this instance the latter is left empty and as such the original file name is used
and the location for the file defaults to ``olssont``'s home directory on the remote
machine. The command above is equivalent to that below which specifies the home directory
using a relative path (``~/``).

.. code-block:: none

    $ scp mydata.csv olssont@hpc:~/

It is also possible to specify the location using absolute paths. For example
if we wanted to save the file in the ``/tmp`` directory this could be achieved
using the command below.

.. code-block:: none

    $ scp mydata.csv olssont@hpc:/tmp/

Just like with the ``cp`` command it is possible to give the copied file a
different name. For example to name it ``data.csv`` (and place it in the ``/tmp``
directory) one could use the command below.

.. code-block:: none

    $ scp mydata.csv olssont@hpc:/tmp/data.csv

If the SSH server is listening on a port other than 22 one needs to specify the port
explicitly. Confusingly the argument for this is not the same as for the ``ssh`` command.
The ``scp`` command uses the argument ``-P``, i.e. it uses upper rather than lower case.
So if we wanted to copy the data to ``bishop`` one could use the command below.

.. code-block:: none

    $ scp -P 2222 mydata.csv olssont@bishop:

Sometimes one wants to copy the entire content of a directory. In this case one can use
the ``-r`` option to *recursively* copy all the content of the specified directory. For
example if we had a directory named ``data`` and we wanted to copy it to ``pawn`` one
could use the command below.

.. code-block:: none

    $ scp -r data/ olssont@pawn:

All of the commands above will prompt you for your password. This can get tedious.
In the next section we will look at a  more secure and less annoying way of
managing the authentication step when working with remote machines.


- ssh-keys
- ctrl-z, bg, fg
- nohup
