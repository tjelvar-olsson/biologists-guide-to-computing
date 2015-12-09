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
