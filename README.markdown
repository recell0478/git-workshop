What is Git ?
=============

Git is a *distributed* **version control** system [1]

[1] <a href="http://git-scm.com/about">http://git-scm.com/about</a>

Getting Git
-----------

Some house-cleaning here. We assume of course you have Git installed,
(hopefully \>= 1.7.0).

If you don't you can install it from downloads on the git homepage or you can
install [Github's git GUI](https://help.github.com/articles/set-up-git/).

Setup
-----

First thing to do is to setup your identity. This identifies you to
other people who download the project.

    $ git config --global user.name "Your Name"
    $ git config --global user.email your.email@example.com

As a helpful step, you may want to set Git to use your favourite editor

    $ git config --global core.editor vim

Starting your journey
---------------------

First, clone this repository:

    $ git clone https://github.com/kuahyeow/git-workshop.git

You may want to fork (create your own copy of) the project on github and
clone from your own repo. You can find the fork button at the top right of
the screen on a github repository, or more help about doing that [here](https://help.github.com/articles/fork-a-repo/).

Once you have cloned your repository, you should now see a directory
called `git-workshop`. This is your `working directory`

    $ cd git-workshop
    $ ls

![](https://upload.wikimedia.org/wikipedia/commons/thumb/4/44/Help-browser.svg/20px-Help-browser.svg.png)
Stuck? Ask for help from the workshop staff

Branching
----------------
Branching is an essential part of working on a  codebase with other developers. 

It allows you to work on your own copies of the code independently before "**merging**" them together, preventing your work from messing up mine.

Let's create our first branch. Name it after yourself; mine will be `owen-mariani`


    $ git branch owen-mariani
    $ git checkout owen-mariani 
    $ git branch

1. We created a branch
2. We switched into the branch
3. We checked what branch we were on
 

The staging area
----------------

Now, let’s try adding a file into the project. 

Let’s create a text file named after you. For me it is `owen.txt` 

Adding, Commiting, and Pushing
----------------

- In Git, you first add content to the **staging area** by using `git add`.

- To make it official **locally** `git commit`. 
- To make it true throughout **upstream** (the whole repo) `git push`.

Adding
----------------
Let’s add the files to the staging area

    $ git add owen.txt

We can see if it worked by checking the `status` of our git repo

    $ git status

Committing
----------

You are now ready to commit. The `-m` flag allows you to enter a message
to go with the commit at the same time.

    $ git commit -m "Added owen.txt"

![](https://upload.wikimedia.org/wikipedia/commons/thumb/4/44/Help-browser.svg/20px-Help-browser.svg.png)
Stuck? Ask for help from the workshop staff

Let’s see what just happened
----------------------------

We should now have a new commit. To see all the commits so far, use
`git log`

    $ git log

The log should show all commits listed from most recent first to least
recent. You would see various information like the name of the author,
the date it was commited, a commit SHA number, and the message for the
commit.

You should also see your most recent commit, where you added the two new
files in the previous section. 

You should see something similar to:

    commit 5a1fad96c8584b2c194c229de7e112e4c84e5089
    Author: owenmariani 
    Date:   Sun Mar 29 16:13:42 2024 +1200

        Added owen.txt

This tells us all we need to know about the commit

![](https://upload.wikimedia.org/wikipedia/commons/thumb/4/44/Help-browser.svg/20px-Help-browser.svg.png)
Stuck? Ask for help from the workshop staff

Undoing
-------

Let's say we committed and we realized it was a mistake, we can easily fix this by doing one of the following:

If we wanted to **go back a certain number of commits**, we can easily just replace 1 with the number we want to go back.

    $ git reset --hard HEAD~1

If there is **a certain commit we want to go back to** we can search for the commit hash and copy and paste it in:
    
    $ git log
    $ git reset --hard <sha1-commit-id>
    
Or if we **just want to look at a previous commit** we can assign it to HEAD doing the following:

    
    $ git checkout <sha1-commit-id>

Merging
-------

We now try out merging. Eventually you will want to merge two branches
together after the conclusion of work.\
`git merge` allows you to do that.

Git merging works by first switching the branch you want to *into*, and
then running the command to merge the other branch in.

We now want to merge our `owen-mariani` branch into `main`. First, switch to
the `main` branch.

    git checkout main

Next, we merge the `owen-mariani` branch into `main` :

    $ git merge owen-mariani

Do you see the following output ?

    Merge made by recursive.
     test.txt |    1 +
     1 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 test.txt

**You have to be in the branch you want merge *into*** and then you always
specify the branch you want to merge.

At this point, you can also try out `gitk` to visualize the changes and
how the two branches have merged

Merge Conflicts
---------------

Git is pretty good at merging automagically, even when the same file is
edited. There are however, some situations where the same line of code
is edited there is no way a computer can figure out how to merge.\
This will trigger a conflict which you will have to fix.

We now practice fixing merge conflicts. Recall that conflicts are caused
by merges which affect the same block of code.

Here’s a branch I prepared earlier. The branch is called `alpher`. Run
the code below to set it up (don’t worry if you can’t understand it)

    $ git checkout alpher

You should now have a new branch called `alpher`. Try merging that
branch into `master` now and fix the ensuing conflict.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/4/44/Help-browser.svg/20px-Help-browser.svg.png)
Stuck? Ask for help from the workshop staff

Fixing a conflict
-----------------

You should see a `conflict` with the `mergeConflict.txt` file. This means that
the same line of text was edited and committed on both the master branch
and the broken branch. The output below basically tells you the current
situation :

    Auto-merging mergeConflict.txt
    CONFLICT (content): Merge conflict in gamow.txt
    Automatic merge failed; fix conflicts and then commit the result.

If you open the `mergeConflict.txt` file, you will see something similar as
below:

    $ cat mergeConflict.txt
    <<<<<<< HEAD
    It was eventually recognized that most of the heavy elements observed in the present universe are the result of stellar nucleosynthesis (http://en.wikipedia.org/wiki/Stellar_nucleosynthesis) in stars, a theory largely developed by Bethe.
    =======

    http://en.wikipedia.org/wiki/Stellar_nucleosynthesis
    Stellar nucleosynthesis is the collective term for the nuclear reactions taking place in stars to build the nuclei of the elements heavier than hydrogen. Some small quantity of these reactions also occur on the stellar surface under various circumstances. For the creation of elements during the explosion of a star, the term supernova nucleosynthesis is used.
    >>>>>>> broken

Git uses pretty much standard conflict resolution markers. The top part
of the block, which is everything between `<<<<<< HEAD` and `======` is
what was in your current branch.\
The bottom half is the version that is present from the `broken` branch.
To resolve the conflict, you either choose one side or merge them as you
see fit.

For example, I might decide to choose the version from the `broken`
branch.

Now, try to **fix the merge conflict**. Pick the text that you think is
better and move them all above the `======`

Once I have done that, I can then mark the conflict as fixed by using
`git add` and `git commit`.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/4/44/Help-browser.svg/20px-Help-browser.svg.png)
Stuck? Ask for help from the workshop staff

    $ git add mergeConflict.txt
    $ git commit -m "Fixed conflict"

Congratulations. You have fixed the conflict. All is good in the world.


