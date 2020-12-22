Patch for Linux kernel modifying read/write family syscalls to allow buffering file changes before committing.
This was an Advanced Topics in Operating Systems assignment #2 in 2020 at MIMUW faculty.

The patch for kernel 5.5.5 is contained in wb-linux-5.5.5.patch.
The solution does not modify mmap, sendfile and some other syscalls.
The commit is not atomic.
It works with (p)read(v)/(p)write(v) family.
