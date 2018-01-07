# Lecture 14

> CS:APP, Chapter 10

## Input/Output (I/O)

This is the process of copying data between main memory and external devices such as disk drives, terminals and networks.

### Unix I/O

A Linux *file* is a sequence of *m* bytes: *B<sub>0</sub>, B<sub>1</sub>, ..., B<sub>k</sub>, ..., B<sub>m - 1</sub>*.

**All I/O devices, not just disks, are modeled as files**! This goes for things like networks and terminals also. This enables all input and output to be performed in a uniform and consistent way.

### Files

Each Linux file has a *type* that indicates its' role in the system.

- A *regular file* contains arbitrary data. This could for example be a *text file* which is a regular file that contains only ASCII or Unicode characters, or *binary files*, which are everyting else. **To the kernel there is no difference between text and binary files**. A Linux text file consists of a sequence of text lines, where each line is a sequence of characters terminated by a *newline* character.
- A *directory* is a file consisting of an array of *links*, where each link maps a *filename* to a file, which may be another directory!

There are plenty of other file types that isn't covered in this chapter.

## Reading File Metadata

The `stat` and `fstat` functions retrieve information about a file.

## I/O Redirection

I/O redirection operators in Linux shells allow users to associate standard input and output with disk files:

```s
ls > foo.txt
```

This causes the shell to load and execute the `ls` program with standard output redirected to disk file `foo.txt`.