# Branch and merge

As your project progresses, the developers want to work on more than one task at a time, fixing bugs while implementing new features. They need a way to keep their work separate so what one person is doing doesn't affect another.

_Branches_ make this easy. The work done "on a branch" doesn't have to be shared, and it doesn't interfere with other branches. Branches let you keep the commits related to each topic together and in isolation, making changes easy to review and track. Modern software development is done almost entirely in branches. The goal is to keep "master" clean until the work is ready to check in. Then you push your changes to "master," or better yet, submit a pull request for the merge.

One of Git's advantages over older version-control systems is that creating a branch is extremely fast; it amounts to writing a 40-character hash into a file under ".git/heads." Switching branches is also fast, because Git stores whole files and just unzips them rather than trying to reconstruct them from lists of changes. Merging in Git isn't _quite_ that simple, but it's straightforward and often completely automatic. Let's learn what branches are, how they're used, and how they work.

## Understanding branches

A _branch_ is simply a chain of commits "branching off" from the main line of development, like a branch on a tree.

If you are switching to Git from another version-control system, you may be accustomed to slightly different terminology. Subversion, for example, calls its main branch "trunk". Git calls it "master." You can rename "master," just as you can rename any other branch, and some teams do this when switching to Git from other version-control systems.

A branch usually starts with a commit on "master." It grows a separate history chain as commits are added. Eventually its changes can be merged back into "master." You will learn to do that shortly.

Suppose you branch off of "master." Here's how to visualize what happens:

```
master:  A---B---C---D
              \
branch:        E---F---G
```

Each capital letter in the diagram represents a commit. Branches are given names such as "add-authentication" and "fix-css-bug," and branches can have branches of their own. The ultimate goal is to let developers do what they need to do without stepping on one another, and to wind up with a "master" branch representing the best efforts of everyone involved.

## Create a branch for Alice

Alice wants to add some CSS to style the cat pictures, so she creates a _topic branch_ (sometimes called a _feature branch_) and calls it "add-style." Let's assume the role of Alice, create the branch, and do some work in that branch.

1. Navigate back to the "Alice" directory. Then use the [`git branch`](https://git-scm.com/docs/git-branch) command to create a branch named "add-style," and the [`git checkout`](https://git-scm.com/docs/git-checkout) command to switch to that branch (make it the *current branch*):

	```bash
	cd ../Alice
	git branch add-style
	git checkout add-style
	```

	You have already encountered `checkout` as a way of replacing files in the working tree by getting them from the index. With no paths in the argument list, `checkout` updates *everything* in the working tree and the index to match the specified commit — in this case, the head of the branch.

1. Open **site.css** in the "Alice/assets" directory and add the following CSS class definition to the bottom of the file:

	```css
	.cat { max-width: 40%; padding: 5 }
	```

1. Save the changes to the file and commit the change:

	```bash
	git commit -a -m "Add style for cat pictures"
	```

1. At this point, Alice wants to make her style available to everyone else, so she switches back to "master" and does a pull in case anyone else has made changes:

	```bash
	git checkout master
	git pull
	```

1. The output says that "master" is up to date (in other words, "master" on Alice's computer matches "master" in the shared repo), so Alice merges the "add-style" branch into "master" using `git merge --ff-only` to perform a [fast-forward merge](https://ariya.io/2013/09/fast-forward-git-merge). Then she pushes "master" from her repo to the shared repo:

	```bash
	git merge --ff-only add-style
	git push
	```

Performing a fast-forward merge because "master" had no changes wasn't strictly necessary in this case because Git would have done it anyway. Still, it's a good habit to get into because an `--ff-only` merge fails if "master" has changed, making you acutely aware that changes have occurred.

## Create a branch for Bob

While Alice is working on the CSS, Bob is sitting in an apartment on the other side of town blissfully unaware of what Alice is doing (which is OK since they're both using branches). Bob decides to make some changes of his own.

1. Return to the "Bob" directory and use the following command to create a branch named "add-cat,", using the popular `checkout -b` option to create the branch and switch to it in a single command:

	```
	cd ../Bob
	git checkout -b add-cat
	```

1. Copy **bobcat2-317x240.jpg** from the zip file containing the [resources that accompany this lesson](https://topcs.blob.core.windows.net/public/git-resources.zip) into Bob's "assets" directory.

1. Now open **index.html** and replace the line that says "Eventually we will put cat pictures here" with the following statement, and then save the file:

	```html
	<img src="assets/bobcat2-317x240.jpg">
	```

1. You have now made two changes to Bob's "add-cat" branch: You have added one file and modified another. Use the following commands to add the new file in the "assets" directory to the index and commit all changes: 

	```bash
	git add assets
	git commit -a -m "Add picture of Bob's cat"
	```

1. Bob now does as Alice did: he switches back to "master" and does a pull to see if anything has changed:

	```
	git checkout master
	git pull
	```

	This time, the output indicates that changes *have* been made to "master" in the shared repo (the result of Alice's push). It also indicates that the change pulled from "master" in the shared repo have been merged with "master" in Bob's repo:

	```
	remote: Counting objects: 4, done.
	remote: Compressing objects: 100% (3/3), done.
	remote: Total 4 (delta 1), reused 0 (delta 0)
	Unpacking objects: 100% (4/4), done.
	From D:/Labs/Git/Bob/../Shared
	   e81ae09..1d2bfea  master     -> origin/master
	Updating e81ae09..1d2bfea
	Fast-forward
	 assets/site.css | 3 ++-
	 1 file changed, 2 insertions(+), 1 deletion(-)
	```

1. Now Bob merges his branch into "master" so that "master" in his repo will have his *and* Alice's changes. Then he pushes "master" on his computer to "master" in the shared repo: 

	```bash
	git merge add-cat
	git push
	```

Bob did not use the `--ff-only` option because he was aware that "master" had changed. A fast-forward-only merge would have failed.

## Sync the repos

At this point, Bob has an up-to-date repo, but you and Alice do not. Each of you needs to a do a `git pull` from the shared repo to make sure you have the latest and greatest version of the site.

1. Use the following commands to sync Alice's repo with the shared repo: 

	```bash
	cd ../Alice
	git pull
	```

1. Now do the same for your repo: 

	```bash
	cd ../Cats
	git pull
	```

Take a moment to verify that your repo, Alice's repo, and Bob's repo are all synced. Each of them should have a JPG file in the "assets" directory and an `<img>` element declared in **index.html**. In addition, the **site.css** file in each repo's "assets" folder should contain a line defining a CSS style named `cat` (the style that Alice added when she made her changes).

Moreover, if you open **index.html** in any of the repos, you should see this:

![There be cats!](media/first-cat.png)

_There be cats!_

## Summary

In this lesson, you learned how to create branches and how to merge them. You learned about:

- [`git branch`](https://git-scm.com/docs/git-branch), which creates a branch
- [`git checkout`](https://git-scm.com/docs/git-checkout), which switches to a branch, and [`git checkout -b`](https://git-scm.com/docs/git-checkout) which creates a branch *and* switches to it
- [`git merge`](https://git-scm.com/docs/git-merge), which combines branches

In the next lesson, you learn how to resolve merge conflicts, which occur when changes made by two or more developers overlap.
