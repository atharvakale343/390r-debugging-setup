# 390r-debugging-setup

-   Link to `p7zip` repository: https://github.com/jinfeihan57/p7zip

## Build target

```bash
# Clone target and update Makefile with debugging flags
git clone git@github.com:jinfeihan57/p7zip.git
cp 7zip_gcc_dbg.mak p7zip/CPP/7zip/7zip_gcc.mak

# Compile target and update path
cd p7zip/CPP/7zip/Bundles/Alone2 && make -f makefile.gcc && cd -
PATH=$PATH:$PWD/p7zip/CPP/7zip/Bundles/Alone2/_o/bin
```

## Run target

### List of commands

```bash
7zz -h
```

### Simple tests

```bash
cd playground
7zz a files.zip file1.txt file2.txt
7zz e files.zip -ofiles_extracted
```

## Target analysis

### File format

![File format](screenshots/file_format.png)

### Mitigations

![Checksec mitigations](screenshots/checksec_mitigations.png)

### ROP Gadgets

![List of ROP Gadgets](screenshots/rop_gadgets.png)

### One Gadgets

![List of One Gadgets](screenshots/one_gadgets.png)

### Function call graph

The following can be used to analyze execution of the target and produce graphs. It requires `valgrind` and `kcachegrind` to be installed.

```bash
valgrind --callgrind-out-file=callgrind_vis2 --tool=callgrind 7zz e files.zip -ofiles_extracted
kcachegrind callgrind_vis2
```

Below are two call graphs produced for the archive and extract commands:

![Zip files](screenshots/func_call_graph1.png)

![Extract files](screenshots/func_call_graph2.png)

# Installing AFL++

Install [american-fuzzy-lop-clang](https://github.com/AFLplusplus/AFLplusplus).