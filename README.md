# gsw-hooks

A collection of git hooks that might be useful for GNUstepWeb hackers.

##pre-commit

Currently just tries to build the app. If it can't, it rejects the commit.

##post-commit

Kills any running instance of the app and (builds and) runs a new instance. This is for developer systems to have the current running version on; I'd imagine you would want to be less ham-fisted on a production server (and you'd want to use a post-update hook).