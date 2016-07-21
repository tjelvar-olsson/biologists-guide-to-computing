Collaborating on projects
=========================

Introduction
------------

In this chapter you will learn how to work collaboratively on projects using
Git.

Collaborating using Git has several benefits. Changes to the documents are kept
under version control giving a transparent audit trail. Furthermore, merging
contributions from collaborators is simple, and automatic for all
non-conflicting changes. 


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
``S.coelicolor-local-GC-content`` repository created in
:doc:`data-analysis` and built on in :doc:`data-visualisation`.

.. code-block:: none

    $ cd S.coelicolor-local-GC-content

We now need to specify the remove repository. My user name on GitHub is
``tjelvar-olsson`` and the repository I created on GitHub was named
``S.coelicolor-local-GC-content``. To add a remote to my local repository
(the one on my computer) I use the command below.

.. code-block:: none

    git remote add origin https://github.com/tjelvar-olsson/S.coelicolor-local-GC-content.git

This command adds a remote named ``origin`` to the local repository. However,
we have not yet *pushed* any data to this remote. Let's do this now.

.. code-block:: none

    git push -u origin master

The ``git push`` command pushes the changes made to the local repository to the
remote repository (named ``origin``). In this case the last argument (``master``)
specifies the specific branch to push.

The ``-u`` option is the short hand for ``--set-upstream`` and is used to set
up the association between your local branch (``master``) and the branch on the
remote. You only need to specify the ``-u`` option the first time you push a
local repository to a remote.

If you are using GitHub or BitBucket to host your remotes you do not need to
remember these commands as the web interface will display the commands you
need to push an existing project to a newly created repository.

.. sidebar:: What is a branch?

    When one initialises a Git repository a default branch named ``master`` is
    created. In this book all work has been done on the ``master`` branch.

    However, it is possible to create new branches from any committed snapshot
    in the history. For example before submitting a paper to Nature one might
    create a branch named ``nature``. During the lengthy review process one
    could then continue working on the ``master`` branch. After six months
    when the reviewers come back with their comments one could then switch
    back to the ``nature`` branch and implement all the requested changes
    and send it back to the editor. At that point the ``nature`` branch will
    have diverged from the ``master`` branch. The editor of Nature then comes
    back stating that in spite of all the changes the manuscript will still
    be rejected due to lack of a "wow" factor. At this point one may want
    to submit to Science. However, one wants to incorporate all the changes
    made on the ``master`` and the ``nature`` branch. That is no problem
    Git has a powerful system for *merging* changes. In this case one
    could merge the ``nature`` branch back onto ``master``. At that point
    one could take a new branch named ``science`` from the master branch
    before submitting the manuscript to science, and so forth.

    Although branching is powerful, it is beyond the scope of this book.
    If you are interested in learning more about it I would recommend
    the free `Learn Git course on codecademy <https://www.codecademy.com/learn/learn-git>`_.


Collaboration using Git
-----------------------

Now it is time for your collaborator to get access to the repository.
Use the web interface and your friend's GitHub/BitBucket user name
to give them write access to the repository.

Now your friend should be able to *clone* the repository. Alternatively,
if all your friends are busy or uninterested you can clone the repository
on a different computer or in a different directory.

.. code-block:: none

    $ git clone git@github.com:tjelvar-olsson/S.coelicolor-local-GC-content.git
    Cloning into 'S.coelicolor-local-GC-content'...
    remote: Counting objects: 8, done.
    remote: Compressing objects: 100% (7/7), done.
    remote: Total 8 (delta 0), reused 8 (delta 0), pack-reused 0
    Receiving objects: 100% (8/8), done.
    Checking connectivity... done.

The command above clones my ``S.coelicolor-local-GC-content.git`` repository.
You will need to replace ``tjelvar-olsson`` with your user name. Alternatively,
have a look in the web interface for a string that can be used to clone the
repository.

In the web interface you may see that there are two options for cloning a
repository "ssh" and "https". You need to have write permissions to be able
to use the "ssh" protocol which allows you to push changes to the remote
from which the repository was cloned. If you do not have write permissions
you can clone the repository using the "https" protocol, which allows you
to *pull* changes from the remote. However, with "https" you cannot push
to the remote.

Now that your friend has cloned your repository it is time for him/her to
add something to it. Create the file ``README.md`` and add the markdown
text below to it.

.. code-block:: none

    # Local GC content variation in *S. coelicolor*

    Project investigating the local GC content of the
    *Streptomyces coelicolor* A3(2) genome.

Now let your friend add the ``README.md`` file to the repository and commit a
snapshot of the repository.

.. code-block:: none

    $ git add README.md
    $ git commit -m "Added readme file"
    [master a531ea4] Added readme file
     1 file changed, 4 insertions(+)
     create mode 100644 README.md

Finally, your friend can push the changes to the remote repository.

.. code-block:: none

    $ git push
    Counting objects: 3, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (3/3), 384 bytes | 0 bytes/s, done.
    Total 3 (delta 1), reused 0 (delta 0)
    To https://github.com/tjelvar-olsson/S.coelicolor-local-GC-content.git
       cba7277..a531ea4  master -> master

Have a look at the repository in the using the GitHub/BitBucket web interface.
You should be able to see the changes reflected there.

Now look at your local repository. You will not see your friends changes reflected
there yet, because you have not yet *pulled* from the remote. It is time for you
to do that now. Run the ``git pull`` command in your local repository.

.. code-block:: none

    $ git pull
    remote: Counting objects: 3, done.
    remote: Compressing objects: 100% (2/2), done.
    remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0
    Unpacking objects: 100% (3/3), done.
    From github.com:tjelvar-olsson/S.coelicolor-local-GC-content
       cba7277..a531ea4  master     -> origin/master
    Updating cba7277..a531ea4
    Fast-forward
     README.md | 4 ++++
     1 file changed, 4 insertions(+)
     create mode 100644 README.md

You have now successfully pulled in the contributions from your friend into
your local repository. Very cool indeed!

Let's go over what happened again.

1. You created a new repository on GitHub
2. You added this repository as a remote to your local repository
3. You pushed the content of your local repository to the remote on GitHub
4. Your friend cloned your GitHub repository, creating a local repository on their machine
5. Your friend committed changes to their local repository
6. Your friend pushed the changes from their local repository to the remote on GitHub
7. You pulled in your friend's changes from the remote on GitHub to your local repository


Resolving conflicts
-------------------


Pull requests
-------------


Take home messages
------------------
