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