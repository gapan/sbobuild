*sbobuild* is a tool that can be used to automate building packages from
slackbuilds.org repositories. It takes into account software that is already
packaged and ready to be installed from binary package repos and prefers those
over the corresponding SlackBuilds.

While building packages, it also creates a full source tree as well as a tree
with build log files.

This is only meant to be used in Salix and with the Salix repositories.

Things to do before using it:

* Create a temporary repo somewhere in your HD. This is where to store created
packages. For example in:

```
/home/george/salix/sbo/repo/x86_64/pkg/
```

This should be full populated by CHECKSUMS.md5, PACKAGES.TXT files. You should
initially place a dummy package in there to create the files, otherwise
slapt-get will complain.

* Add this repo to your slapt-getrc:

```
SOURCE=file:///home/george/salix/sbo/repo/x86_64/pkg/:OFFICIAL
```

* Make sure your `/etc/slapt-get/slapt-srcrc` contains the following lines:

```
PKGEXT=txz
PKGTAG=salix
```

* Create the structure of the local repo where packages/sources/logs will be
stored. In this example that will be under
`/home/george/salix/sbo/repo/x86_64` and it will include these directories:

```
├── log
├── pkg
│   ├── metagen
│   └── salix
└── source

```

* Notice the `metagen` script. You have to put it in there. Everything else
are directories.

* Edit the paths to `storagedir_src`, `storagedir_pkg`, `logdir` in the
*sbobuild.conf* file accordingly. In this example it could be
something like:

```
storagedir_src=/home/george/salix/sbo/repo/%s/source
storagedir_pkg=/home/george/salix/sbo/repo/%s/pkg
logdir=/home/george/salix/sbo/repo/%s/log
```

which also accomodates for both i486 and x86_64 repositories. Put "%s"
where you want to replace with "i486" and "x86_64", respectively as in
the above example.

* Fire up sbobuild and provide it with a list of SlackBuilds to build:

```
sudo sbobuild `cat LIST`
```
