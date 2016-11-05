Working remotely
================

Sometimes you need to work on a computer different to the one that is in front
of you. This may be because you are working from home and need to log into your
work computer. Alternatively it may be because you need to do some work on a
high-performance computing cluster.

In this chapter we will cover tools for logging into remote machines using the
SSH (Secure SHell) protocol. SSH is an open standard that is available on all
UNIX based systems. In fact it has gained so much traction that Microsoft have
decided to add support for it in Windows.


Jargon busting
--------------

Although, it is relatively simple to log into a remote machine using SSH explaining
it requires the use of some networking terminology (as we are logging into the
remote machine over a network). Let us therefore start off with some jargon busting
before we get into the practical details.

A computer can be connected to a network using physical Ethernet cables or in a
wireless fashion using WiFI. Computers connected to the same network can
communicate with each other.  This communication takes place over so called
"ports", which are identified by numbers. A famous port is port 80, which is
used for HTTP (the HyperText Transfer Protocol) a.k.a. web-browsing. There are many
other ports. For example port 443 is used for HTTPS and port 22 is used for SSH.

Machines on a network can be identified and accessed via their IP (Internet
Protocol) address. An IP address that you may have come across is
``127.0.0.1``, it identifies the local machine that you are working on.

Although very useful, IP addresses can be difficult to remember. In order to overcome
this problem, internet service providers as well as the people responsible for looking
after your organisations network make use of domain name servers (DNS). The purpose
of a DNS server is to translate unique resource locations (URLs) to IP addresses.

A DNS server can also be used to lookup the IP address of a machine given a
simpler more memorable name. To illustrate this we can find the IP address(es)
of Google's web servers using the command ``host``.

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

Throughout the remainder of this chapter we will connect to remote machines
using their hostname. What happens in practise is that your institutes DNS
server translates this hostname to an IP address and you send your
communications over the network to the machine identified by that IP address.
Furthermore when you communicate with the remote machine you will be using
a specific port. The default port for the SSH protocol is port 22.


Logging in using the Secure Shell protocol
------------------------------------------

In its most basic form the ``ssh`` command takes on the form ``ssh hostname``.
Where ``hostname`` is the name of the remote machine that you want to log in to.
This assumes that you want to login to the remote machine using the same user
name as that on the local machine. This is often not the case and it is
common to see the ``ssh`` command in the form ``ssh user@hostname``, where
``user`` is the Unix user name you want to log in as on the remote machine.

Let us illustrate this with an example. Suppose that one wanted to login to a
remote computer named ``hpc``, this could for example be the head node on your
institutes high-performance computing cluster. Assuming that your user name on
the head node was ``olssont`` then you could log in using the command below.

.. code-block:: none

    $ ssh olssont@hpc

.. warning:: All of the remote machines in this chapter are fictional. This
             means that if you try to run the commands verbatim you will see
             errors along the lines of the below.

             .. code-block:: none

                ssh: Could not resolve hostname ...

             When trying these examples make sure that you are trying to connect
             to a machine that exists on your network.

If the machine that you are trying to log in to has a port 22 open (the 
default SSH port) you will be prompted for your password.

.. sidebar:: What is a head node?

    A computing cluster is basically a collection of computers. Computing
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

If your user name and  password authenticates successfully the shell in your terminal will
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

In the above the ``hostname`` command prints out the hostname of the local machine.

By default port 22 is used for the SSH protocol. However, sometimes a machine
may expose its SSH server on a different port. For example if
we had a machine called ``bishop`` that exposed its SSH server on port 2222
one could login to it using the command below.

.. code-block:: none

    $ ssh -p 2222 olssont@bishop

In the above the ``-p`` flag is used to specify the port to connect to.

Sometimes you want to be able to run software that makes use of windowing
systems (i.e. all software with a graphical user interface). For example the
statistical software package ``R`` has built in functionality for displaying
plots in a graphical window, which means it requires a windowing system. Most
Unix-based systems make use of the X11 as their windowing system. We therefore
need to enable X11-forwarding in SSH to be able to run programs that require
graphics. This is achieved using the ``-X`` flag.

.. code-block:: none

    $ ssh -X olssont@pawn

In the above we are connecting to a machine named ``pawn`` with X11-forwarding
enabled.


Copying files using Secure Copy
-------------------------------

Now that we know how to login to a remote machine we need to work out how to
copy data to and from it. This is achieved using the ``scp``, *secure copy*,
command.

Suppose that we wanted to copy the file ``mydata.csv`` over to ``olssont``'s
home directory on the ``hpc`` head node, we could achieve this using the
command below.

.. code-block:: none

    $ scp mydata.csv olssont@hpc:

Note the colon (``:``) after the host name. It demarcates the end of the host name
and the beginning of the location to copy the file to on the remote machine.
In this instance the latter is left empty and as such the original file name is used
and the location for the file defaults to ``olssont``'s home directory on the remote
machine. The command above is equivalent to that below which specifies the home directory
using a relative path (``~/``).

.. code-block:: none

    $ scp mydata.csv olssont@hpc:~/

It is also possible to specify the location using an absolute path. For example
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
So if we wanted to copy the data to ``bishop``, where the SSH server is listening
on port 2222 one could use the command below.

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


Password-less authentication using SSH keys
-------------------------------------------

An alternative and more secure method to using password based authentication is to use
public-key cryptography. Public-key cryptography, also known as asymmetric cryptography,
uses a pair of so called "keys". One of these keys is public and one is private. The
public key is one that you can distribute freely, in this case to all the remote machines
that you want to be able to login to. However, the private key must never be compromised
as it is what allows you access to all the remote machines. One way to think about this
system is to view the public key as a lock that only the private key can open.
You can fit the all the machines that you want secure access to with copies of
the same public key as long as you keep the private key safe.

Enough theory let's try it out.

The first step is to generate a public/private key pair. This is achieved using
the command ``ssh-keygen``. This will prompt you for the file to save the key
as, the default ``~/.ssh/id_rsa`` file is a good option if you have not yet
setup any key pairs. You will then be prompted, to optionally, enter a
passphrase.  This provides another layer of protection in case someone gets
hold of your private key. However, it does mean that you will be prompted for
the passphrase the first time you make use of the key in a newly booted system.
Personally, I am paranoid so I make use of the passphrase and I suggest that
you do too.

.. code-block:: none

    $ ssh-keygen
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/olssont/.ssh/id_rsa):
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:

If you used the default naming scheme for your key pair you should now have two
files in your ``.ssh`` directory: ``id_rsa`` (your private key) and ``id_rsa.pub``
(your public key).

.. code-block:: none

    $ ls -l .ssh/
    -rw-------  1 olssont  NR4\Domain Users   1679 23 Feb  2015 id_rsa
    -rw-r--r--  1 olssont  NR4\Domain Users    407 23 Feb  2015 id_rsa.pub

Note that only the user has read/write permissions on the private key, whereas
the pubic key is readable by everyone.

Now let us setup password-less login to the cluster head node. First of all let
us copy the public key to the remote machine using ``scp``.

.. code-block:: none

    $ scp ~/.ssh/id_rsa.pub olssont@hpc:

Now we need to login to the head node to configure it. At this point we will
still need to use our password. Once logged into the head node we need to
create a ``.ssh`` directory in the user's home directory (if it does not
already exist). We then need to append the public key to a file named
``authorized_keys`` in the ``.ssh`` directory. Finally we logout of the head
node.

.. code-block:: none

    $ ssh olssont@hpc
    $ hostname
    hpc
    $ mkdir .ssh
    $ cat id_rsa.pub >> .ssh/authorized_keys
    $ exit

.. sidebar:: What does the ``>>`` expression do?

    The ``>>`` symbol is similar to the ``>`` redirection symbol. However,
    redirection using ``>`` will replace an existing file whereas ``>>`` will
    append to it. In this case we do not want to destroy any previously added
    public keys so we append to the ``authorized_keys`` file.

Now we should be able to ``ssh`` and ``scp`` to the head node in a password-less fashion.
If you setup your key pair using a passphrase you will be prompted for it the first time
you use the key pair.

Great that's really cool! However, it was quite a lot of work to get the public key onto
the remote machine. There is a better way to do this using the program ``ssh-copy-id``.
Depending on the operating system that you are using may need to install this
program, see :doc:`managing-your-system` for details on how to install
software.

Once you have ``ssh-copy-id`` on your system you can provision a remote machine with your
public key using a single command. Below we use it to add our pubic key to ``bishop``.

.. code-block:: none

    $ ssh-copy-id -i ~/.ssh/id_rsa.pub olssont@bishop

The optional ``-i`` flag is used to specify which public key should be copied
to the remote machine.


Managing your login details using SSH config
--------------------------------------------

Suppose that access to your institutes cluster was setup in a way that required
you to use the full ``hpc.awesomeuni.ac.uk`` host name, but that you wanted to
be able to login using the shorter name ``hpc``. You can configure your machine
to setup access in this fashion by creating the file ``.ssh/config`` file and adding
the lines below to it.

.. code-block:: none

    Host hpc
        HostName hpc.awsomeuni.ac.uk
        User olssont

The SSH configuration above also specifies the user name. This means that you
can login to the head node using the command below (note the lack of a user
name).

.. code-block:: none

    $ ssh hpc


As you start using
SSH keys to manage access to various machines you are likely to find yourself
using multiple key pairs. In this case you will want to be able to specify the name
of the private key, also known as an identity file, in the ``.ssh/config`` file.

.. code-block:: none

    Host hpc
        HostName hpc.awsomeuni.ac.uk
        User olssont
        IdentityFile ~/.ssh/id_rsa

Finally in the examples described earlier access to ``bishop`` had been configured
to use port 2222. To configure access to this remote machine we could use the
specification below.

.. code-block:: none

    Host bishop
        HostName bishop
        User olssont
        Port 2222
        IdentityFile ~/.ssh/id_rsa

Again, using the ``.ssh/config`` file in this way means that we do not need to remember
port numbers and what options to invoke the ``scp`` and ``ssh`` commands with.
Copying a file can then be achieved using the concise syntax below.

.. code-block:: none

    $ scp mydata.csv bishop:

Logging in to the machine becomes similarly trivial.

.. code-block:: none

    $ ssh bishop


Executing long running commands on remote hosts
-----------------------------------------------

One problem that one can encounter when working on a remote machine is that if
the connection is broken whilst a program is running it may fail.

Luckily, it is quite easy to work around this. All one needs to do is to prefix
the command to run the program of interest with ``nohup``. The ``nohup`` command
makes the program of interest immune to hangups.

To see this in action open up two terminals on your computer. In one of them
we will monitor the running processes using the command ``top``.

.. code-block:: none

    $ top

This should display a lot of information about the current running processes. To
make things a little easier to digest we can limit the output to the processes
owned by you. Press ``U``, which will prompt you for a user name. Enter your
user name and press enter. You should now only see the processes owned by you.

In the second terminal we will simulate a long running program using the
command ``sleep``, which simply pauses execution for a specified number of
seconds.

.. code-block:: none

    $ sleep 3600

In the first terminal, running ``top``, you should now see the ``sleep`` program
running.

Now close the second terminal, the one in which you are running the ``sleep``
command. Note that the ``sleep`` program disappears from the ``top`` display.
This is because the program was interrupted by the closing of the terminal.

Open a new terminal. This time we will prefix the ``sleep`` command with ``nohup``.


.. code-block:: none

    $ nohup sleep 3600

Now close the terminal running the ``sleep`` command again. Note that the ``sleep``
command is still present in the ``top`` display. It will keep running until it is
finished in an hours time.


Key concepts
------------

- You can use the ``ssh`` command to login to remote machines
- You can copy data to and from remote machines using the ``scp`` command
- You can use SSH keys to avoid having to type in your password every time you want to interact with a remote machine
- Using SSH keys is also more secure than using passwords
- If you need to interact with many remote machines it may make sense to create a ``.ssh/config`` file
- You can use ``nohup`` to ensure that long running processes are not killed by losing connection to the remote machine
