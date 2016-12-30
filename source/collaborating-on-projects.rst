Collaborating on projects
=========================

In this chapter you will learn how to work collaboratively on projects using
Git.

Collaborating using Git has several benefits. Changes to the documents are kept
under version control giving a transparent audit trail. Merging
contributions from collaborators is simple, and automatic for all
non-conflicting changes. Furthermore, as Git is a distributed version control
system the project becomes stored on many different computers, on yours and your
collaborators' computers as well as on remote servers, effectively backing up
your code and text files.

.. warning:: Although you can store anything in Git it is bad practise to
             track large files such as genomes and microscopy files in it.

             It is bad practise in that large files makes it difficult to move
             the repository between different computers.  Furthermore, once a
             file has been committed to Git it is almost impossible to remove
             it to free up the space.

             Also, if you are using a free service to host your code it is impolite
             to use it as a backup system for your raw data.

             It is therefore good practise to add large files in your project
             to the ``.gitignore`` file.


Find a friend
-------------

The first step to collaboration is to have someone to collaborate with.
If you have not already shared your experiences of this book with anyone
I suggest that you engage the person sitting next to you in the office
as soon as possible. Things are more fun when you work with others!


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
It is up to you. However the rest of this chapter will make use of GitHub as
a remote server, as it is my current favorite.


Pushing the project to a remote server
--------------------------------------

Now that you have a GitHub/BitBucket account it is time to push the
``S.coelicolor-local-GC-content`` project from the
:doc:`data-analysis` and :doc:`data-visualisation` chapters to it.

Go to the web interface of your account and find the button that says something
along the lines of "New repostiory" or "Create repository".  Give the project
the name ``S.coelicolor-local-GC-content`` and make sure that the backend uses Git
(if you are using BitBucket there is an option to use an alternative source
control management system named Mercurial) and that it is initialised as an
empty repository (i.e. don't tick any checkboxes to add readme,
licence or ``.gitignore`` files).

Now on your local computer (the one in front of you where you have been doing
all the exercises from this book so far) go into the directory containing the
``S.coelicolor-local-GC-content`` repository created in
:doc:`data-analysis` and built on in :doc:`data-visualisation`.

.. code-block:: none

    $ cd S.coelicolor-local-GC-content

We now need to specify the remote repository. My user name on GitHub is
``tjelvar-olsson`` and the repository I created on GitHub was named
``S.coelicolor-local-GC-content``. To add a remote to my local repository
(the one on my computer) I use the command below.

.. note:: In the command below ``tjelvar-olsson`` is my GitHub user name.

.. code-block:: none

    $ git remote add origin \
    https://github.com/tjelvar-olsson/S.coelicolor-local-GC-content.git

The command adds the GitHub :term:`URL` as a  remote named ``origin`` to the
local repository. However, we have not yet *pushed* any data to this remote.
Let's do this now.

.. code-block:: none

    $ git push -u origin master

The ``git push`` command pushes the changes made to the local repository to the
remote repository (named ``origin``). In this case the last argument (``master``)
specifies the specific branch to push to.

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
    back to the ``nature`` branch and implement all the suggested changes
    and send it back to the editor. At that point the ``nature`` branch will
    have diverged from the ``master`` branch. The editor of Nature then comes
    back stating that in spite of all the changes the manuscript will still
    be rejected due to the lack of a "wow" factor. At this point one may want
    to submit to Science. However, one wants to incorporate all the changes
    made on the ``master`` and the ``nature`` branch. That is not a problem as
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
on a different computer or in a different directory to simulate the collaboration
process by working on the same project in two different locations.

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


Working in parallel
-------------------

The workflow described above was linear. You did some work, you friend cloned
your work, your friend did some work, you pulled your friends work. What if
you both worked on the project in parallel in your local repositories, how
would that work?

Let's try it out. In your local repository add some information on how
to download the genome to the ``README.md`` file.

.. code-block:: none
    :emphasize-lines: 6-10

    # Local GC content variation in *S. coelicolor*

    Project investigating the local GC content of the
    *Streptomyces coelicolor* A3(2) genome.

    Download the genome using ``curl``.

    ```
    $ curl --location --output Sco.dna http://bit.ly/1Q8eKWT
    ```

Now commit these changes.

.. code-block:: none

    $ git add README.md
    $ git commit -m "Added info on how to download genome"
    [master 442c433] Added info on how to download genome
     1 file changed, 6 insertions(+)

Now your friend tries to work out what the ``gc_content.py`` and
the ``local_gc_content_figure.R`` scripts do. The names are not
particularly descriptive so he/she looks at the code and works out
that the ``gc_content.py`` script produces a local GC content CSV
file from the genome and that the ``local_gc_content_figure.R``
script produces a local GC plot from the CSV file. Your friend
therefore decides to rename these scripts to ``dna2csv.py`` and
``csv2png.R``.

.. code-block:: none

    $ git mv gc_content.py dna2csv.py
    $ git mv local_gc_content_figure.R csv2png.R
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

            renamed:    local_gc_content_figure.R -> csv2png.R
            renamed:    gc_content.py -> dna2csv.py

    $ git commit -m "Improved script file names"
    [master 1f868ad] Improved script file names
     2 files changed, 0 insertions(+), 0 deletions(-)
     rename local_gc_content_figure.R => csv2png.R (100%)
     rename gc_content.py => dna2csv.py (100%)

At this point your friend pushes their changes to the GitHub remote.

.. code-block:: none

    $ git push
    Counting objects: 2, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (2/2), done.
    Writing objects: 100% (2/2), 351 bytes | 0 bytes/s, done.
    Total 2 (delta 0), reused 0 (delta 0)
    To git@github.com:tjelvar-olsson/S.coelicolor-local-GC-content.git
       a531ea4..1f868ad  master -> master

Now you realise that you have not pushed the changes that you made to the
``README.md`` file to the GitHub remote. You therefore try to do so.

.. code-block:: none

    $ git push
    To https://github.com/tjelvar-olsson/S.coelicolor-local-GC-content.git
     ! [rejected]        master -> master (fetch first)
    error: failed to push some refs to 'https://github.com/tjelvar-olsson/...'
    hint: Updates were rejected because the remote contains work that you do
    hint: not have locally. This is usually caused by another repository pushing
    hint: to the same ref. You may want to first integrate the remote changes
    hint: (e.g., 'git pull ...') before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.

Rejected? What is going on? Well, it turns out that the "hints" give us some useful
information.  They inform us that the update was rejected because the remote
contained work that we did not have in our local repository. It also suggests
that we can overcome this by using ``git pull``, which would pull in your
friends updates from the GitHub remote and merge them with your local updates.

.. code-block:: none

    $ git pull

Now unless you have defined an alternative editor as your default (using the
``$EDITOR`` environment variable) you will be dumped into a ``vim`` session
with the text below in the editor.

.. code-block:: none

    Merge branch 'master' of https://github.com/tjelvar-olsson/...

    # Please enter a commit message to explain why this merge is necessary,
    # especially if it merges an updated upstream into a topic branch.
    #
    # Lines starting with '#' will be ignored, and an empty message aborts
    # the commit.

To accept these changes you need to save the file and exit the editor (in Vim
``Esc`` followed by ``:wq``). Once this is done you will be dumped back into
your shell.

.. code-block:: none

    $ git pull
    remote: Counting objects: 2, done.
    remote: Compressing objects: 100% (2/2), done.
    remote: Total 2 (delta 0), reused 2 (delta 0), pack-reused 0
    Unpacking objects: 100% (2/2), done.
    From https://github.com/tjelvar-olsson/S.coelicolor-local-GC-content
       a531ea4..1f868ad  master     -> origin/master
    Merge made by the 'recursive' strategy.
     local_gc_content_figure.R => csv2png.R | 0
     gc_content.py => dna2csv.py            | 0
     2 files changed, 0 insertions(+), 0 deletions(-)
     rename local_gc_content_figure.R => csv2png.R (100%)
     rename gc_content.py => dna2csv.py (100%)

Note that Git managed to work out how to merge these changes together automatically.
Let's have a look at the history.

.. code-block:: none

    $ git log --oneline
    a5779d6 Merge branch 'master' of https://github.com/tjelvar-olsson/S.coelicolor-local-GC-content
    1f868ad Improved script file names
    442c433 Added info on how to download genome
    a531ea4 Added readme file
    cba7277 Added R script for generating local GC plot
    6d8e0cf Initial file import

Now we should now be able to push to the GitHub remote.

.. code-block:: none

    $ git push
    Counting objects: 5, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (5/5), done.
    Writing objects: 100% (5/5), 765 bytes | 0 bytes/s, done.
    Total 5 (delta 2), reused 0 (delta 0)
    To https://github.com/tjelvar-olsson/S.coelicolor-local-GC-content.git
       1f868ad..a5779d6  master -> master

Collaboratively working on projects in parallel, really cool stuff!

Let's go over what happened.

1. You committed some changes to your local repository
2. Your friend committed some changes to their local repository
3. Your friend pushed their changes to the GitHub remote
4. You tried, but failed, to push your changes to the GitHub remote
5. You pulled in your friend's changes from the GitHub remote
6. Git automatically worked out how to merge these changes with yours
7. You pushed the merged changes to the remote


Resolving conflicts
-------------------

So far so good, but what happens if both you and your friend
edit the same part of the same file in your local repositories?
How does Git deal with this? Let's try it out.

First of all your friend pulls in your changes from the GitHub remote.  By
pulling your friend ensures that he/she is working on the latest version of the
code.

.. code-block:: none

    $ git pull
    remote: Counting objects: 5, done.
    remote: Compressing objects: 100% (3/3), done.
    remote: Total 5 (delta 2), reused 5 (delta 2), pack-reused 0
    Unpacking objects: 100% (5/5), done.
    From github.com:tjelvar-olsson/S.coelicolor-local-GC-content
       1f868ad..a5779d6  master     -> origin/master
    Updating 1f868ad..a5779d6
    Fast-forward
     README.md | 6 ++++++
     1 file changed, 6 insertions(+)


Your friend then edits the ``README.md`` file.

.. code-block:: none
    :emphasize-lines: 12-16

    # Local GC content variation in *S. coelicolor*

    Project investigating the local GC content of the
    *Streptomyces coelicolor* A3(2) genome.

    Download the genome using ``curl``.

    ```
    $ curl --location --output Sco.dna http://bit.ly/1Q8eKWT
    ```

    Generate local GC content csv file from the genome.

    ```
    $ python dna2csv.py
    ```

Your friend then commits and pushes these changes.

.. code-block:: none

    $ git add README.md
    $ git commit -m "Added info on how to generate local GC content CSV"
    [master 9f41c21] Added info on how to generate local GC content CSV
     1 file changed, 6 insertions(+)
    $ git push
    Counting objects: 3, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (3/3), 399 bytes | 0 bytes/s, done.
    Total 3 (delta 2), reused 0 (delta 0)
    To git@github.com:tjelvar-olsson/S.coelicolor-local-GC-content.git
       a5779d6..9f41c21  master -> master

Now you work on your local repository. You too are concerned with giving
more detail about how to make use of the scripts so you edit your
local copy of the ``README.md`` file.

.. code-block:: none
    :emphasize-lines: 12-17

    # Local GC content variation in *S. coelicolor*

    Project investigating the local GC content of the
    *Streptomyces coelicolor* A3(2) genome.

    Download the genome using ``curl``.

    ```
    $ curl --location --output Sco.dna http://bit.ly/1Q8eKWT
    ```

    Data processing.

    ```
    $ python dna2csv.py
    $ Rscript csv2png.R
    ```

And you commit the changes to your local repository.

.. code-block:: none

    $ git add README.md
    [-- olssont@ exit=0 S.coelicolor-local-GC-content --]
    $ git commit -m "Added more info on how to process data"
    [master 2559b5d] Added more info on how to process data
     1 file changed, 7 insertions(+)

However, when you try to push you realise that your friend has pushed
changes to the remote.

.. code-block:: none

    $ git push
    Username for 'https://github.com': tjelvar-olsson
    Password for 'https://tjelvar-olsson@github.com':
    To https://github.com/tjelvar-olsson/S.coelicolor-local-GC-content.git
     ! [rejected]        master -> master (fetch first)
    error: failed to push some refs to 'https://github.com/tjelvar-olsson/...'
    hint: Updates were rejected because the remote contains work that you do
    hint: not have locally. This is usually caused by another repository pushing
    hint: to the same ref. You may want to first integrate the remote changes
    hint: (e.g., 'git pull ...') before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.

So you pull.

.. code-block:: none

    $ git pull
    remote: Counting objects: 3, done.
    remote: Compressing objects: 100% (1/1), done.
    remote: Total 3 (delta 2), reused 3 (delta 2), pack-reused 0
    Unpacking objects: 100% (3/3), done.
    From https://github.com/tjelvar-olsson/S.coelicolor-local-GC-content
       a5779d6..9f41c21  master     -> origin/master
    Auto-merging README.md
    CONFLICT (content): Merge conflict in README.md
    Automatic merge failed; fix conflicts and then commit the result.

The automatic merge failed! The horror!

Let's find out what the status of the repository is.

.. code-block:: none

    $ git status
    On branch master
    Your branch and 'origin/master' have diverged,
    and have 1 and 1 different commit each, respectively.
      (use "git pull" to merge the remote branch into yours)
    You have unmerged paths.
      (fix conflicts and run "git commit")

    Unmerged paths:
      (use "git add <file>..." to mark resolution)

            both modified:   README.md

    no changes added to commit (use "git add" and/or "git commit -a")

Okay, so both you and your friend have been editing the ``README.md`` file.
Let's have a look at it.

.. code-block:: none
    :emphasize-lines: 13-17, 19-22

    # Local GC content variation in *S. coelicolor*

    Project investigating the local GC content of the
    *Streptomyces coelicolor* A3(2) genome.

    Download the genome using ``curl``.

    ```
    $ curl --location --output Sco.dna http://bit.ly/1Q8eKWT
    ```

    <<<<<<< HEAD
    Data processing.

    ```
    $ python dna2csv.py
    $ Rscript csv2png.R
    =======
    Generate local GC content csv file from the genome.

    ```
    $ python dna2csv.py
    >>>>>>> 9f41c215cce3500e80c747426d1897f93389200c
    ```

So Git has created a "merged" file for us and highlighted the section that
is conflicting. In the above the first highlighted region contains the changes
from the ``HEAD`` in your local repository and the second highlighted region
shows the changes from your friend's commit ``9f41c21``.

Now you need to edit the file so that you are happy with it. Your
friend's idea of documenting what the command does is a good one so you could
edit the ``README.md`` file to look like the below.

.. code-block:: none
    :emphasize-lines: 12-22

    # Local GC content variation in *S. coelicolor*

    Project investigating the local GC content of the
    *Streptomyces coelicolor* A3(2) genome.

    Download the genome using ``curl``.

    ```
    $ curl --location --output Sco.dna http://bit.ly/1Q8eKWT
    ```

    Generate ``local_gc_content.csv`` file from ``Sco.dna`` file.

    ```
    $ python dna2csv.py
    ```

    Generate ``local_gc_content.png`` file from ``local_gc_content.csv`` file.

    ```
    $ Rscript csv2png.R
    ```

Now you need to add and commit these changes.

.. code-block:: none

    $ git add README.md
    [-- olssont@ exit=0 S.coelicolor-local-GC-content --]
    $ git commit -m "Manual merge of readme file"
    [master 857b470] Manual merge of readme file

Now that you have merged the changes you can push to the remote.

.. code-block:: none

    $ git push
    Counting objects: 6, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (6/6), done.
    Writing objects: 100% (6/6), 708 bytes | 0 bytes/s, done.
    Total 6 (delta 4), reused 0 (delta 0)
    To https://github.com/tjelvar-olsson/S.coelicolor-local-GC-content.git
       9f41c21..857b470  master -> master

Let's go over what just happened.

1. Your friend pulled in your changes from the GitHub remote
2. Your friend edited the ``README.md`` file
3. Your friend committed and pushed their changes to the remote
4. You edited the ``README.md`` file in your local repository
5. You committed the changes to your local repository
6. You tried but failed to push to the GitHub remote repository
7. You pulled the changes from the GitHub remote repository; but the automatic merging failed
8. You looked at the status of your local repository to find out what state it was in
9. You resolved the conflicts manually by editing the ``README.md`` file
10. You added the updated ``README.md`` file to the staging area and committed it
11. You pushed your manual merge to the GitHub remote repository

That's it, you now have all the tools you need to start collaborating with
your colleagues using Git! May the force be with you.


Key concepts
------------

- Git is a powerful tool for collaborating on projects
- Every person working on a Git project have all the files on their local computer
- By adding a remote to a local repository one can push updates to another repository (the remote)
- It is also possible to pull changes from a remote
- One way to collaborate using Git is to have multiple people pulling from and pushing to the same remote repository
- It is only possible to push to a remote if you have all the updates that are on the remote in your local repository
- When pulling Git will do its best to resolve any conflicts between your local updates and updates from the remote
- If there are conflicts that Git cannot resolve you have to fix them manually
- By pushing your updates to a remote server such as GitHub you are effectively backing up your project
