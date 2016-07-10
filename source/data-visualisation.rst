Data visualisation
==================


Introduction
------------

So far we have been learning how to crunch data. Now we will look into
visualising data.

There are two main purposes of representing data visually:

1. To explore the data
2. To convey a message to an audience

In this chapter we will look at how both of these can be accomplished using `R
<https://www.r-project.org/>`_ and its associated `ggplot2 <http://ggplot2.org/>`_
package.
 
We will start off by exploring Anderson's `Iris flower data set
<https://en.wikipedia.org/wiki/Iris_flower_data_set>`_. This will be an
exercise in data exploration.

Finally, we will use R and ggplot2 to build up a sliding window plot of the
GC content data from the :doc:`data-analysis` chapter. This will serve to
illustrate some points to consider when trying to convey a message to an
audience.


Starting R and loading the Iris flower data set
-----------------------------------------------

R like Python can be run in an interactive shell. This is a great way to
get familiar with R. To access R's interactive shell simply type ``R`` into your terminal and press enter.

.. code-block:: none

    $ R

    R version 3.2.2 (2015-08-14) -- "Fire Safety"
    Copyright (C) 2015 The R Foundation for Statistical Computing
    Platform: x86_64-apple-darwin14.5.0 (64-bit)

    R is free software and comes with ABSOLUTELY NO WARRANTY.
    You are welcome to redistribute it under certain conditions.
    Type 'license()' or 'licence()' for distribution details.

      Natural language support but running in an English locale

    R is a collaborative project with many contributors.
    Type 'contributors()' for more information and
    'citation()' on how to cite R or R packages in publications.

    Type 'demo()' for some demos, 'help()' for on-line help, or
    'help.start()' for an HTML browser interface to help.
    Type 'q()' to quit R.

    >

R comes bundled with a number of data sets. To view these you can use the
``data()`` function, which lists all the available data sets.

.. code-block:: R

    > data()
    Data sets in package ‘datasets’:

    AirPassengers           Monthly Airline Passenger Numbers 1949-1960
    BJsales                 Sales Data with Leading Indicator
    BJsales.lead (BJsales)
                            Sales Data with Leading Indicator
    BOD                     Biochemical Oxygen Demand
    CO2                     Carbon Dioxide Uptake in Grass Plants
    ...

We are interested in the Iris flower data set, called ``iris``, let us load
it.

.. code-block:: R

    > data(iris)

This loads the ``iris`` data set into the workspace. You can list the content
of the workspace using the ``ls()`` function.

.. code-block:: R

    > ls()
    [1] "iris"

.. sidebar:: What is a workspace?

    R has the concept of a "workspace". The workspace is the current working
    environment and includes any user defined objects. At the end of a R
    session the user can save the workspace. If saved the workspace will be
    automatically loaded the next time R is started.


Understanding the structure of the ``iris`` data set
----------------------------------------------------

First of all let us find out about the internal structure of the ``iris`` data set using the ``str()`` funciton.

.. code-block:: R

    > str(iris)
    'data.frame':   150 obs. of  5 variables:
     $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
     $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
     $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
     $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
     $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...

This reveals that the ``iris`` data set is a data frame with 150 observations
and five variables. It is also worth noting that ``Species`` is recorded as
a Factor data structure. This means that it has categorical data. In this case
three different species.

In R a data frame is a data structure for storing two-dimensional data, i.e.
like a spreadsheet with rows and columns. In a data frame each column contains
the same type of data and each row has values for each column.

.. sidebar:: What is the difference between vectors, lists and data frames?

    As well as data frames R also has concepts of vectors and lists.
    A vector is a list where each item is of the same type. A list
    is more flexible in that the items can be of different types.
    The data frame is essentially a list of equal length lists.

You can find the names of the columns in a data frame using the ``names()`` function.

.. code-block:: R

    > names(iris)
    [1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width"  "Species"

To find the number of columns and rows one can use the ``ncol()`` and
``nrow()`` functions, respectively.

.. code-block:: R

    > ncol(iris)
    [1] 5
    > nrow(iris)
    [1] 150

To view the first six rows of a data frame one can use the ``head()`` function.

.. code-block:: R

        > head(iris)
      Sepal.Length Sepal.Width Petal.Length Petal.Width Species
    1          5.1         3.5          1.4         0.2  setosa
    2          4.9         3.0          1.4         0.2  setosa
    3          4.7         3.2          1.3         0.2  setosa
    4          4.6         3.1          1.5         0.2  setosa
    5          5.0         3.6          1.4         0.2  setosa
    6          5.4         3.9          1.7         0.4  setosa

To view the last six rows of a data frame one can use the ``tail()`` function.

.. code-block:: R

    > tail(iris)
        Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
    145          6.7         3.3          5.7         2.5 virginica
    146          6.7         3.0          5.2         2.3 virginica
    147          6.3         2.5          5.0         1.9 virginica
    148          6.5         3.0          5.2         2.0 virginica
    149          6.2         3.4          5.4         2.3 virginica
    150          5.9         3.0          5.1         1.8 virginica


A note on statistics in R
-------------------------

Although this book is not about statistics it is worth mentioning that R is a
superb tool for doing statistics. It has many built in functions for statisitcal
computing. For example to calculate the median ``Sepal.Length`` for the ``iris``
data one can use the built in ``median()`` function.

.. code-block:: R

    > median(iris$Sepal.Length)
    [1] 5.8

In the above the ``$`` symbol is used to specify the column of interest in the
data frame.

Another useful tool for getting an overview of a data set is the
``summary()`` function.

.. code-block:: R

    > summary(iris)
      Sepal.Length    Sepal.Width     Petal.Length    Petal.Width
     Min.   :4.300   Min.   :2.000   Min.   :1.000   Min.   :0.100
     1st Qu.:5.100   1st Qu.:2.800   1st Qu.:1.600   1st Qu.:0.300
     Median :5.800   Median :3.000   Median :4.350   Median :1.300
     Mean   :5.843   Mean   :3.057   Mean   :3.758   Mean   :1.199
     3rd Qu.:6.400   3rd Qu.:3.300   3rd Qu.:5.100   3rd Qu.:1.800
     Max.   :7.900   Max.   :4.400   Max.   :6.900   Max.   :2.500
           Species
     setosa    :50
     versicolor:50
     virginica :50


Default plotting in R
---------------------

Before using ggplot2 let us have a look at how to generate default
plots in R.

First of all let us plot a histogram of the ``Sepal.Length``.

.. code-block:: R

    > hist(iris$Sepal.Length)

.. figure:: images/iris_sepal_length_histogram.png
   :alt: Iris sepal length histogram.

   Histogram of Iris sepal length data generated using R's built in ``hist()``
   function.

Having to type out the name of the data set every time you want to access
a column from it can get annoying. To overcome this problem R has the
built in function ``attach()``, which can be used to attach objects to R's
search path. This means that the columns in the data frame become available
via their names.

.. code-block:: R

    > attach(iris)

We can now produce the same histogram using the command below.

.. code-block:: R

    > hist(Sepal.Length)

Scatter plots can be produced using the ``plot()`` function.
The command below produces a scatter plot of ``Sepal.Length``
versus ``Sepal.Width``.

.. code-block:: R

    > plot(Sepal.Length, Sepal.Width)

.. figure:: images/iris_sepal_length_vs_width_scatterplot.png
   :alt: Iris sepal length vs width scatter plot.

   Scatter plot of Iris sepal length vs width generated using R's built in ``plot()``
   function.

Finally, a decent overview of the all data can be obtained by passing the
entire data frame to the ``plot()`` function.

.. code-block:: R

    > plot(iris)

.. figure:: images/iris_sepal_data_summary_plot.png
   :alt: Iris sepal data summary plot.

   Overview plot of Iris data using R's built in ``plot()`` function.

R's built in plotting functions are useful for getting quick exploratory
views of the data. However, they are a bit dull. In the next section we will
make use of the ggplot2 package to make more visually pleasing and informative
figures.


Loading the ggplot2 package
---------------------------

In order to make use of the ggplot2 package we need to load it. This is achieved
using the ``library()`` function.

.. sidebar:: Why do I load a package using a function called library?

    There are many aspects of R, which can be confusing. A common confusion
    stems around the usage of the words library and package.

    In R the term "package" refers to a structured collection of functions,
    data and compiled code, whereas the term "library" refers to the location
    where packages are stored.

    However, because of the naming of the ``library()`` function many people
    think it is used to load libraries. However it does not, it loads packages.
    This has resulted in the terms library and package being used
    interchangeably. 

    This may all sound a bit esoteric, but it does illustrate why you should take
    care when naming your variables, functions and scripts. If you name them
    inappropriately they will cause confusion.


.. code-block:: R

    > library("ggplot2")


Plotting using ggplot2
----------------------

To get an idea of what it feels like to work with ggplot2 let us re-create the
previous histogram and scatter plot with it.

Let us start with the histogram.

.. code-block:: R

    > ggplot(data=iris, mapping=aes(Sepal.Length)) + geom_histogram()

.. figure:: images/ggplot_iris_sepal_length_histogram.png
   :alt: Iris sepal length histogram plotted using ggplot.

   Histogram of Iris sepal length data generated using R's ggplot2 package.
   The bin width used is different from the one used by R's built in ``hist()``
   function, hence the difference in the appearance of the distribution.

The syntax used may look a little bit strange at first. However, before going
into more details about what it all means let's create the scatter plot to
get a better feeling of how to work with ggplot2.



.. code-block:: R

    > ggplot(data=iris, mapping=aes(x=Sepal.Length, y=Sepal.Width)) + geom_point()


.. figure:: images/ggplot_iris_sepal_length_vs_width_scatterplot.png
   :alt: Iris sepal length vs width scatter plotted using ggplot.

   Scatter plot of Iris sepal length vs width generated using R's ggplot2
   package.

In the examples above we provide the ``ggplot()`` function with two
arguments ``data`` and ``mapping``. The former contains the data frame
of interest and the latter specifies the columns(s) to be plotted.

The ``ggplot`` function returns a ggplot object that can be plotted.  However,
in order to view an actual plot one needs to add a layer to the ggplot object
defining how the data should be presented.  In the examples above this is
achieved using the ``+ geom_histogram()`` and ``+ geom_point()`` syntax.

A ggplot object consists of separate layers. The reason for separating out the
generation of a figure into separate layers is to allow the user to better be
able to reason about the best way to represent the data.

The three layers that we have come across so far are:

- Data: the data to be plotted
- Aesthetic: how the data should be mapped to the aesthetics of the plot
- Geom: what type of plot should be used

Of the above the "aestheitc" layer is the trickest to get ones head around.
However, take for example the scatter plot, one aesthetic choice that we have
made for that plot is that the ``Sepal.Length`` should be on the x-axis and
the ``Sepal.Width`` should be on the y-axis.

To reinforce this let us augment the scatter plot by sizing the points in the
scatter plot by the ``Petal.Width`` and coloring them by the ``Species``, all
of which could be considered to be aesthetic aspects of how to plot the data.

.. code-block:: R

    > ggplot(iris, aes(x=Sepal.Length, y=Sepal.Width, size=Petal.Width, color=Species)) + geom_point()

.. figure:: images/ggplot_aesthetic_scatter_plot.png
   :alt: Iris sepal length vs width, ponints sized by petal width coloured by species.

   Scatter plot of Iris sepal length vs width, where the size of each point represents
   the petal width and the colour is used to indicate the species.

By adding these additional aesthetic attributes to the figure we can start to
discern some structure that was previously hidden.


Scripting data visualisation
----------------------------

Now that we have got a basic understanding of how to interact with R and the functionality
in the ggplot library let's take a step towards automating the figure generation by
recording the steps required to plot the data in a script.

Let us work on the histogram example. In the code below we store the ggplot
object in the variable ``g`` and make use of ``ggsave()`` to write the plot to
a file named ``histogram.png``. Save the code below to a file named
``iris_sepal_length_histogram.R``

.. code-block:: R

    
    library("ggplot2")
    data(iris)

    g <- ggplot(data=iris, aes(Sepal.Length)) +
         geom_histogram()

    ggsave('iris_sepal_length_histogram.png')

To run this code we make use of the program ``Rscript``, which comes bundled with your
R installation.

.. code-block:: none

    $ Rscript iris_sepal_length_histogram.R

This will write the file ``iris_sepal_length_histogram.png`` to your current working
directory.


Faceting
--------

In the extended scatterplot example we found that it was useful to be able to
visualise which data points belonged to which species. Maybe it would be useful
to do something similar with the data in our histogram.

In order to be able to achieve this ggplot2 provides the concept of faceting,
the ability to split your data by one or more variables. Let us split the
data by ``Species`` using the ``facet_grid()`` function.

.. code-block:: R

    library("ggplot2")
    data(iris)

    g <- ggplot(data=iris, aes(Sepal.Length)) +
         geom_histogram() +
         facet_grid(Species ~ .)

    ggsave('histogram.png')

In the above the ``facet_grid(Species ~ .)`` states that we want one
``Species`` per row, as opposed to one species per column
(``facet_grid(. ~ Species)``). Replacing the dot (``.``) with another
variable would result in a faceting the data into a two dimensional grid.

.. figure:: images/ggplot_faceted_histogram.png
   :alt: Iris sepal length histogram faceted by species.

   Histogram of Iris sepal length data faceted by species.


Adding more colour
------------------

The faceted histogram plot clearly illustrates that there are differences
in the distributions of the sepal length between the different Iris species.

However, suppose that the faceted histogram figure was meant to be displayed
next to the extended scatter plot produced above. To make it easier to make
the mental connection between the two data representations it may be useful
to colour the histograms by species as well.

The colour of a histogram is an aesthetic characteristic. Let us add the
fill colour as an aesthetic to the histogram geometric object.

.. code-block:: R

    library("ggplot2")
    data(iris)

    g <- ggplot(data=iris, aes(Sepal.Length)) +
         geom_histogram(aes(fill=Species)) +
         facet_grid(Species ~ .)

    ggsave('histogram.png')


.. figure:: images/ggplot_faceted_and_coloured_histogram.png
   :alt: Iris sepal length histogram faceted and coloured by species.

   Histogram of Iris sepal length data faceted and coloured by species.


Purpose of data visualisation
-----------------------------

There are two main purposes of representing data visually:

1. To explore the data
2. To convey a message to an audience

So far we have been focussing on the former. Let us know think a little bit
more about the latter, conveying a message to an audience. In other words
creating figures for presentations and publications.


Conveying a message to an audience
----------------------------------

The first step in creating an informative figure is to consider who the
intended audience is. Is it the general public, school children or
plant biologists? The general public may be interested in trends, whereas
a plant scientist may be interested in a more detailed view.

Secondly, what is the message that you want to convey. Stating this explicitly
will help you when making decisions about the content and the layout of the
figure.

Thirdly, what medium will be used to display the figure. Will it be displayed
for a minute on a projector or will it be printed in an article? The audience
will have less time to absorb details from the former.

So suppose we wanted to create a figure intended for biologists, illustrating
that there is not much variation in the local GC content of *Streptomyces
coelicolor*  A3(2). Let us further suppose that the medium is in a printed
journal where the figure can have a maximum width of 89 mm.

The code below load the data and plots it as a line, resulting in a local GC
content plot. Note also that we specify the width and height of the plot in
millimeters in the ``ggsave()`` function.

.. code-block:: R

    library("ggplot2")
    df = read.csv("local_gc_content.csv", header=T)
    g = ggplot(df, aes(x=middle, y=gc_content)) +
        geom_line()
    ggsave("local_gc_content.png", width=89, height=50, units="mm")


.. figure:: images/local_gc_content_1.png
   :alt: Initial attempt at plotting local GC content.

   Initial attempt at plotting local GC content.


However, the scale of the y-axis makes the plot misleading. It looks like there
is a lot of variation in the data. Let's expand the y range to span from 0 to a
100 percent.

.. code-block:: R

    library("ggplot2")
    df = read.csv("local_gc_content.csv", header=T)
    g = ggplot(df, aes(x=middle, y=gc_content)) +
        geom_line() +
        ylim(0, 100)
    ggsave("local_gc_content.png", width=89, height=50, units="mm")


.. figure:: images/local_gc_content_2.png
   :alt: Local GC content with y-axis scaled corretly.

   Local GC content with y-axis scaled corretly.


At the moment it looks like the line is floating in mid-air. This is because
ggplot adds some padding to the limits. Let's turn this off.

.. code-block:: R

    library("ggplot2")
    df = read.csv("local_gc_content.csv", header=T)
    g = ggplot(df, aes(x=middle, y=gc_content)) +
        geom_line() +
        ylim(0, 100) + 
        coord_cartesian(expand=FALSE)
    ggsave("local_gc_content.png", width=89, height=50, units="mm")


.. figure:: images/local_gc_content_3.png
   :alt: Local GC content without padding.

   Local GC content without padding.


The labels on the x-axis are a bit difficult to read. To make it easier
to understand the content of the x-axis let's scale it to use kilobases
as its units.

.. code-block:: R

    library("ggplot2")
    df = read.csv("local_gc_content.csv", header=T)
    g = ggplot(df, aes(x=middle, y=gc_content)) +
        geom_line() +
        ylim(0, 100) + 
        coord_cartesian(expand=FALSE) +
        scale_x_continuous(labels=function(x)x/1000)
    ggsave("local_gc_content.png", width=89, height=50, units="mm")


.. figure:: images/local_gc_content_4.png
   :alt: Local GC content with kilobases as the x axis units.

   Local GC content with kilobases as the x axis units.


Finally, let us add labels to the x-axis.

.. code-block:: R

    library("ggplot2")
    df = read.csv("local_gc_content.csv", header=T)
    g = ggplot(df, aes(x=middle, y=gc_content)) +
        geom_line() +
        ylim(0, 100) + 
        coord_cartesian(expand=FALSE) +
        scale_x_continuous(labels=function(x)x/1000) +
        xlab("Nucleotide position (KB)") +
        ylab("GC content (%)")
    ggsave("local_gc_content.png", width=89, height=50, units="mm")


.. figure:: images/local_gc_content_5.png
   :alt: Local GC content with labelled axis.

   Local GC content with labelled axis.


Writing a caption
-----------------

All figures require a caption to help the audience understand how to interpret
the plot.

In this particular case two particular items stand out as missing from being
able to interpret the plot. First of all what is the source of the DNA, i.e.
where is the data from? Secondly, what was the window and step sizes of the
sliding window analysis?

It would also be appropriate to explicitly state what the message of the plot
is, i.e. that there is not much variation in the local GC content. In this case
we could further substantiate this claim by citing the mean and standard deviation
of the local GC content across the genome.

.. code-block:: R

    > df = read.csv("local_gc_content.csv", header=T)
    > mean(df$gc_content)
    [1] 72.13948
    > sd(df$gc_content)
    [1] 1.050728

Below is the final figure and it's caption

.. figure:: images/local_gc_content_5.png
   :alt: Local GC content with labelled axis.

   There is little variation in the local GC content of *Streptomyces
   coelicolor* A3(2). Using a window size of 100 KB and a step size of 50 KB
   the local GC content has a mean of 72.1% and a standard deviation of 1.1%.

Other useful tools for scripting the generation of figures
----------------------------------------------------------

There are many different ways of visualising data and generating figures.  A
broad distinction can be made between *ad-hoc* methods, usually using graphical
user interfaces and button clicking, and methods that can be automated, i.e.
methods that can reproduce the figure without human intervention.

One take home message from this chapter is that you should automate the
generation of your figures. This will save you time when you realize that you
need to alter the style of all the figures when submitting a manuscript for
publication. It also will make your research more reproducible.

Apart from R and ggplot there are several tools available for automating the
generation of your figures.  In Python there is the `matplotlib
<http://matplotlib.org/>`_ package, which is very powerful and it is a great
tool for plotting data generated from Python scripts.  `Gnuplot
<http://www.gnuplot.info/>`_ is a scripting language designed to plot data and
mathematical functions, it is particularly good at depicting three-dimensional
data. 

If you are dealing with graphs, as in evolutionary trees and metabolic pathways,
it is worth having a look at `Graphviz <www.graphviz.org/>`_.

A general purpose tool for manipulating images on the command line is
`ImageMagick <www.imagemagick.org/>`_. It can be used to resize, crop and
transform your images.  It is a great tool for automating your image
manipulation.

If you are wanting to visualise data in the web pages it worth invsting some
time looking at the various JavaScript libraries out there for data visualisation.
One poplar option is `D3.js <https://d3js.org/>`_.


Key concepts
------------

- R and ggplot are powerful tools for visualising data
- R should also be your first port of call for statistical computing (although
  that is not covered in this book)
- In ggplot visual representation of your data are built up using layers
- Separating out different aspects of plotting into different layers makes
  it easier to reason about the data and the best way to represent it visually
- Scripting your data visualisation makes it easier to modify later on
- Scripting your data visualisation makes it reproducible
- When creating a figure ask yourself: who is the audience and what is the message
