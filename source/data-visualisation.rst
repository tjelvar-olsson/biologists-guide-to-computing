Data visualisation
==================


Introduction
------------

So far we have been learning how to crunch data. Now we will look into
visualising data.

There are many different ways of visualising data and generating figures.  A
broad distinction can be made between *ad-hoc* methods, usually using graphical
user interfaces and button clicking, and methods that can be automated, i.e.
methods that can be reproduce the figure without human intervention.

The take home message from this chapter is that you should automate the
generation of your figures. This will save you time when you realize that you
need to alter the figures and it will make your research more reproducible.

There are several tools available for automating the generation of your
figures.  In Python there is the `matplotlib <http://matplotlib.org/>`_
package, which is very powerful and it is a great tool for plotting data
generated from Python scripts.  `Gnuplot <http://www.gnuplot.info/>`_ is a
scripting language designed to plot data and mathematical functions, it is
particularly good at depicting three-dimensional data.  `R
<https://www.r-project.org/>`_ is a statistical scripting language, with
built-in support for data visualisation. The default plots in R are not
beautiful, furthermore it can be difficult to customise them. However the
`ggplot2 <http://ggplot2.org/>`_ R package overcomes this problem by making
visually pleasing figures as well as making it easy to visualise the data using
different types of plots.

In this chapter we will be using R and ggplot to visualise and understand
Anderson's
`Iris flower data set <https://en.wikipedia.org/wiki/Iris_flower_data_set>`_.
Finally, we will use R and ggplot to create a sliding window plot of the
GC content data from the :doc:`data-analysis` chapter.


Quick R primer
--------------


Background
----------

- Purpose of data visualisation
- History of data visualisation


How to create informative figures
---------------------------------

- Audience
- Message
- Medium


Scripting the generation of your plot
-------------------------------------

- Matplotlib
- Gnuplot
- R


Writing a caption
-----------------


Other useful tools for scripting the generation of figures
----------------------------------------------------------

- Graphviz
- ImageMagick
- D3js


Key concepts
------------
