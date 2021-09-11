# What Are The Differences Between lsof And netstat In Linux

**lsof** is a tool for listing open files. It lists all open file handles in the kernel which may include sockets (used for interprocess communications) and TCP and UDP connections.

**netstat** is a tool to look at network statistics. You can also use it to look at open network connections, and, with the version in most Linux distributions, to associate those with processes. It also has options to give you a lot of other network-related information that’s not available in lsof, though, such as the routing table, the status of the network interfaces, and so on.

You’ll find both of these on many other Unix-like systems. Since **lsof** was created fairly recently, and was designed to be highly portable, it’ll generally act the same on all of them, it is more generic and standards compliant. **netstat**, however, dates back to the days when there were major differences among different Unix systems, so options for it can differ greatly on different systems.

As an example, you can find the PID of the process listening on port 8086 with **netstat**:

```bash
netstat -tunlp | grep :8086
```

and then use **lsof** to list the files used by the process:

```bash
lsof -p PID
```
