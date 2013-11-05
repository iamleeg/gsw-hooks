# gsw-hooks

A collection of git hooks that might be useful for GNUstepWeb hackers.

##pre-commit

Builds the app, and runs the tests (via `make check`). If any of that
fails, it aborts the commit. GNUstep-make provides a default
do-nothing implementation of `make check` so this is safe to have in
your hooks even if you don't have any tests yet. Well, to the extent
that _THAT_ is safe.

##post-commit

Kills any running instance of the app and (builds and) runs a new
instance. This is for developer systems to have the current running
version on; I'd imagine you would want to be less ham-fisted on a
production server (and you'd want to use a `post-update` hook).

##post-update

The `post-update` hook requires a little setup to use. We're going to
have two systems: `server` and `client`. There will be three repos
(not counting any offsite things you have like remotes on GitHub or
BitBucket):

 1. The server's main repository: `server:/path/webapp.git`
 2. The server's deployment repository: `server:/path/deploy`
 3. Your development repository: `client:/home/developer/webapp`

To set up, do the following things on `server` (in addition to setting
up the necessary users, SSH daemon, GSW itself etc):

    $ cd /path
    $ mkdir webapp.git
    $ cd webapp.git
    $ git init --bare
    $ git pull some-repo master
    $ [deploy post-update from here to hooks/post-update]
    $ cd ..
    $ git clone webapp.git deploy

Now on the client you can do:

    $ [check out the source to ~/webapp]
    $ git remote add server user@server:/path/webapp.git
    $ [edit test edit test edit test]
    $ git push -u server master

The act of pushing will build the app on the server, bring it down and
launch the new version. You _did_ run the tests in your pre-commit
hook, right?

### Limitations

Currently `post-update` doesn't integrate with any GNUstep
configuration, for example the server adapter. Therefore if you
require multiple instances of the same app, you shouldn't use this
script as it will kill all the instances then just start one.

It's perhaps a little unkind just to kill a server and restart it, a
more friendly handover mechanism would be useful.

Pull requests to fix either of these are welcome.