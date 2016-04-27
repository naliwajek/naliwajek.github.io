---
layout: post
title: Git empty commits and what you can use them for
category: Terequireredchnical Notes
---
Why would anyone push empty commits on purpose?

Usually when we hit the time when a commit cannot be made because there are no changes present, we quickly realize our mistake and add missing files.

Not this time.

This time we will run with this option:

~~~ shell
git commit --allow-empty
~~~

This will open your editor and allow you to create a commit with no changes, only message will be requiered.

This is helpful in few situations.

## Triggering hooks

You need to hit a trigger which listens on your remote for incoming pushes, things such as:

* TravisCI and continious integration services in general
* linterns which work per pull request basis
* static analysis tools like Code Climate and so on

In case of weird problems, no _retry_ buttons or what gives, you can always trigger hooks that way.

## Notify on changes in server setup

When you've done a significant change on the server side and need a way to mark that in time and in the codebase, as well as share it with rest of the team.

For example:

~~~ text
[Server Notice] Update PostgreSQL to 9.4.4

Changes:

* Fix possible failure to recover from an inconsistent database state (Robert Haas)

  Recent PostgreSQL releases introduced mechanisms to protect against multixact wraparound, but some of that code did not account for the possibility that it would need to run during crash recovery, when the database may not be in a consistent state.

...
~~~

No need for date since it's stored in the git history.

Now every developer with access to CI will see this commit.

Of course, release notes above were copied from the official Postgres website but if your devops team suspects specific ways in which update can affect your technology stack, by all means put it there!

### Notify multiple repos at once

Now, because that's just a simple git commit which isn't much more than a simple text entry, you can very easily write scripts which will run alongside your update tool and push such commit to multiple repositories at once.

Implementation will vary between teams but it'd be quite easy to add such commit automatically to all the repos of all the apps you have on your server.

### Easy to find

Such information will also be very easy to find. For example, to search for all such server notices you could run:

~~~ text
git log --all --grep='Server Notice'
~~~

### Taggable

If your flow use tags a lot, don't worry. Nothing changes. You can still tag this commit as any other. Once you're done coding, have a `v1.0` version of your app and then finish your server setup, you can do stuff like:

~~~ text
git tag -a v1.0a -m "server setup finished"
~~~

And now you have two points in git tagged: finished codebase and same codebase after server changes which are described by commit messages between `v1.0` and `v1.0a` tags, if that's your game.

### Other options?

Of course, creativity is the limit here. Thing to remember is that commit messages, even in commits with no actual changes whatsoever, are important pieces of information already in your favorite journaling system.

Have any other ideas? Comments? Ideas against this solution? Comment below.
