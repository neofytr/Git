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