# File I/O

## File I/O in Linux

- files are opend via file descriptor(fd)
  - represented by an integer
- Kernel maintains a Per-process list of files -file table
  - list indexed by file descriptor

- Every process has at least 3 file descriptors open
  - stdin(fd 0 or STDIN_FILENO)
    - terminal input device
  - stdout(fd 1 or STDOUT_FILENO)
    - terminal display
  - stderr(fd 2 or STDERR_FILENO)
    - terminal display

## Opening and Accessing Files

- open() (possibliy with O_CREATE)

- read()
- write()
- close()

:question: what about fopen, fwrite, fread?

- "f" prefix relate to buffered I/O, not system calls
- non "f" prefix versions listed here are the underlying system calls.

:star: fopen vs open()

- bufferd io uses FILE*
- syscall open() uses file descriptor

- located in different man page sections

- openat() is eventually called from fopen()

:question: What's the difference between fopen() and open()?

- f prefix functions from <stdio.h> provide platform-independent user-buffering

:question: What is file buffering?

- Rather than reading / writing a full block, read from or write to copies in memory
- Improves performance.

:question: What do we mean by user-buffering

- Happens in userspace, rather than in the kernel
- Reduce the amount of system calls into the kernel

### Opening files

- open()
  - maps pathname to file descriptor
  - include open flags argument
    - access - (read only, read/write create, append, synchronous I/O, etc)
  - mode argument used when creating
    - read/write/execute permissions for user/group/other



### Reading file content

- read()
- returns:
  - Number of bytes read, or -1 on error(and set errno)
  - return values < count mean no more bytes are available now (end of file, interrupted by signal)
- Read the first byte of a file
- create the file with zero size if it doesn't exist

### Create File and Umask

:question: What happend to the write permission?

- leared by umask

:question: What is umask?

- Per-process way to restrict permissions set on created files( & ~umask )

:question: what happens if i change my umask to 0003?

- Now both w and x are cleared

### System call Logging with strace

- strace is a handy tool for debugging application interaction with the kernal through syscall
- no source code required!

### Non-Blocking Reads

- O_NONBLOCK allows your thread needs to do work while waiting for fifo / pipe data

### Synchronized I/O

:question: Is data written to disk when write() completes successfully?

- No - the writer is into a kernel buffer

:question: Why?

- Disks are slow!

:question: When is this a problem?

- unclean shutdown

:question: How can we manage the risk?

- fsync(), fdatasync(), O_SYNC
- Configure disk write caching
- avoid sync() for performance reasons