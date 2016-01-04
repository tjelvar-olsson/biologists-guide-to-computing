How to think like a computer
============================

These days computers can perform complex task. However, at their core computers
only have a relatively small set of basic capabilities. Information is stored as
zeros and ones and Boolean logic is used to perform calculations.

This chapter will cover fundamental aspects of computing required to learn
scripting and programming. It will introduce bits and bytes and how they can be
used to represent numbers and characters (as in letters and symbols). The
chapter will also give a brief overview of how a computer stores information
and performs calculations.


Binary, bits and bytes
----------------------

In computing a bit is the smallest unit of information and it can be either
zero or one. Suppose that we wanted to represent a positive integer using
zeros and ones. We can achieve this using using the base-2 numeral systems,
a.k.a. binary.

.. sidebar:: Integer

   An integer is a whole number, i.e. a number that can be expressed without a
   fractional component. In computing people often make a distinction between
   signed and unsigned integers. The former includes negative values and the
   latter does not.


Take for example the number 208. This number can be represented as two sets
of hundreds and eight sets of ones (as well as zero sets of tens).

.. math::

    208 &= 200 + 0 + 8 \\
        &= (2*100) + (0 * 10) + (8 * 1) \\
        &= (2 * 10^2) + (0 * 10^1) + (8 * 10^0)

The equations above illustrate the basis of the base-10 numeral system. Note
that the base of the exponent in each term is ten. This means that one needs
ten states to represent numbers. However, computers only have two states 0 and
1. To express an integer in binary we therefore need to formulate it so that
the base of each exponent is 2.

.. math::

    208 &=  (1*64) + (1*32) + (0*16) + (1*8) + (1*4) + (0*2) + (0*1)  \\
        &=  (1 * 2^6) + (1 * 2^5) + (0 * 2^4) + (1 * 2^3) + (1 * 2^2) + (0 * 2^1) + (0 * 2^0)

.. sidebar:: Bit

    In computing the term "Bit" is an abbreviation of the term "Binary digIT".

So to express 208 as binary we could use the bits ``1101100``.

To recap in computing a bit is the smallest unit of information and it can be
either zero or one. Using several bits we can express integers using the binary
numeral system.

In computing bits are often grouped together and the most common grouping is to
bundle 8-bits together, this is known as a byte. One byte can therefore have
256 unique states. An unsigned 8-bit integer, stored in a byte, can therefore
represent any integer ranging from 0 to 255. This range may seem familiar to
people who have used colour pickers to specify RGB values.


Character encodings
-------------------

If a computer only works in terms of zeros and ones how can one use it to read
and write text? This is the subject of character encodings.

One of the most prevalent character encodings is ASCII (American Standard Code
for Information Interchange), which was first published in 1963. It has its roots in
telegraphic codes. Based on the English alphabet it encodes 128 characters
into 7-bit integers. These characters include: 0-9, a-z, A-Z, the space character,
punctuations symbols and control codes for operating Teletype machines.

.. figure:: images/ASCII_Code_Chart-Quick_ref_card.png
   :alt: ASII code chart quick reference card.

   ASCII Code Chart, scanner copied from the material delivered with TermiNet
   300 impact type printer with Keyboard, February 1972, General Electric Data
   communication Product Dept., Waynesboro VA. By Namazu-tron [Public domain],
   `via Wikimedia Commons
   <https://commons.wikimedia.org/wiki/File%3AASCII_Code_Chart-Quick_ref_card.png>`_.

Although many of the control characters are now obsolete some of them still form
the basis of how we interpret text in computers. For example the "line feed" character,
which originally moved the paper in the printer forward, is now used to
represent new lines in text documents.

In going from Teletype machines to computers different operating systems had
different interpretations on how new lines should be represented. One surviving
convention, still in use in Windows, requires both the "line feed" and the
"carriage return" characters. The former representing the advancement of paper
in the printer and the latter the repositioning of the print-head. Another
convention, used in Unix-like systems, is to simply use the line feed
character to represent new lines. This can result in compatibility issues when
opening Unix-like text files in some Windows programs. For example when
open a multi-line file with Unix-based line endings in Notepad the text appears
like one long line.

Many other character encodings are based on ASCII. A famous example is
ISO-8859-1 (Latin 1), which uses an eighth bit to expand the ASCII character set
to include the remaining characters from the Latin script.

These days the emergent standard is Unicode, which provides a standard encoding
for dealing with most of the worlds writing systems. Unicode is backwards compatible
with ASCII and ISO-8859-1 in that Unicode's code points, the natural numbers used to
assign unique characters, have the same values as those of the ASCII and
ISO-8859-1 encodings.

.. sidebar:: Natural number

   A natural number is a whole non-negative number. In computing such a number
   is commonly referred to as an unsigned integer.


Real numbers
------------

So we have learnt that integers can be represented in computers with zeros and
ones by making use of the binary numeral system. Let us now discuss how real
numbers can be represented and some issues that one can come across when working
with these.

.. sidebar:: Real numbers

   Real numbers include rational numbers such as integers and fractions as well
   as irrational numbers such as the :math:`\sqrt{2}` and :math:`\pi`.

One way of storing and working with real numbers is to use the fixed-point number
representation. A fixed-point number is basically an integer that is scaled by an
implicit factor. So if the implicit scaling factor was :math:`1/10` and the
explicit integer stored in the computer was :math:`127` then the real number
represented would be :math:`12.7`.

However, the fixed-point number representation has the disadvantage that the
range of numbers that can be represented is relatively small.  The
floating-point number was invented to work around this limitation.

Floating-point numbers basically allow the decimal (radix) point to float.
This means that numbers of differing orders of magnitude can be expressed using
the same units. It is very much similar to scientific notation where the distance
to the moon can be expressed as :math:`3.844 * 10^8` and the size of a typical
bacterium can be expressed as :math:`1.0 * 10^{-6}`. A consequence of this is that
the numbers that can be expressed are not uniformly spaced, i.e. as the size of
the exponent increases the step size between two representable numbers increases.

All real numbers cannot be represented precisely using floating-point numbers.
Furthermore, arithmetic operations on floating-point numbers cannot truly
represent arithmetic operations. This can lead to issues with accuracy. We can
illustrate this using Python (we will get more details on scripting and Python
in the :doc:`data-munching` chapter).

.. code-block:: python

    >>> 0.6 / 0.2
    2.9999999999999996


Boolean logic
-------------

Boolean logic is a mathematical formalism for describing logical relations.
In Boolean logic things are either ``True`` or ``False``. These truth values
are often represented as 1 and 0 respectively.
There are three basic operators ``AND``, ``OR`` and ``NOT`` for working with
truth values. These are sometimes referred to as logic gates.

=====  =====  ===========  ==========
``x``  ``y``  ``x AND y``  ``x OR y``
=====  =====  ===========  ==========
  0      0         0           0
  1      0         0           1
  0      1         0           1
  1      1         1           0
=====  =====  ===========  ==========

=====  =========
``x``  ``NOT x`` 
=====  =========
  0        1  
  1        0  
=====  =========

Using these axioms more complex logic gates can be built up. For example, by
combining ``NOT`` and ``AND`` one can create what is commonly referred to as
a ``NAND`` gate.

=====  =====  ===========  =================
``x``  ``y``  ``x AND y``  ``NOT (x AND y)``
=====  =====  ===========  =================
  0      0         0           1
  1      0         0           1
  0      1         0           1
  1      1         1           0
=====  =====  ===========  =================

Importantly one can use Boolean logic gates to implement integer arithmetic
and memory. This combined with the fact that it is relatively easy to
physically implement boolean logic using relays led to the construction of
the first computers.

Although you may not want to create your own computer having a basic
understanding of Boolean logic will help you when implementing algorithms. For
example one often need to make conditional logic statements along the lines of
"``IF`` the cell contains green fluorescent protein ``AND`` it is illuminated
with light of wavelength 395 nm ``THEN`` it will emit fluorescence at 509 nm".

.. note:: Boolean logic is also used in Boolean networks, a formalism that
          can be used to describe gene regulatory networks.



The microprocessor
------------------

A microprocessor executes machine instructions. Machine instructions tell the
microprocessor what to do.  At the most basic level there are three things that
a microprocessor can do: perform mathematical operations, move data from one
memory location to another, make decisions and jump to new sets of instructions
based on those decisions.

.. sidebar:: The C programming language

   C is a popular programming language designed by Dennis Ritchie in 1972.
   It is a low-level language, which means that it allows the programmer to
   work close to the hardware by providing direct access to the systems memory.
   One of the most famous C projects is the Linux kernel, which is a massive open
   source project with millions of lines of code and thousands of contributors.

Most programming languages provide some sort of abstraction layer so that the
programmer does not need to think in terms of machine instructions. For example,
the main purpose of a C compiler is to convert C source code into machine
instructions.

When working with higher level languages, such as Python, one does not really need
to worry about what happens at the microprocessor level.

However, knowing that a microprocessor can make decisions and jump to new sets
of instructions can be useful when trying to understand concepts such as
loops. A loop is essentially a set of machine instructions that end with a
decision to jump back to the beginning of the same set of instructions.

Loops often include a criteria for exiting the loop. If the criteria for
exiting the loop is not defined, or it cannot be reached, the loop will keep
cycling forever in what is termed an "infinite loop".

.. code-block:: C
   :caption: Basic C program illustrating a while loop. The loop terminates
             when the integer ``i`` is no longer less than 3.

   int main () {
      int i = 0;
      while( i < 3 ) {
         i = i + 1;
      }
      return 0;
   }


Computer memory
---------------

Computer memory comes in different forms with different characteristics. The
hard drive of a computer is a type of memory where data can be stored
permanently. RAM (Random Access Memory) is a type of memory where data is
volatile, i.e. it is not retained when the machine reboots. A less well known
type of memory is the registry, which resides in the CPU (Central Processing
Unit). Being physically close to the CPU means that reading and writing of data
to the registry is very fast. Other important characteristics of computer
memory include the its size and cost. Below is a table summarising these
characteristics.

=========  =========  ==============  ==============  ========
Location   Speed      Size            Cost            Volatile
=========  =========  ==============  ==============  ========
Registry   Very fast  Very small      Very expensive  Yes
RAM        Fast       Small/Moderate  Expensive       Yes
Hard disk  Slow       Very large      Cheap           No
=========  =========  ==============  ==============  ========

If one is working with really large data sets the main bottleneck in the
processing pipeline can be reading data from and writing data to memory.
This is known as being IO (input/output) bound.


Key concepts
------------

- A bit is the smallest piece of data that can be stored in a computer, it can
  be set to either zero or one
- A byte is 8-bits
- An unsigned 8-bit integer can represent any of the integers between 0 and 255
- A character encoding maps characters to integers, common character encodings
  include ASCII and Unicode
- Real numbers tend to be handled using floating-point representation
- There are some inherent limitations when working with floating-point numbers
  which can lead to issues with accuracy
- Boolean logic is one of the main tools employed by the computer to do work
  and to store data
- A microprocessor executes machine instructions
- Machine instructions can tell the microprocessor to perform mathematical
  operations, move data around and to make decisions to jump to new sets of
  machine instructions
- The hard disk, RAM and the register are different types of memory with
  different characteristics
- Processes that spend most of their time reading and writing data are said to
  be IO bound
