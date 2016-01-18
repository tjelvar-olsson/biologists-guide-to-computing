Data analysis
=============

Introduction
------------

*Streptomyces coelicolor* is a soil-dwelling bacterium with a complex life-cycle
involving mycelial growth and sporulation. It is of particular interest in that
it can produce a range of natural products of pharmaceutical relevance. In fact
it is the source of the majority of natural antibiotics.

In 2002 the genome of *Streptomyces coelicolor*  A3(2), the model actinomycete
organism, was sequenced. In this chapter we will do some basic bioinformatics
on this genome to illustrate some fundamental concepts of computer programming.

One feature of interest when examining DNA is the guanine-cytosine (GC) content,
because DNA with high GC-content is more stable than DNA with low GC-content.
The GC-content is defined as:

.. math::

    GC content = 100 *  \frac{number of G + number of C}{number of A + number of T} 


In this chapter we will use the Python scripting language to perform our data
analysis.


What is Python?
---------------

Python is a high-level scripting language that is growing in popularity in the
scientific community. It uses a syntax that is relatively easy to get to grips
with and which encourages code readability.

Here we use Python because it is easy to learn and because it is becoming the
*de facto* scripting language in the scientific community.


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


Using Python in interactive mode
--------------------------------

Here we will make use of Python in an
`interactive mode <https://docs.python.org/2/tutorial/interpreter.html#interactive-mode>`_,
which means that we can type Python commands straight into the terminal. In fact
when working with Python in an interactive mode one can think of it as switching
the terminal's shell from Bash to Python.

To start Python in an interactive mode simply type ``python`` into the terminal.

.. code-block:: none

    $ python
    Python 2.7.10 (default, Jul 14 2015, 19:46:27)
    [GCC 4.2.1 Compatible Apple LLVM 6.0 (clang-600.0.39)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    >>>

The three greater than signs (``>>>``) represent the primary prompt into which
commands can be entered.

.. code-block:: python

    >>> print("hello")
    hello

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

- Why use variables
- When to use variables
- How to name variables


Determining the GC count of a sequence
--------------------------------------


Creating reusable functions
---------------------------

- Why one should use functions
- When one should use functions
- How to split logic into separate functions (a function should only do one thing),
  i.e. one function for reading in a sequence and one function for determining the GC
  count of a sequence

Loops
-----

- Sliding window analysis of GC content


Reading and writing files
-------------------------

- Reading in the streptomyces sequence
- Writing out the sliding window analysis


Key concepts
------------
