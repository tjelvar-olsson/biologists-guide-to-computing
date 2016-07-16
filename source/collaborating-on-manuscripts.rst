Collaborating on manuscripts
============================

Introduction
------------

In this chapter you will learn how to work collaboratively on documents using
Git. We will collaboratively craft a scientific document in plain text using
markdown syntax. We will then use `Pandoc <http://pandoc.org/>`_ to convert the
document to PDF.

Collaborating on plain text documents using Git has several benefits. Changes
to the documents are kept under version control giving a transparent audit
trail. Furthermore, merging contributions from collaborators is simple, and
automatic for all non-conflicting changes. 


Find a friend
-------------

The first step to collaboration is to have someone to collaborate with.
If you have not already shared your experiences of this book with anyone
I suggest that you engage the person sitting next to you in the office
as soon as possible. Things are more fun when you work with others.


Get a GitHub or BitBucket account
---------------------------------

Although you can host your own Git server I would strongly recommend making use
of either `GitHub <https://github.com/>`_ or `BitBucket
<https://bitbucket.org/>`_ as your remote server for backing up your Git
repositories.

Registration is free for both GitHub and BitBucket. GitHub gives you unlimited
number of public repositories and collaborators for free. However, you have to
pay for private repositories. BitBucket allows you to have private repositories
for free, but limits the number of collaborators that you can have on these.
Personally, I started with BitBucket because I was ashamed of my code and did not
want others to see it. Then I realised that code did not have to be perfect to
be useful and then I started using public GitHub repositories more and more.
It is up to you.


.. todo:: Go through data-analysis and data-visualisation chapters and add Git
          integration as you go along. The repository should be called:
          S.coelicolor-local-GC-content


Pushing the ``S.coelicolor-local-GC-content`` to a remote server
----------------------------------------------------------------

Now that you have a GitHub/BitBucket account it is time to push the
``S.coelicolor-local-GC-content`` repository to it.

Go to the web interface of your account and find the button that says something
along the lines of "New repostiory" or "Create repository".  Give the project
the name ``S.coelicolor-local-GC-content`` make sure that the backend uses Git
(if you are using BitBucket there is an option to use an alternative source
control management system named Mercurial) and that it is initialised as an
empty repository (i.e. don't tick any checkboxes to add ``README`` files,
licences or ``.gitignore`` files).

Now on your local computer (the one in front of you where you have been doing
all the exercises from this book so far) go into the directory containing the
``S.coelicolor-local-GC-content`` repository (it does not matter if you have
named the directory something else as long as it is the location where you
added the scripts from chapters :doc:`data-analysis` and doc:`data-visualisation`.

Add remote origin...


Collaboration using Git
-----------------------

The first step is to setup an account on GitHub or 
- GitHub and BitBucket
- Find a friend to experiment with
- Add a remote origin to an existing project
- Push and pull
- Resolving conflicts


Crafting scientific manuscripts using Pandoc
--------------------------------------------

- Basic structure
- Including figures
- Reference management
- Title, author and abstract
- Pandoc
- Citeproc


Benefits of using Git and plain text files
------------------------------------------

- version control
- painless merging of changes
- transparency and audit trial
- automation

Take home messages
------------------
