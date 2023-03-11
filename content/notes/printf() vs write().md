---
date: 05/03/2023
---
[[C]]

The first big difference between these two is that `write()` is a system call and `printf()` is a C function. A system call is a function provided by the kernell and write is just a regular C library function.

The other big difference between these two is that `write()` is *only* meant to write a sequence of bytes.
On the other hand, `printf()` allows you to ouptut your data into a formated sequence of bytes and then calls `write()` to print those bytes. 