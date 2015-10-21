How to think like a computer
============================

These days computers can perform complex task. However, at their core computers
only have a relatively small set of basic capabilities. Information is stored as
zeros and ones and Boolean logic is used to perform calculations.

Binary, bits and bytes
----------------------

.. sidebar:: Integer

   An integer is a whole number, i.e. a number that can be expressed without a fractional component.

Suppose that we wanted to represent a positive integer using zeros and ones. We can
achieve this using using the base-2 numeral systems, a.k.a. binary. Take for example
the number 208. Other ways to represent this number include:

.. math::

    208 &= (2*100) + (0 * 10) + (8 * 1) \\
        &= (2 * 10^2) + (0 * 10^1) + (8 * 10^0)

The equations above illustrate the basis of the base-10 numeral system. Note that
the base of the exponent in each term is ten.  To express an integer in binary
we need to formulate it so that the base of each exponent is 2.

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

If a computer only works in terms of zeros and ones how can we use it to read
and write text? Basically this comes down to the subject of character encodings.

One of the most prevalent character encodings is ASCII (American Standard Code for
Information Interchange).

- Roots in telegraphic codes
- First published in 1963
- Based on English alphabet
- Encodes 128 characters into 7-bit integers
- 0-9, a-z, A-Z, punctuation symbols, control codes originating from Teletype machines and a space character
- The control characters define how text and space are processed
- Some of the control characters are now obsolete
- Line feed vs carriage return
- How this now manifests itself in different operating systems
- 8-bit
- ISO-8859-1 (Latin 1)
- UTF
- ASCII is 7-bit encoding for small subset of Unicode/UCS and ASCII when preceded with a zero (to make it 8-bit) is valid UTF-8

.. figure:: https://upload.wikimedia.org/wikipedia/commons/e/e0/ASCII_Code_Chart-Quick_ref_card.png
   :alt: ASII code chart quick reference card.

   ASCII Code Chart, scanner copied from the material delivered with TermiNet
   300 impact type printer with Keyboard, February 1972, General Electric Data
   communication Product Dept., Waynesboro VA. By Namazu-tron [Public domain],
   `via Wikimedia Commons
   <https://commons.wikimedia.org/wiki/File%3AASCII_Code_Chart-Quick_ref_card.png>`_.


Floating point numbers
----------------------

- How real numbers can be represented using binary
- Describe the problem with using a fixed point
- Floating point numbers and their implementation
- The numbers that can be represented are not evenly spaced
- Rounding errors

Boolean logic
-------------

- AND, OR, NOT
- use in arithmetic
- use in memory
- use in conditional logic and for algorithms to decide what to do
- can be implemented using relays


The microprocessor
------------------

At the most basic level a microprocessor executes machine instructions.
The machine instructions can tell the microprocessor to do three things:

- Perform mathematical operations
- Move data from one location to another
- Make decisions and jump to a new set of instructions based on those decisions

- Describe relevance to algorithms, loops, etc


Different types of memory
-------------------------

- IO can be a real issue
