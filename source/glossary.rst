Glossary
========

.. glossary::

    Bit
        The smallest unit of informaiton, abbreviation of the term Binary digIT.

    Blob
        Binary Large OBject, typically a large binary file stored in a database.

    Bug
        Defect in a computer program which stops it from working as intended,
        see also :term:`debug`.

    Class
        In `object-oriented programming` a class is a template that can be used to create
        an :term:`object`.

    CPU
        Central Processing Unit, part of the computer that executes machine instructions.

    Debug
        The activity of identifying and removing a :term:`bug` (defect) from a
        computer program. 

    DNS
        Domain Name Server a machine that translates URLs to IP addresses.

    DRY
        Don't repeat yourself. Make use of a variables for frequently used values. Make
        use of functions for frequently used processing logic.

    Function
        A set of instructions for performing a procedure. Most commonly a function takes a
        set of arguments and returns a value.

    HTTP
        HyperText Transfer Protocol, commonly used for web browsing.

    List
        A common data structure representing an ordered collection of elements. Also known
        as an array.

    Method
        A method refers to a :term:`function` that is part of a :term:`class`.

    Init
        Abbreviation of the word "initialise". Usually used to represent a one time
        event that that results in the creation of a new entity. 

    Integer
        A whole number, i.e. a number that can be expressed without a fractional component.

    I/O
        Input/Output; often used in the context of reading and writing data to
        memory.   

    Object
        In :term:`object-oriented programming` an object is an instance of a :term:`class`.

    Object-oriented programming
        Programming methodology centered around the concept of objects rather than actions
        and procedures.

    IP address
        Internet Protocol address.

    Pipe
        A pipe is a means to redirect the output from one command into another.
        The character used to represent a pipe is the vertical bar: ``|``.

    RAM
        Random Access Memory, fasta access volitile memory.

    Recursion
        A procedure whose implementaiton calls itself. Of practical use for problems where
        the solution depends on smaller instances of the same problem.

    Scope
        Determines which parts of a program has access to what. For example any part of a
        program can have access to a global variable. However, if a variable is contained
        within the scope of a function only code within that function can access the variable.

    Shell
        Program that allows the user to interact with the operating system's services and
        programs, see also :term:`terminal`.

    Standard input stream
        Stream of characters ingested by a program, often abbreviated as
        ``stdin``. A common way to send data to a program is to :term:`pipe`
        them from the :term:`standard output stream` of another program. In the
        below we use the ``echo`` command to send the string ``foo bar baz`` to
        the standard input stream of the ``wc`` command. 

        .. code-block:: none

            $ echo "foo bar baz" | wc
                   1       3      12

    Standard error stream
        Stream of characters, representing error output, emitted by a program.
        Commonly viewed in the shell when running a command.  Often abbreviated
        as ``stderr``.

    Standard Library
        A set of functionality that comes built-in with the core programming language.

    Standard output stream
        Stream of characters emitted by a program. Commonly viewed in the shell when
        running a command. The standard output stream can be redirected using a :term:`pipe`.
        Often abbreviated as ``stdout``.

    State
        All the information, to which a program has access, at a particular point in time.


    String
        A list of characters used to represent text.

    TDD
        See :term:`test-driven development`

    Terminal
        Application for accessing a shell, see also :term:`shell`.

    Test-driven development
        Methodology used in software development that makes use of rapid iterations of development
        cycles. The develoment cycle includes three steps:

            1. Write a test
            2. Write minimal code to make the test pass
            3. Refactor the code if necessary

    URL
        Unique Resource Location
