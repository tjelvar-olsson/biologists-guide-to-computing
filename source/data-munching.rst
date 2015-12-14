Data munching
=============


Introduction
------------

At this stage of the project it would be useful to get a better understanding
of the content of the FASTA file. For example how many different species does
it contain?

By briefly inspecting some random FASTA description lines it becomes clear that
some species include variants. Below are some sample *R. coli* extracts from
the FASTA file.

.. code-block:: none

    ...IraP OS=Escherichia coli (strain SE11) GN=iraP PE=3 SV=1
    ...IraP OS=Escherichia coli (strain SMS-3-5 / SECEC) GN=iraP PE=3 SV=1
    ...IraP OS=Escherichia coli (strain UTI89 / UPEC) GN=iraP PE=3 SV=1

How many variants are there per species? Do all variants contain the same
number of proteins?

To answer these questions we will make use of the Python scripting language.
Python is a high-level scripting language that is growing in popularity in the
scientific community. It uses a syntax that is relatively easy to get to grips
with and which encourages code readability.


Stop and think
--------------

One of the most common mistakes when programming is to start coding before the
problem is one is trying to solve is clearly defined.

What do we want to achieve? Pause for a second and try to answer this question
to yourself. It may help trying to explain it out loud.

Now describe all the steps required to achieve this goal. Again it may help
speaking out loud to yourself or imagining that you are describing the required
steps to a colleague.

Did you find that easy or difficult? Thinking about what one wants to achieve
and all the steps required to get there can often be a bit overwhelming. At
this stage it is easy to think to oneself that the detail will become clear
later on and that one should stop wasting time and get on with the coding. Try
to avoid doing this. Rather, try to think of a simpler goal. 

For example take the simplest goal described so far: working out how many
different species are in the FASTA file. This goal would require us to:

1. Identify all the description lines in the FASTA file
2. Extract the species information from the line
3. Ensure that there are no duplicate entries
4. Count the number of entries

If you know the tools and techniques required to do all of the steps above this
may not seem overwhelming, but if you don't it may. In either case it is
worthwhile treating each step as a goal in itself and think about what would be
required to achieve each individual step. By iterating over this process you
will either get down to steps sizes that do not feel overwhelming or you will
identify the key aspect that you feel unsure about.

For example, suppose that one felt unsure about how to go about identifying all
the description lines in the FASTA file we could decompose this step even further.

1. Create a list for storing the description lines
2. Iterate over all the lines in the input file
3. For each line check if it is a description line
4. If it is add the line to the list

Now suppose one felt unsure about how to check if a line was a description
line. This would represent a good place to start experimenting with some actual
code. Can we come up with a code snippet that can check if a string is a FASTA
description line?


Measuring success
-----------------

Up until this point we have been testing our code snippets by running the code
and verifying the results manually. This is terribly inefficient and soon breaks
down once you have more than one thing to test.

It is much better to write tests that can be run automatically.  Let's examine
this concept. Create a file named ``scripts/fasta_utils.py`` and add the Python
code below to it.

.. code-block:: python
    :linenos:

    """Module containing utility functions for working with FASTA files."""

    def test_is_description_line():
        """Test the is_description_line() function."""
        print("Testing the is_description_line() function...")

    test_is_description_line()

This is our first piece of Python code so let us explain it in detail.

The first line is a module level "docstring" and it is used to explain the
intent of the module as a whole.  A "module" is basically a file with a ``.py``
extension, i.e. a Python file. Modules are used to group related functionality
together.

Lines three to five represent a function named :func:`test_is_description_line`.
Python functions consist of two parts: the function definition (line 3) and the
function body (lines 4 and 5). 

In Python functions are defined using the ``def`` keyword. Note that the
``def`` keyword is followed by the name of the function. The name of the
function is followed by a parenthesized set of arguments, in this case the
function takes no arguments. The end of the function definition is marked using
a colon.

The body of the function, lines four and five, need to be indented. The standard
in Python is to use four white spaces to indent code blocks. The first line of the
function body, line four, is a docstring explaining the intent of the function.
Line five makes use of the built-in ``print()`` function to write a string to the
standard out stream. Python's built-in ``print()`` function is similar to the
``echo`` command used in :doc:`keeping-track-of-your-work`.

Finally, on line seven the :func:`test_is_description_line` function is called,
i.e. the logic within the function's body is executed. In this instance this
means that the ``"Testing the is_description_line() function..."`` string is
written to the standard output stream.

Let us try out this code in a terminal.

.. code-block:: none

    $ python scripts/fasta_utils.py
    Testing the is_description_line() function...

So far so good? At the moment our :func:`test_is_description_line` function
does not actually test anything. Let us rectify that now.

.. code-block:: python
    :linenos:
    :emphasize-lines: 6

    """Module containing utility functions for working with FASTA files."""

    def test_is_description_line():
        """Test the is_description_line() function."""
        print("Testing the is_description_line() function...")
        assert is_description_line(">This is a description line") is True

    test_is_description_line()

There are quite a few things going on in the newly added line. First of all it
makes use of three built-in features of Python: the ``assert`` and ``is``
keywords, as well as the ``True`` constant. Let's work through these in reverse
order.

Python has some `built-in constants
<https://docs.python.org/2/library/constants.html>`_, most notably ``True``,
``False`` and ``None``. The ``True`` and ``False`` constants are the only
instances of the ``bool`` (boolean) type and ``None`` is often used to
represent the absence of a value.

In Python ``is`` is an operator that checks for object identity, i.e. if the
object returned by the :func:`is_description_line` function and ``True`` are
the same object. If they are the same object the comparison evaluates to
``True`` if not it evaluates to ``False``.

The ``assert`` keyword is used to insert debugging statements into a program.
It provides a means to ensure that the state of a program is as expected. If
the statement being evaluated, in this case ``is_description_line(">This is a
description line") is True`` evaluates to ``False`` an ``AssertionError`` is
raised.

So, what will happen if we run the code in its current form?
Well, we have not yet defined the :func:`is_description_line` function, so
Python will raise a ``NameError``. Let us run the code.

.. code-block:: none

    $ python scripts/fasta_utils.py
    Testing the is_description_line() function...
    Traceback (most recent call last):
      File "scripts/fasta_utils.py", line 8, in <module>
        test_is_description_line()
      File "scripts/fasta_utils.py", line 6, in test_is_description_line
        assert is_description_line(">This is a description line") is True
    NameError: global name 'is_description_line' is not defined

Great now we are getting somewhere! What? Well, we have impemented some
code to test the functionality of the :func:`is_description_line` and it
tells us that the function does not exist. This is useful information.
Let us add a placeholder :func:`is_description_line` function to the Python
module.

.. code-block:: python
    :linenos:
    :emphasize-lines: 3,4

    """Module containing utility functions for working with FASTA files."""

    def is_description_line(line):
        """Return True if the line is a FASTA description line."""

    def test_is_description_line():
        """Test the is_description_line() function."""
        print("Testing the is_description_line() function...")
        assert is_description_line(">This is a description line") is True

    test_is_description_line()

Note that the function we have added on lines three and four currently does nothing.
However, when we run the script we should no longer get a ``NameError``. Let's find
out what happens when we run the code.

.. code-block:: none

    $ python scripts/fasta_utils.py
    Testing the is_description_line() function...
    Traceback (most recent call last):
      File "scripts/fasta_utils.py", line 11, in <module>
        test_is_description_line()
      File "scripts/fasta_utils.py", line 9, in test_is_description_line
        assert is_description_line(">This is a description line") is True
    AssertionError

More progress! Now we see the expected ``AssertionError``. Let us add some code to try
to get rid of this error message. To achieve this we simply need to make the script
return ``True``.

.. code-block:: python
    :linenos:
    :emphasize-lines: 5

    """Module containing utility functions for working with FASTA files."""

    def is_description_line(line):
        """Return True if the line is a FASTA description line."""
        return True

    def test_is_description_line():
        """Test the is_description_line() function."""
        print("Testing the is_description_line() function...")
        assert is_description_line(">This is a description line") is True

    test_is_description_line()

Now, we can run the code again.

.. code-block:: none

    $ python scripts/fasta_utils.py
    Testing the is_description_line() function...

No error message, the code is now working to the specification described in the test.
However, the test does not specify what the behaviour should be for a biological
sequence line. Let us add another assert statement to specify this.

.. code-block:: python
    :linenos:
    :emphasize-lines: 11

    """Module containing utility functions for working with FASTA files."""

    def is_description_line(line):
        """Return True if the line is a FASTA description line."""
        return True

    def test_is_description_line():
        """Test the is_description_line() function."""
        print("Testing the is_description_line() function...")
        assert is_description_line(">This is a description line") is True
        assert is_description_line("ATCG") is False

    test_is_description_line()

Now we can run the code again.

.. code-block:: none

    $ python scripts/fasta_utils.py
    Testing the is_description_line() function...
    Traceback (most recent call last):
      File "scripts/fasta_utils.py", line 13, in <module>
        test_is_description_line()
      File "scripts/fasta_utils.py", line 11, in test_is_description_line
        assert is_description_line("ATCG") is False
    AssertionError

More progress, we now have a test to ensure that the :func:`is_description_line` function
returns ``False`` when the input line is a sequence. Let us try to implement the desired
functionality to make the test pass. For this we will use the
`startswith() <https://docs.python.org/2/library/stdtypes.html#str.startswith>`_ method,
that is built into strings, to check if the string starts with a ">" character.

.. code-block:: python
    :linenos:
    :emphasize-lines: 5-8

    """Module containing utility functions for working with FASTA files."""

    def is_description_line(line):
        """Return True if the line is a FASTA description line."""
        if line.startswith(">"):
            return True
        else:
            return False

    def test_is_description_line():
        """Test the is_description_line() function."""
        print("Testing the is_description_line() function...")
        assert is_description_line(">This is a description line") is True
        assert is_description_line("ATCG") is False

    test_is_description_line()

In the code above we make use of conditional logic, i.e. ``if`` something is
``True`` do something otherwise do something ``else``. As mentioned previously
whitespace is important in Python and four are spaces used to indent the lines after
the ``if`` and ``else`` statements to tell Python which statement(s) belong in the conditional
code blocks. In this case we only have one statement per conditional, but it is
possible to group several statements together based on their indentation.

Let us test the code again.

.. code-block:: none

    $ python scripts/fasta_utils.py
    Testing the is_description_line() function...


Fantastic the code behaves in the way that we want it to behave!

However, the current implementation of the :func:`is_description_line` function
is a little bit verbose. Do we really need the ``else`` conditional?  What
would happen if it was not there and the line started with a ">"? The program
would enter the ``if`` conditional statement and return ``True``. When the
function returns a value to program jumps out of the function so the next
``return`` statement would never be reached.

The beauty of tests now become more apparent. We can start experimenting with
the implementation of a function and feel confident that we are not breaking
existing functionality. As long as the tests do not fail that is!

Let us test out our hypothesis that the ``else`` conditional is redundant by
removing it and de-denting the ``return False`` statement.

.. code-block:: python
    :linenos:
    :emphasize-lines: 7

    """Module containing utility functions for working with FASTA files."""

    def is_description_line(line):
        """Return True if the line is a FASTA description line."""
        if line.startswith(">"):
            return True
        return False

    def test_is_description_line():
        """Test the is_description_line() function."""
        print("Testing the is_description_line() function...")
        assert is_description_line(">This is a description line") is True
        assert is_description_line("ATCG") is False

    test_is_description_line()

Now we can simply run the tests to see what happens.

.. code-block:: none

    $ python scripts/fasta_utils.py
    Testing the is_description_line() function...

Amazing, we just made a change to our code and can feel pretty sure that it is
still working as intended. This is very powerful.

The methodology used in this section is known as Test-Driven Development, often
referred to as TDD. It involves three steps:

1. Write a test
2. Write minimal code to make the test pass
3. Refactor the code if necessary

In this instance we started off by writing a test checking that the
:func:`is_description_line` function returned ``True`` when the input was a
description line.  We then added *minimal* code to make the test pass, i.e.  we
simply made the function return ``True``. At this point no refactoring was
needed so we added another test to check that the function returned ``False``
when the input was a sequence line. We then added some naive code to make the
tests pass.  At this point, we believed that there was scope to improve the
implementation of the function, so we refactored it to remove the redundant
``else`` statement.

Well done! That was a lot of information. Go make yourself a cup of tea.


More string processing
----------------------

Because both DNA and proteins can be represented as strings of characters many
aspects of biological data processing involve string manipulations. This
section will therefore provide a brief summary of how Python can be used for
such string processing.

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


The Python string object
^^^^^^^^^^^^^^^^^^^^^^^^

Now that we know how how to make use of Python in interactive mode let us go
through some common string processing scenarios.

When parsing in strings from a text file one often has to deal with lines that
have leading and/or trailing one spaces. Commonly one wants to get rid of them.
This can be achieved using the ``strip()`` method built into the string object.

.. code-block:: python

    >>> "  text with leading/trailing spaces ".strip()
    'text with leading/trailing spaces'

Another common use case is to want to replace a word in a line. For example,
when we strip out the leading and trailing white spaces one might want to
update the word ``with`` to ``without`` to make the resulting string reflect
its current state. This can be achieved using the ``replace()`` method.

.. code-block:: python

    >>> "  text with leading/trailing spaces ".strip().replace("with", "without")
    'text without leading/trailing spaces'

.. note:: In the example above we chain the ``strip()`` and ``replace()`` methods
          together. In practise this means that the ``replace()`` methods acts
          on the return value of the ``strip()`` method.

.. sidebar:: What is the difference between a function and a method?

             Often the two terms are used interchangeably. However, a method refers
             to a function that is part of a class and the term function refers to
             a function which stands on its own.

Earlier we saw how the ``startswith()`` method can be used to identify FASTA
description lines.

.. code-block:: python

    >>> ">MySeq1|description line".startswith(">")
    True

The ``endswith()`` method complements the ``startswith()`` method and is often
used to examine file extensions.

.. code-block:: python

    >>> "/home/olsson/images/profile.png".endswith("png")
    True

This example above only works if the file extension is in lower case.

.. code-block:: python

    >>> "/home/olsson/images/profile.PNG".endswith("png")
    False

However, we can overcome this issue by adding a call to the ``lower()`` method,
which converts the string to lower case.

.. code-block:: python

    >>> "/home/olsson/images/profile.PNG".lower().endswith("png")
    True

Another common use case is to want to search for a particular string within
another string. For example one might want to find out if the UniProt
identifier "Q6GZX4" is present in a FASTA description line. To achieve this one
can use the ``find()`` method, which returns the index position (zero-based)
where the search term was first identified.

.. code-block:: python

    >>> ">sp|Q6GZX4|001R_FRG3G".find("Q6GZX4")
    4

If the search term is not identified ``find()`` returns -1.

.. code-block:: python

    >>> ">sp|P31946|1433B_HUMAN".find("Q6GZX4")
    -1

When iterating over lines in a file one often want to split the line based on a
delimiter. This can be achieved using the ``split()`` method. By default this
splits on white space characters and returns a list of strings.

.. code-block:: python

    >>> "text without leading/trailing spaces".split()
    ['text', 'without', 'leading/trailing', 'spaces']

A different delimiter can be used by providing it as an argument to the ``split()``
method.

.. code-block:: python

    >>> ">sp|Q6GZX4|001R_FRG3G".split("|")
    ['>sp', 'Q6GZX4', '001R_FRG3G']

There are many variations on the string operators described above. It is useful
to familiarise yourself with the
`Python documentation on strings <https://docs.python.org/2/library/string.html>`_.


Regular expressions
^^^^^^^^^^^^^^^^^^^

Regular expressions can be defined as a series of characters that define a
search pattern.

Regular expressions can be very powerful. However, they can be
difficult to build up. Often it is a process of trial and error. This means
that once they have been created, and the trial and error process has been
forgotten, it can be extremely difficult to understand what the regular
expression does and why it is constructed the way it is.

.. warning:: Use regular expression as a last resort. A good rule of thumb is
             to always try to use regular string operations to implement the
             desired functionality and only switch to regular expressions when
             the code implemented using regular string operations becomes more
             difficult to understand than the equivalent regular expression.

To use regular expressions in Python we need to import the :mod:`re` module.
The :mod:`re` module is part of Python's standard library. Importing modules
in Python is achieved using the ``import`` keyword.

.. sidebar:: What is a standard library?

             In computing a standard library refers to a set of functionality
             that comes built-in with the core programming language.

.. code-block:: python

    >>> import re

Let us store a FASTA description line in a variable.

.. code-block:: python

    >>> fasta_desc = ">sp|Q6GZX4|001R_FRG3G"

Now, let us search for the UniProt identifer ``Q6GZX4`` within the line.

.. code-block:: python

    >>> re.search(r"Q6GZX4", fasta_desc)  # doctest: +ELLIPSIS
    <_sre.SRE_Match object at 0x...>

There are two things to note here:

1. We use a raw string to represent our regular expression
2. The regular expression ``search()`` method returns a match object (or None if no match is found)

.. sidebar:: What is a "raw string"?

    In Python "raw" strings differ from regular strings in that the bashslash
    ``\`` character is interpreted literally. For example the regular string
    equivalent of ``r"\n"`` would be ''"\\n" where the first backslash is used
    to escape the effect of the second (remember that ``\n`` represents a
    newline).

    Raw strings where introduced in Python to make it easier to create regular
    expressions that rely heavily on the use of literal backslashes.

The index of the first and last matched characters can be accessed as using the
match object's ``start()`` and ``end()`` methods.

.. code-block:: python

    >>> match = re.search(r"Q6GZX4", fasta_desc)
    >>> if match:
    ...     print(fasta_desc[match.start():match.end()])
    ...
    Q6GZX4

In the above we make use of the fact that Python strings support slicing.
The ``[start:end]`` syntax used for slicing strings, and lists, is inclusive
for the start index and exclusive for the end index.

.. code-block:: python

    >>> "012345"[2:4]
    '23'

To see the value of regular expressions we need to create one that can do
something more than an exact match. For example a regular expression that can
could match all the patterns ``id0``, ``id1``, ..., ``id9``.

Now suppose that we had a list containing FASTA description lines with these
types of identifiers.

.. code-block:: python

    >>> fasta_desc_list = [">id0 match this",
    ...                    ">id9 and this",
    ...                    ">id100 but not this (initially)",
    ...                    "AATCG"]
    ...

Let us loop over the items in this list and print out the lines that match our
identifier regular expression.

.. code-block:: python

    >>> for line in fasta_desc_list:
    ...     if re.search(r">id[0-9]\s", line):
    ...         print(line)
    ...
    >id0 match this
    >id9 and this

There are several things to note in the above. First of all we are using the
concept of a ``for`` loop to iterate over all the items in the
``fasta_desc_list``. Secondly, there are two noteworthy aspects of the regular
expression. The ``[0-9]`` syntax means match any digit. The ``\s`` regular
expression meta character means match any white space character. 

.. sidebar:: The ``[0-9]`` syntax works in Bash too!

             For example list the files ``photo_0.png``, ``photo_1.png``,
             ..., ``photo_9.png`` you could use the command.

             .. code-block:: none

                ls photo_[0-9].png

If one wanted to create a regular expression to match an identifier with an
arbitrary number of digits one can make use of the ``*`` meta character, which
causes the regular expression to match the preceding expression 0 or more times.

.. code-block:: python

    >>> for line in fasta_desc_list:
    ...     if re.search(r">id[0-9]*\s", line):
    ...         print(line)
    ...
    >id0 match this
    >id9 and this
    >id100 but not this (initially)

It is possible to extract specific pieces of information from a line using
regular expressions. This uses a concept known as "groups", which are indicated
using parenthesis. Let us try to extract the UniProt identifier from a FASTA
description line.

.. code-block:: python

    >>> print(fasta_desc)
    >sp|Q6GZX4|001R_FRG3G
    >>> match = re.search(r">sp\|([A-Z,0-9]*)\|", fasta_desc)

.. warning:: Note how horrible and incomprehensible the regular expression is.

It took me a couple of attempts to get this regular expression right as I
forgot that ``|`` is a regular expression meta character that needs to be
escaped using a backslash ``\``. Note the regular expression representing the
UniProt idendifier ``[A-Z,0-9]*`` is enclosed in parenthesis. The parenthesis
denote that the UniProt identifier is a group that we would like access to.


    >>> match.groups()
    ('Q6GZX4',)
    >>> match.group(0)
    '>sp|Q6GZX4|'
    >>> match.group(1)
    'Q6GZX4'

Finally Let us have a look at a common pitfall when using regular expressions
in Python: the difference between the methods search() and match().

.. code-block:: python

    >>> print(re.search(r"cat", "my cat has a hat"))  # doctest: +ELLIPSIS
    <_sre.SRE_Match object at 0x...>
    >>> print(re.match(r"cat", "my cat has a hat"))  # doctest: +ELLIPSIS
    None

Basically ``match()`` only looks for a match at the beginning of the string to
be searched. For more information see the
`search() vs match() <https://docs.python.org/2/library/re.html#search-vs-match>`_
section in the Python documentation.

There is a lot more to regular expressions in particular all the meta
characters. For more information have a look at the
`regular expressions operations <https://docs.python.org/2/library/re.html>`_
section in the Python documentation.



Extracting species information
------------------------------

Add test and functionality for extracting species.


Only running tests when the module is called directly
-----------------------------------------------------


Counting the number of species
------------------------------


