I want to demo how to prepare a deb for hello world.

Before below steps, [setup tool](https://www.debian.org/doc/manuals/debmake-doc/ch03.en.html) first

1. [source code](#step-1-source-code)
2. [tar it for debmake](#step-2-tar-it-for-debmake)
3. [debianizes the upstream source tree by adding template files](#step-3-debianizes-the-upstream-source-tree-by-adding-template-files)
4. [manual customization](#step-4-manual-customization)
5. [debuild](#step-5-debuild)

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
└── README.md
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
└── README.md
```

# Step 4: manual customization

1. debian/rules
2. debian/control
3. debian/copyright

See diff:

```diff
diff --git a/debhello-0.0/debian/control b/debhello-0.0/debian/control
old mode 100644
new mode 100755
index 9a0670a..cb0152d
--- a/debhello-0.0/debian/control
+++ b/debhello-0.0/debian/control
@@ -1,15 +1,13 @@
 Source: debhello
-Section: unknown
+Section: devel
 Priority: extra
 Maintainer: Tim Chen <timchen@ingrasys.com>
 Build-Depends: debhelper (>=9)
 Standards-Version: 3.9.6
-Homepage: <insert the upstream URL, if relevant>
+Homepage: https://timmy00274672.wordpress.com/
 
 Package: debhello
 Architecture: any
 Multi-Arch: foreign
 Depends: ${misc:Depends}, ${shlibs:Depends}
-Description: auto-generated package by debmake
- This Debian binary package was auto-generated by the
- debmake(1) command provided by the debmake package.
+Description: This is my demo
diff --git a/debhello-0.0/debian/copyright b/debhello-0.0/debian/copyright
old mode 100644
new mode 100755
index 9888328..bae87f4
--- a/debhello-0.0/debian/copyright
+++ b/debhello-0.0/debian/copyright
@@ -1,12 +1,24 @@
 Format: http://www.debian.org/doc/packaging-manuals/copyright-format/1.0/
 Upstream-Name: debhello
-Source: <url://example.com>
+Source: https://salsa.debian.org/debian/debmake-doc
 
-Files:     Makefile
-           src/hello.c
-Copyright: __NO_COPYRIGHT_NOR_LICENSE__
-License:   __UNKNOWN__
-
-#----------------------------------------------------------------------------
-# Files marked as NO_LICENSE_TEXT_FOUND may be covered by the following
-# license/copyright files.
+Files:     *
+Copyright: 2015 Osamu Aoki <osamu@debian.org>
+License:   Expat
+ Permission is hereby granted, free of charge, to any person obtaining a
+ copy of this software and associated documentation files (the "Software"),
+ to deal in the Software without restriction, including without limitation
+ the rights to use, copy, modify, merge, publish, distribute, sublicense,
+ and/or sell copies of the Software, and to permit persons to whom the
+ Software is furnished to do so, subject to the following conditions:
+ .
+ The above copyright notice and this permission notice shall be included
+ in all copies or substantial portions of the Software.
+ .
+ THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS
+ OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
+ MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
+ IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
+ CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
+ TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
+ SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
diff --git a/debhello-0.0/debian/rules b/debhello-0.0/debian/rules
index dca10d1..87a6727 100755
--- a/debhello-0.0/debian/rules
+++ b/debhello-0.0/debian/rules
@@ -1,15 +1,15 @@
 #!/usr/bin/make -f
 # You must remove unused comment lines for the released package.
-#export DH_VERBOSE = 1
-#export DEB_BUILD_MAINT_OPTIONS = hardening=+all
-#export DEB_CFLAGS_MAINT_APPEND  = -Wall -pedantic
-#export DEB_LDFLAGS_MAINT_APPEND = -Wl,--as-needed
+export DH_VERBOSE = 1
+export DEB_BUILD_MAINT_OPTIONS = hardening=+all
+export DEB_CFLAGS_MAINT_APPEND  = -Wall -pedantic
+export DEB_LDFLAGS_MAINT_APPEND = -Wl,--as-needed
 
 %:
    dh $@  
 
-#override_dh_auto_install:
-#  dh_auto_install -- prefix=/usr
+override_dh_auto_install:
+   dh_auto_install -- prefix=/usr
 
 #override_dh_install:
 #  dh_install --list-missing -X.pyc -X.pyo
```

# Step 5: debuild

Run `debuild`, refer to result: [debuild.log](/debuild.log)

Tree:

```
├── debhello-0.0
│   ├── debian
│   │   ├── changelog
│   │   ├── compat
│   │   ├── control
│   │   ├── copyright
│   │   ├── debhello                                    <================ New
│   │   │   ├── DEBIAN
│   │   │   │   ├── control
│   │   │   │   └── md5sums
│   │   │   └── usr
│   │   │       ├── bin
│   │   │       │   └── hello
│   │   │       └── share
│   │   │           └── doc
│   │   │               └── debhello
│   │   │                   ├── changelog.Debian.gz
│   │   │                   ├── copyright
│   │   │                   └── README.Debian
│   │   ├── debhello.debhelper.log                      <================ New
│   │   ├── debhello.substvars                          <================ New
│   │   ├── debhelper-build-stamp                       <================ New
│   │   ├── files
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
│       ├── hello                                       <================ New
│       └── hello.c
├── debhello_0.0-1_amd64.build                          <================ New
├── debhello_0.0-1_amd64.changes                        <================ New
├── debhello_0.0-1_amd64.deb                            <================ New
├── debhello_0.0-1.debian.tar.xz                        <================ New
├── debhello_0.0-1.dsc                                  
├── debhello_0.0.orig.tar.gz
├── debhello-0.0.tar.gz
├── debuild.log                                         <================ output of debuild.log
└── README.md
```

------
# Reference

[Chapter 4. Simple Example](https://www.debian.org/doc/manuals/debmake-doc/ch04.en.html)