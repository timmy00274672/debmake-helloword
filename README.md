I want to demo how to prepare a deb for hello world.

# Step 1: source code

Tree:

```
debhello-0.0/
debhello-0.0/src/
debhello-0.0/src/hello.c
debhello-0.0/Makefile
```

# Step 2: tar it for `debmake`

`tar zcvf debhello-0.0.tar.gz debhello-0.0/`

tree
```
├── debhello-0.0
│   ├── Makefile
│   └── src
│       └── hello.c
├── debhello-0.0.tar.gz   <=================== This one
├── README.md
└── result.html
```

# Step 3: debianizes the upstream source tree by adding template files

```shell
cd debhello-0.0
debmake
```

Result:
```
bmc@bmc-R2-1208R-EV:~/workspace/debmake-doc$ cd debhello-0.0
bmc@bmc-R2-1208R-EV:~/workspace/debmake-doc/debhello-0.0$ debmake
I: set parameters
I: sanity check of parameters
I: pkg="debhello", ver="0.0", rev="1"
I: *** start packaging in "debhello-0.0". ***
I: provide debhello_0.0.orig.tar.gz for non-native Debian package
I: pwd = "/home/bmc/workspace/debmake-doc"
I: $ ln -sf debhello-0.0.tar.gz debhello_0.0.orig.tar.gz
I: pwd = "/home/bmc/workspace/debmake-doc/debhello-0.0"
I: parse binary package settings:
I: binary package=debhello Type=bin / Arch=any M-A=foreign
I: analyze the source tree
I: build_type = make
I: scan source for copyright+license text and file extensions
I: 100 %, ext = c
I: check_all_licenses
I: ..
I: check_all_licenses completed for 2 files.
I: bunch_all_licenses
I: format_all_licenses
I: make debian/* template files
I: single binary package
I: debmake -x "1" ...
I: creating => debian/control
I: creating => debian/copyright
I: substituting => /usr/share/debmake/extra0/rules
I: creating => debian/rules
I: substituting => /usr/share/debmake/extra0/changelog
I: creating => debian/changelog
I: substituting => /usr/share/debmake/extra1/compat
I: creating => debian/compat
I: substituting => /usr/share/debmake/extra1/watch
I: creating => debian/watch
I: substituting => /usr/share/debmake/extra1/README.Debian
I: creating => debian/README.Debian
I: substituting => /usr/share/debmake/extra1source/local-options
I: creating => debian/source/local-options
I: substituting => /usr/share/debmake/extra1source/format
I: creating => debian/source/format
I: substituting => /usr/share/debmake/extra1patches/series
I: creating => debian/patches/series
I: run "debmake -x2" to get more template files
I: $ wrap-and-sort
bmc@bmc-R2-1208R-EV:~/workspace/debmake-doc/debhello-0.0$
```

Tree

```
├── debhello-0.0
│   ├── debian                  <================ New
│   │   ├── changelog
│   │   ├── compat
│   │   ├── control
│   │   ├── copyright
│   │   ├── patches
│   │   │   └── series
│   │   ├── README.Debian
│   │   ├── rules
│   │   ├── source
│   │   │   ├── format
│   │   │   └── local-options
│   │   └── watch
│   ├── Makefile
│   └── src
│       └── hello.c
├── debhello_0.0.orig.tar.gz    <================ Backup for you
├── debhello-0.0.tar.gz
├── README.md
└── result.html
```