# lsof Command Line Tips And Tricks

## Introduction

**lsof** is a command line utility for all Unix and Linux like operating systems to check “list of open files”. It is mainly used to retrieve information about files that are opened by various processes. Open files in a system can be of different type like disk files, network sockets, named pipes, devices,...

Many of the processes or devices that **lsof** can report on belong to `root` or were launched by `root`, so you will need to use the **sudo** command with **lsof**.

## General Use Cases

Running command without any options will list all open files of your system that belongs to all active process.

```bash
lsof
```

You can list only the processes which opened a specific file, by providing the filename as arguments.

```bash
lsof /var/log/syslog
```

It becomes very handy in a situation where `df` and `du` command shows different disk usage of same file system, using **lsof** command we can find files which were removed while they were opened and used by some processes.

```bash
lsof /var/log | grep -i "deleted"
```

Above command will give you the pid of files which were deleted but they are still exist in the system in deleted state. So, to release the space from file system we can safely kill the process by its pid.

To list the processes which opened files under a specified directory, we use `+D` option. `+D` will recurse the sub directories also. If you don’t want **lsof** to recurse, then use `+d` option.

```bash
lsof +D /var/log/
```

You can list the files opened by process names starting with a string, using ‘-c’ option. You can give multiple -c switch on a single command line.

```bash
lsof -c ssh -c init
```

To display all the opened files for the respective user, we use `-u` option.

```bash
lsof -u tttinh
```

To list all open files except a user, use ^(caret symbol) in front of user name.

```bash
lsof -u ^tttinh
```

To list all the files opened by a specific process, use `-p` option.

```bash
lsof -p 1234
```

When you want to kill all the processes which has files opened by a specific user, you can use `-t` option to list output only the process id of the process, and pass it to kill as follows

```bash
kill -9 `lsof -t -u tttinh`
```

By default when you use more than one list option in lsof, they will be ORed. For example:

```bash
lsof -u tttinh -c haha
```

The above command uses two list options, `-u` and `-c`. So the command will list process belongs to user `tttinh` as well as process name starts with `haha`.

But when you want to list a process belongs to user `tttinh` and the process name starts with `haha`, you can use `-a` option.

```bash
lsof -u tttinh -c haha -a
```

## Network Related Use Cases

```bash
# List all the network connections opened.
lsof -i
```

```bash
# List only IPV4.
lsof -i4
```

```bash
# List only IPV6
lsof -i6
```

```bash
# List all the network files which is being used by a process 1234
lsof -i -a -p 1234
```

```bash
# List the open files which are listening on port 25
lsof -i :25
```

```bash
# List open files for port 1-1024
lsof -i :1-1024
```

```bash
# List all the TCP or UDP connections
lsof -i tcp
lsof -i udp
```

```bash
# List all TCP connections on port 80
lsof -i tcp:80
```

```bash
# Find the IPv4 socket file of IP 192.168.1.3
lsof -i@192.168.1.3
```

To find an IP version 6 socket file by an associated numeric colon-form address that has a run of zeroes in it – e.g., the loop-back address(127.0.0.1) use below command and options:

```bash
lsof -i@[::1]
```
