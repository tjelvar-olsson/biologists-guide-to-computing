Structuring and storing data
============================

Data structures
---------------

In :doc:`how-to-think-like-a-computer` we introduced some basic data types,
including integers, floating point numbers, characters and strings. However,
most of the time we are not interested in individual instances of integers or
floating point values, we want to analyse lots of data points, often of a
heterogeneous nature.

We need some way of structuring our data. In computing there are several well
defined data structures for accomplishing this. Three data structures of particular
interest are lists, dictionaries and graphs. Lists and dictionaries are generally
useful and graphs are of particular interest in biology.

A list, also known as an array, is basically a collection of elements in a
specific order. The most common use case is to simply go through all the
elements in a list and do something with them. For example if you had a stack of
CVs on your desk you could go through them and depending on whether or not the
font was too small you could through them in the bin or not. This is similar to
what we did with the FASTA file in :doc:`first-steps-towards-automation`, if a
line did not contain the expression "OS=Homo sapiens" it was ignored.

Because lists have an order associated with them they can also be used to
implement different types of queuing behaviour such as "first-in-first-out" and
"first-in-last-out". Furthermore, individual elements can usually be accessed
through their numerical indices.

A dictionary, also known as a map or an associative array, is an unordered
collection of elements that can be accessed through their associated
identifiers. In other words each entry in a dictionary has a name, the
identifier, and a value. For example, suppose that you needed to store
information about the flowering time for different species of flowers. In this
case it would make a lot of sense to have a system where you could look up the
flowering time information based on the species name.

It is worth noting that it is possible to create nested data structures. For
example, think of a spreadsheet. The spreadsheet could be represented as a list
containing lists. In other words the elements of the outer list would represent
the rows in the spreadsheet and the elements in an inner list would represent
values of cells at different columns for a specific row.

A graph, also known as a tree, is a data structure that links nodes together
via edges. This should sound relatively familiar to you as it is the basic
concept behind phylogenetic trees. Each node represents a species and the edges
represent the inferred evolutionary relationships between the species.

Because of their general utility lists and dictionaries are built-in to many
high level programming languages such as Python and JavaScript. However, data
structures for graphs are generally not.


Data persistence
----------------

Suppose that your program has generated a phylogenetic tree and it has used
this tree to determine that scientists and baboons are more closely related
than expected. Result! At this point the program is about to finish. What
should happen to the phylogenetic tree? Should it be thrown away or will you
still need access to it even though you have got your killer result? If the
former is the case you don't need to do anything the data structure will
disappear when the program exits. As outlined in
:doc:`how-to-think-like-a-computer` RAM is volatile. Therefore if the latter is
the case you will want to make your tree persistent by writing it to disk.

Binary vs plain text...

The beauty of plain text files
------------------------------

Useful plain text file formats
------------------------------
