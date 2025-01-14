# About Version Control

Version control is a system that records to a file or set of files over time so that you can recall specific versions later. 

It allows you to revert selected files back to a previous state, revert the entire project back to a previous state, compare changes over time, see who last modified something that might be causing a problem, who introduced an issue and when, and more. 

Using a version control system also generally means that if you screw things up or lose files, you can easily recover. In addtion, you get this all for very little overhead.

# What is Git?

Git is a distributed version control system. Even though Git's user interface is fairly similar to other distributed version control systems, Git stores and thinks about information in a very different way.

## Snapshots, Not Differences

The major differences between Git and any other VCS is the way Git thinks about it's data. 

Conceptually, most other systems store information as a list of file-based changes. These other systems think of the information they store as a set of files and the changes made to each file over time (this is commonly described as *delta-based* version control).

Git doesn't think of or store data this way. Git thinks of data more like a series of snapshots of a miniature filesystem. With Git, every time you commit, or save the state of your project, Git basically takes a picture of what all your files look like at that moment and stores a reference to that snapshot. To be efficient, Git doesn't store the file again, just a link to the previous identical file it has already stored. Git thinks about it's data more like a **stream of snapshots**.

This is an important distinction between Git and nearly all other VCSs. It makes Git reconsider almost every aspect of version control that most other systems copied from the previous generation. This makes Git more like a mini filesystem with some incredibly powerful tools built on top of it, rather than simply a VCS.

## Nearly Every Operation is Local

Most operations in Git need only local files and resources to operate - generally no information is needed from another computer on your network. If you're used to CVCS where most operations have that network latency overhead, this aspect of Git will make you think that the gods of speed have blessed Git with unworldly powers. Because you have the entire history of the project right there on your local disk, most operations seem almost instantaneous.

For example, to browse the history of the project, Git doesn’t need to go out to the server to get the history and display it for you — it simply reads it directly from your local database. This means you see the project history almost instantly. If you want to see the changes introduced between the current version of a file and the file a month ago, Git can look up the file a month ago and do a local difference calculation, instead of having to either ask a remote server to do it or pull an older version of the file from the remote server to do it locally.

This also means that there is very little you can't do if you're offline or off VPN. If you get on an airplane or a train and want to do a little work, you can commit happily (to your local copy, remember?) until you get to a network connection to upload. If you go home and can't get your VPN client working properly, you can still work. In many other systems, doing so it either impossible or painful. 

## Git has Integrity

Everything in Git is checksummed before it is stored and is then referred to by that checksum. This means it's impossible to change the contents of any file or directory without Git knowing about it.

This functionality is built into Git at the lowest levels and is integral to it's philosophy. You can't lose information in transit or get file corruption without Git being able to detect it.

The mechanism that Git uses for this checksumming is called a SHA-1 hash. This is a 40-character string composed of hexadecimal characters (0-9 and a-f) and calculated based on the contents of a file or directory structure in Git.

You will see these hash values all over the place in Git because it uses them so much. In fact, Git stores everything in its database not by file name but by the hash value of its contents.

## Git Generally Only Adds Data

When you do actions in Git, nearly all of them only *add* data to the Git database. It is hard to get the system to do anything that is not undoable or to make it erase data in any way. As with any VCS, you can lose or mess up changes you haven't committed yet, but after you commit a snapshot into Git, it is very difficult to lose, especially if you regularly push your database to another repository.

This makes using Git a joy because we know we can experiment without the danger of severely screwing things up.

## The Three States

Git has three main states that your files can reside in: *modified*, *staged*, and *committed*:

1. Modified means that you have changed the file but have not committed it to your database yet.
2. Staged means that you have marked a modified file in it's current version to go into your next commit snapshot.
3. Committed means that the data is safely stored in your local database.


This leads us to the three main sections of a Git project: the working tree, the staging area, and the Gir directory (repository).

The working tree is a single checkout of one version of the project. These files are pulled out of the compressed database in the Git directory (repository) and placed on disk for you to use or modify.

The staging area is a file, generally contained in your Git directory, that stores information about what will go into your next commit. It's technical name in Git parlance is the "index", but the phrase "staging area" works just as well.

The Git directory is where Git stores the metadata and object database for your project. This is the most important part of Git, and it is what is copied when you *clone* a repository from another computer.

The basic Git workflow goes something like this:

1. You modify files in your working tree.
2. You selectively stage just those changes you want to be part of your next commit, which adds *only* those changes to the staging area.
3. You do a commit, which takes the files as they are in the staging area and stores that snapshot permanently to your Git directory.

If a particular version of a file is in the Git directory, it's considered *committed*. If it has been modified and was added to the staging area, it is *staged*. And if it was changed since it was checked out but has not been staged, it is *modified*.

## Git Configuration

Git comes with a tool called `git config` that lets you get and set configuration variables that control all aspects of how Git looks and operates. These variables can be stored in three different places:

1. `[path]/etc/gitconfig` file: Contains values applied to every user on the system and all their repositories. If you pass the option `--system` to `git config`, it reads and writes from this file specifically. Because this is a system configuration file, you would need administrative or superuser privilege.
2. `~/.gitconfig` or `~/.config/git/config` file: Values specific personally to you, the user. You can make Git read and write to this file specifically by passing the `--global` option, and this affects *all* of the repositories you work with on your system.
3. `config` file in the Git directory (that is, `.git/config`) of whatever repository you're currently using: Specific to that single repository. You can force Git to read from and write to this file with the `--local` option, but that is in fact the default. Unsurprisingly, you need to be located somewhere in a Git repository for this option to work properly.

Each level overrides values in the previous levels, so values in `.git/config` trump those in `[path]/etc/gitconfig`.

## Your Identity

The first thing you should do when you install Git is to set your name and email address. This is important because every Git commit uses this information, and it's immutably baked into the commits you start creating:

```bash
git config --global user.name "Raj Shukla"
git config --global user.email "rajisbornforcoc@gmail.com"
```

Again, you need to do this only once if you pass the `--global` option, because then Git will always use that information for anything you do on that system. If you want to override this with a different name or email address for specific projects, you can run the command without the `--global` option when you're in that project.