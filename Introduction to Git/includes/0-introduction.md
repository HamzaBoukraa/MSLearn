# Introduction

Imagine that you have started a new job as a software developer at a firm that writes avionics software for commercial airliners. Quality control is critical, and developers work in small teams using [Git](https://git-scm.com/) for version control. You have experience with other version-control systems, but to date, your experience with Git is minimal. To flourish in your new role and add value to the team, you need to get up to speed on Git — and quickly.

So you decide to build a Web site that lets you and your friends share pictures of your cats. The project will serve as a learning platform, and you will apply the lessons learned there at work. You enlist a couple of friends who are also software developers to help out. Together, you set out to build the site using Git to aid in collaboration, keep track of changes (and who makes them), make sure nothing bad happens when two people change the same file, and keep all the source-code files backed up in case the server goes down.

In this module, you accomplish all this and more with Git. Git can seem a little cryptic at first and even be frustrating at times, but if you learn it step by step, you will find that there's a reason it is quickly becoming the world's most popular version-control system — not just for software developers, but for teams who write documentation and produce other collaborative works as well. 

## What is a version-control system?

A [version-control system](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control) (VCS) is a program (or set of programs) that tracks changes to a collection of files. One goal is to easily recall earlier versions of individual files or the entire project. Another is to allow several team members to work on a project, even on the same files, at the same time without impacting each other.

Another name for version-control systems is software configuration management (SCM) systems. The two terms are often used interchangeably — in fact, Git's official documentation is located at [git-scm.com](https://git-scm.com/). Technically, however, version control is just one of the practices involved in SCM, while a VCS can be used for projects other than software including books and online tutorials.

With a version-control system, you can:

- See all the changes made to your project, when the changes were made, and who made them.
- Include a message with every change explaining the reasoning behind it.
- Retrieve past versions of the entire project or individual files.
- Create *branches*, wherein changes can be made experimentally. This allows several different sets of changes (for example, features or bug fixes) to be worked on at the same time, possibly by different people, without impacting the master branch. Later, you can merge the changes you want to keep back into master.
- Attach a tag to a version — for example, to mark a new release.

Git is a fast, versatile, highly scalable, free, open-source version-control system. Its primary author is [Linus Torvalds](https://en.wikipedia.org/wiki/Linus_Torvalds), the creator of Linux. If you're interested, you can learn how Git came about by reading [A Short History of Git](https://git-scm.com/book/en/v2/Getting-Started-A-Short-History-of-Git).

## Distributed version control

Earlier VCSs, such as [CVS](http://www.nongnu.org/cvs/), [Subversion](https://subversion.apache.org/), and [Perforce](https://www.perforce.com/), used a centralized server to store a project's history. This meant that one server was also a single point of failure.

Git is _distributed_, which means that a project's complete history is stored on the client as well as the server. You can edit files without a network connection, check them in locally, and sync with the server when a connection becomes available. If a server goes down, you still have a local copy of the project. Technically, you don't even have to have a server. Changes can be passed around in e-mail or shared using removable media.

## Git terminology

To understand Git, you have to understand the terminology. Here is a short list of terms that Git users frequently use. Don't sweat the details for now; all of these terms will become familiar as you work your way through the exercises in this module.

- **Working tree:** The set of nested directories and files that contains the project being worked on.

- **Repository (repo):** The directory, located at the top level of a working tree, where Git keeps all of the history and metadata for a project. Repositories are almost always referred to as *repos*. A *bare repository* is one that is not part of a working tree; it is used for sharing or backup. A bare repo is usually a directory with a name ending in **.git** — for example, **project.git**.

- **Hash:** A number produced by a [hash function](https://en.wikipedia.org/wiki/Hash_function) that reduces the contents of a file or other object to a fixed number of bits. Git uses [SHA-1 hashes](https://en.wikipedia.org/wiki/SHA-1), which are 160 bits long. One advantage to using hashes is that Git can tell whether a file has changed by hashing its contents and comparing the result to the previous hash. If the file's time-and-date stamp has changed but the file's contents have not, Git knows it.

- **Object:** A Git repo contains four types of "objects," each uniquely identified by an SHA-1 hash. A **blob** object contains an ordinary file. A **tree** object represents a directory; it contains names, hashes, and permissions. A **commit** object represents a specific version of the working tree. A **tag** is a name attached to a commit.

- **Commit:** When used as a verb, "commit" means to make a commit object. This action takes its name from commits to a database. It means you are committing the changes you have made so others can eventually see them, too.

- **Branch:** A named series of linked commits. The most recent commit on a branch is called the *head*. The default branch, which is created when you initialize a repository, is called "master." The head of the current branch is called `HEAD`. Branches are an incredibly useful feature of Git because they allow developers to work independently (or together) in branches and later merge their changes into master.

- **Remote:** A named reference to another Git repository. When you create a repo, Git creates a remote named "origin" that is the default remote for push and pull operations.

- **Commands**, **subcommands**, and **options:** Git operations are performed using commands such as `git push` and `git pull`. `git` is the command, and `push` or `pull` is the subcommand. The subcommand specifies the operation you want Git to perform. Commands are frequently accompanied by options, which are predceded by hyphens (-) or double hyphens (--) — for example, `git reset --hard`. 

Once more, don't get too hung up on the details. These terms and others such as "push" and "pull" will make more sense shortly. But you have to start somewhere, and you might find it helpful to come back and review this glossary terms after you have completed this module.

## The Git command line

There are several different GUIs available for for Git. Examples include [GitKraken](https://www.gitkraken.com/) (which is cross-platform), [TortoiseGit](https://tortoisegit.org/) (Windows), and [git gui](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-gui.html) (Linux). Many program editors such as Microsoft's [Visual Studio Code](https://code.visualstudio.com/) also have an interface to Git. Unfortunately, they all work differently and have different limitations. None of them implement _all_ of Git's functionality.

The exercises in this module use the Git command line — specifically, Git commands executed in a Bash shell. Bash is the default on macOS and Linux, and it's installed automatically by [Git for Windows](https://gitforwindows.org/). The command line works the same no matter what operating system you're using. Plus, the command line lets you tap into *all* of Git's functionality. Developers who see Git only through a GUI sometimes find themselves confronted with error messages they can't resolve, and have to resort to the command line to get going again.

If you use Windows, you should be aware of a couple of peculiarities stemming from Git's Linux heritage. First, commands are case-sensitive. Second, paths use forward slashes ("/") rather than backslashes.

The next step is to install Git on your computer and verify that it's working.