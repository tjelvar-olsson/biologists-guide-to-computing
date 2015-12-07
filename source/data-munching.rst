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

For example, suppose that one felt unsure about how to go about extracting the
species information from the FASTA description line. This would represent a
good place to start experimenting with some actual code. Can we come up with a
code snippet that can extract the relevant information from a string?


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
    :emphasize-lines: 6,7

    """Module containing utility functions for working with FASTA files."""

    def test_is_description_line():
        """Test the is_description_line() function."""
        print("Testing the is_description_line() function...")
        assert is_description_line(">This is a description line") is True

    test_is_description_line()

Explain code...

Explain that the code will fail and why this is radical and great...

Show what happens when the code is run...

Make function return True...

Add...

.. code-block:: python

    """Module containing utility functions for working with FASTA files."""

    def test_is_description_line():
        """Test the is_description_line() function."""
        print("Testing the is_description_line() function...")
        assert is_description_line(">This is a description line") is True
        assert is_description_line("ACTG") is False


test_is_description_line()

Implement real functionality...

Explain the value of TDD...
