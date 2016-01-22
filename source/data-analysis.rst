Data analysis
=============

Introduction
------------

*Streptomyces coelicolor* is a soil-dwelling bacterium with a complex life-cycle
involving mycelial growth and sporulation. It is of particular interest in that
it can produce a range of natural products of pharmaceutical relevance. In fact
it is the major source of natural antibiotics.

In 2002 the genome of *Streptomyces coelicolor*  A3(2), the model actinomycete
organism, was sequenced. In this chapter we will do some basic bioinformatics
on this genome to illustrate some fundamental concepts of computer programming.

One feature of interest when examining DNA is the guanine-cytosine (GC) content.
DNA with high GC-content is more stable than DNA with low GC-content.
The GC-content is defined as:

.. math::

    100 *  \frac{G + C}{A + T + G + C} 


In this chapter we will use the Python scripting language to perform our data
analysis.


What is Python?
---------------

Python is a high-level scripting language that is growing in popularity in the
scientific community. It uses a syntax that is relatively easy to get to grips
with and which encourages code readability.

Python is used because it is easy to learn and because it is becoming the
*de facto* scripting language in the scientific community.


Using Python in interactive mode
--------------------------------

In this section we will make use of Python using its
`interactive mode <https://docs.python.org/2/tutorial/interpreter.html#interactive-mode>`_,
which means that we can type Python commands straight into the terminal. In fact
when working with Python in interactive mode one can think of it as switching
the terminal's shell from Bash to Python.

To start Python in its interactive mode simply type ``python`` into the terminal.

.. code-block:: none

    $ python
    Python 2.7.10 (default, Jul 14 2015, 19:46:27)
    [GCC 4.2.1 Compatible Apple LLVM 6.0 (clang-600.0.39)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    >>>

Note that this prints out information about the version of Python that is being used
and how it was compiled before leaving you at the interactive prompt. In this instance
I am using Python version 2.7.10.

The three greater than signs (``>>>``) represent the primary prompt into which
commands can be entered.

.. code-block:: python

    >>> 1 + 2
    3

The secondary prompt, used for continuation lines, is represented by three dots
(``...``).

.. code-block:: python

    >>> line = ">sp|Q6GZX4|001R_FRG3G Putative transcription factor 001R"
    >>> if line.startswith(">"):
    ...     print(line)
    ...
    >sp|Q6GZX4|001R_FRG3G Putative transcription factor 001R


Variables
---------

A variable is a means of storing a piece of information using using a
descriptive name. The use of variables is encouraged as it allows us to
avoid having to repeat ourselves, the :term:`DRY` principle.

In Python variables are assigned using the equals sign.

.. code-block:: python

    >>> pi = 3.14

When naming variables being explicit is more important than being succinct.
One reason for this is that you will spend more time reading your code than
you will writing it. Avoiding the mental overhead of trying to understand
what all the acronyms mean is a good thing. For example, suppose that we
wanted to create a variable for storing the radius of a circle. Please
avoid the temptation of naming the variable ``r``, and go for the longer
but more explicit name ``radius``.

.. code-block:: python

    >>> radius = 1.5


Determining the GC count of a sequence
--------------------------------------

Suppose that we had a :term:`string` representing a DNA sequence.

.. code-block:: python

    >>> dna_string = "attagcgcaatctaactacactactgccgcgcggcatatatttaaatata"
    >>> print(dna_string)
    attagcgcaatctaactacactactgccgcgcggcatatatttaaatata

A string is a data type for representing text. As such it is not ideal for data
processing purposes. In this case the DNA sequence would be better represented
using a :term:`list`, with each item in the list representing a DNA letter.

In Python we can convert a string into a list using the built-in :func:`list`
function.

.. code-block:: python

    >>> dna_list = list(dna_string)
    >>> print(dna_list)  # doctest: +NORMALIZE_WHITESPACE
    ['a', 't', 't', 'a', 'g', 'c', 'g', 'c', 'a', 'a', 't', 'c', 't', 'a', 'a',
     'c', 't', 'a', 'c', 'a', 'c', 't', 'a', 'c', 't', 'g', 'c', 'c', 'g', 'c',
     'g', 'c', 'g', 'g', 'c', 'a', 't', 'a', 't', 'a', 't', 't', 't', 'a', 'a',
     'a', 't', 'a', 't', 'a']

Python's list :term:`class` has got a method called :func:`count` that we can use
to find out the counts of particular elements in the list.

.. code-block:: python

    >>> dna_list.count("a")
    17

To find out the total number of items in a list one can use Python's built-in
:func:`len` function, which returns the *length* of the list.

.. code-block:: python

    >>> len(dna_list)
    50

When using Python you need to be careful when dividing integers, because in Python 2
the default is to use integer division, i.e. to discard the remainder.

.. code-block:: python

    >>> 3 / 2
    1

One can work around this by ensuring that at least one of the numbers is
represented using floating point.

.. code-block:: python

    >>> 3 / 2.0
    1.5

.. warning:: In Python 3, the behavour of the division operator has been
             changed, and dividing two integers will result in normal division.

One can convert an integer to a floating point number using Python's built-in
:func:`float` function.    

.. code-block:: python

    >>> float(2)
    2.0

We now have all the information required to calculate the GC-content of the DNA
sequence.

.. code-block:: python

    >>> gc_count = dna_list.count("g") + dna_list.count("c")
    >>> gc_frac = float(gc_count) / len(dna_list)
    >>> 100 * gc_frac
    38.0


Creating reusable functions
---------------------------

Suppose that we wanted to calculate the GC-content for several sequences. In
this case it would be very annoying, and error prone, to have to enter the
commands above into the Python shell manually for each sequence. Rather, it
would be advantageous to be able to create a piece of code that could be called
repeatedly to calculate the GC-content. We can achieve this using the concept of
functions. In other words functions are a means for programmers to avoid repeating
themselves, thus adhering to the :term:`DRY` principle.

Let us create a simple function that adds to items together.

.. code-block:: python

    >>> def add(a, b):
    ...     return a + b
    ...
    >>> add(2, 3)
    5

In Python functions are defined using the ``def`` keyword. Note that the
``def`` keyword is followed by the name of the function. The name of the
function is followed by a parenthesized set of arguments, in this case the
function takes two arguments ``a`` and ``b``. The end of the function
definition is marked using a colon.

The body of the function, the return statement, needs to be indented. The
standard in Python is to use four white spaces to indent code blocks. In this
case the function body only contains one line of code. However, a function
can include several indented lines of code.

.. warning:: Whitespace really matters in Python! If your code is not correctly
             aligned you will see ``IndentationError`` messages telling you
             that everything is not as it should be. You will also run into
             ``IndentationError`` messages if you mix white spaces and tabs.

Now we can create a function for calculating the GC-content of a sequence.
As with variables explicit trumps succinct in terms of naming.

.. code-block:: python

    >>> def gc_content(sequence):
    ...     "Return GC-content as a percentage from a list of DNA letters."
    ...     gc_count = sequence.count("g") + sequence.count("c")
    ...     gc_fraction = float(gc_count) / len(sequence)
    ...     return 100 * gc_fraction
    ...
    >>> gc_content(dna_list)
    38.0


List slicing
------------

Suppose that we wanted to look at local variability in GC-content. To achieve
this we would like to be able to select segments of our initial list. This is
known as "slicing", as in slicing up a salami.

In Python slicing uses a ``[start:end]`` syntax that is inclusive for the start
index and exclusive for the end index. To illustrate slicing let us first
create a list to work with.

.. code-block:: python

    >>> zero_to_five = ["zero", "one", "two", "three", "four", "five"]

To get the first two elements.

.. code-block:: python

    >>> zero_to_five[0:2]
    ['zero', 'one']

Note that the start position for the slicing is 0 by default so we could just
as well have written.

.. code-block:: python

    >>> zero_to_five[:2]
    ['zero', 'one']

To get the last three elements.

.. code-block:: python

    >>> zero_to_five[3:]
    ['three', 'four', 'five']

It is worth noting that we can use negative indices, where -1 represents the
last element. So to get all elements except the first and the last one could
slice the list using the indices 1 and -1.

.. code-block:: python

    >>> zero_to_five[1:-1]
    ['one', 'two', 'three', 'four']

Let us now use list slicing to calculate the local GC-content measurements of
our DNA.


.. code-block:: python

    >>> gc_content(dna_list[:10])
    40.0
    >>> gc_content(dna_list[10:20])
    30.0
    >>> gc_content(dna_list[20:30])
    70.0
    >>> gc_content(dna_list[30:40])
    50.0
    >>> gc_content(dna_list[40:50])
    0.0


Loops
-----

It can get a bit repetitive, tedious, and error prone specifying all the ranges
manually. A better way to do this is to make use of a loop construct. A loop
allows a program to cycle through the same set of operations a number of times.

In lower level languages *while* loops are common because they operate in a way
that closely mimic how the hardware works. The code below illustrates a typical
setup of a while loop.

.. code-block:: python

    >>> cycle = 0
    >>> while cycle < 5:
    ...     print(cycle)
    ...     cycle = cycle + 1
    ...
    0
    1
    2
    3
    4

However, when working in Python it is much more common to make use of *for*
loops. For loops are used to iterate over elements in data structures such as
lists.

.. code-block:: python

    >>> for item in [0, 1, 2, 3, 4]:
    ...     print(item)
    ...
    0
    1
    2
    3
    4

In the above we had to manually write out all the numbers that we wanted. However,
because iterating over a range of integers is such a common task Python has a
built-in function for generating such lists.

.. code-block:: python

    >>> range(5)
    [0, 1, 2, 3, 4]

So a typical for loop might look like the below.

    >>> for item in range(5):
    ...     print(item)
    ...
    0
    1
    2
    3
    4

The :func:`range` function can also be told to start at a larger number. Say for
example that we wanted a list including the numbers 5, 6 and 7.

.. code-block:: python

    >>> range(5, 8)
    [5, 6, 7]

As with slicing the start value is included whereas the end value is excluded.

It is also possible to alter the step size. To do this we must specify the start
and end values explicitly before adding the step size.

.. code-block:: python

    >>> range(0, 50, 10)
    [0, 10, 20, 30, 40]

We are now in a position where we can create a naive loop for for calculating
local GC-content of our DNA.

.. code-block:: python

    >>> for start in range(0, 50, 10):
    ...     end = start + 10
    ...     print(gc_content(dna_list[start:end]))
    ...
    40.0
    30.0
    70.0
    50.0
    0.0

Loops are really powerful. They provide a means to iterate over lots of items
and as such to automate repetitive tasks.


Creating a sliding window GC-content function
---------------------------------------------

So far we have been working with Python in interactive mode. This is a great
way to explore what can be done with Python. It is also a handy way to get
access to a calculator from the command line. However, it can get a little
bit clunky when trying to write constructs that span serval lines, such as
functions.

Now we will examine how one can write a Python script as a text file and
how to run that text file through the Python interpreter, i.e. how to
run a Python script from the command line.

Use your favourite text editor to enter the code below into a file
named ``gc_content.py``.

.. code-block:: python

    sequence = list("attagcgcaatctaactacactactgccgcgcggcatatatttaaatata")
    print(sequence)

.. note:: If your text editor is not giving your syntax highlighting find out
          how it can be enabled. If your text editor does not support syntax
          highlighting find a better text editor!

Open up a terminal and go to the directory where you saved the ``gc_content.py``
script. Run the script using the command below.

.. code-block:: none

    $ python gc_content.py

You should see the output below printed to your terminal.

.. code-block:: none

    ['a', 't', 't', 'a', 'g', 'c', 'g', 'c', 'a', 'a', 't', 'c', 't', 'a', 'a',
    'c', 't', 'a', 'c', 'a', 'c', 't', 'a', 'c', 't', 'g', 'c', 'c', 'g', 'c',
    'g', 'c', 'g', 'g', 'c', 'a', 't', 'a', 't', 'a', 't', 't', 't', 'a', 'a',
    'a', 't', 'a', 't', 'a']

In the script we used Python's built-in :func:`list` function to convert the
DNA string into a list. We then printed out the ``sequence`` list.

- Sliding window analysis of GC content
- def(sequence, window_size, step_size): 


Downloading the genome
----------------------

.. code-block:: none

    $ curl --location --output Sco.dna http://bit.ly/1Q8eKWT
    $ head Sco.dna
    SQ   Sequence 8667507 BP; 1203558 A; 3121252 C; 3129638 G; 1213059 T; 0 other;
         cccgcggagc gggtaccaca tcgctgcgcg atgtgcgagc gaacacccgg gctgcgcccg        60
         ggtgttgcgc tcccgctccg cgggagcgct ggcgggacgc tgcgcgtccc gctcaccaag       120
         cccgcttcgc gggcttggtg acgctccgtc cgctgcgctt ccggagttgc ggggcttcgc       180
         cccgctaacc ctgggcctcg cttcgctccg ccttgggcct gcggcgggtc cgctgcgctc       240
         ccccgcctca agggcccttc cggctgcgcc tccaggaccc aaccgcttgc gcgggcctgg       300
         ctggctacga ggatcggggg tcgctcgttc gtgtcgggtt ctagtgtagt ggctgcctca       360
         gatagatgca gcatgtatcg ttggcagaaa tatgggacac ccgccagtca ctcgggaatc       420
         tcccaagttt cgagaggatg gccagatgac cggtcaccac gaatctaccg gaccaggtac       480
         cgcgctgagc agcgattcga cgtgccgggt gacgcagtat cagacggcgg gtgtgaacgc       540


Reading and writing files
-------------------------

- Reading in the streptomyces sequence
- Writing out the sliding window analysis


Key concepts
------------
