# Error handling

## Errno and error handling

- C library mechanism for reporting errors is errno <errno.h>
- See [errno.h]([ (opengroup.org)](https://pubs.opengroup.org/onlinepubs/009695399/basedefs/errno.h.html)) for list of posix defined errors - ENOENT means "no such file or directory"
- Use "errno -l" from the command line to dump error values and names
- 