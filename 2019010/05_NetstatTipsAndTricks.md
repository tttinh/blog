# Netstat Command Line Tips And Tricks

## Introduction

Netstat is a command line network statistics tool that is used for checking your network configuration and activity.

- It displays both incoming and outgoing network connections, routing tables, network interface and network protocol statistics.
- It is very important tool for network administrators for finding problems in the network and to determine the amount of traffic on the network as a performance measurement.
- It provides the following statistics:
  - The name of the protocol TCP or UDP.
  - The IP address of the local system with used port number. The name of the local system with name of the port.
  - The IP address and port number of the remote system with connected socket.
  - The possible states such as CLOSE_WAIT, ESTABLISHED, CLOSED, FIN_WAIT_1, FIN_WAIT_2, LISTEN, SYN_RECEIVED, SYN_SEND, LAST_ACK, and TIME_WAIT.

## Some simple use cases

| Options     | Description |
| ----------- | ----------- |
| -a          | List all connections |
| -at         | List out only TCP connections |
| -au         | List out only UDP connections |
| -ant        | Show IP address without reverse DNS lookup |
| -l          | List all listening conditions |
| -lt         | List only listening TCP ports |
| -lu         | List only listening UDP ports |
| -lu         | List only listening UDP ports |
| -s          | Display summary statistics |
| -st         | Display TCP summary statistics |
| -su         | Display UDP summary statistics |
| -F          | Displays Domain Name Where Possible for IP Address |
| -tnl        | List Only Listening TCP Connections |
| -unl        | List Only Listening UDP Connections |
| -r          | Show Kernel’s Network Routing Table |
| -rn         | Display Kernel Routing Information |
| -ie         | Print Network Interfaces |

## More advance use cases

Display all Open connections to port 1234

```bash
netstat -anp | grep ":1234"
```

Show Active/Established Connections

```bash
netstat -atnp | grep ESTA
```

Get Continuous List of Active Connections

```bash
watch -d -n0 "netstat -atnp | grep ESTA"
```

Check if a Service is Running

```bash
sudo netstat -aple | grep ntp
```

Here are a bunch of security-oriented netstat commands. Some of them are useful in bringing small-scale DOS attacks under control.
Display IPs with High Number of Connections

```bash
netstat -tn 2>/dev/null | grep :80 | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -nr | head
```

IP Addresses Connected to Port 80

```bash
netstat -tn 2>/dev/null | grep ':80 ' | awk '{print $5}' |sed -e 's/::ffff://' | cut -f1 -d: | sort | uniq -c | sort -rn | head
```

Display Number of Active Connections on Port 80

```bash
netstat -an |grep :80 |wc -l
```

Displays Foreign IP Addresses Only

```bash
netstat -antu | grep :80 | grep -v LISTEN | awk '{print $5}'
```

The below command will output how many active SYNC_REC are occurring and happening on the server. The number should be low (less than 5). If the number is in double digits, you may be suffering a DoS attack or being mail bombed.

```bash
netstat -n -p|grep SYN_REC | wc -l
```

Like the above command, this command too lists all unique IP addresses of the node that are sending SYN_REC connection status

```bash
netstat -n -p | grep SYN_REC | awk '{print $5}' | awk -F: '{print $1}'
```

Connections Per Remote IP

```bash
netstat -antu | awk '{print $5}' | awk -F: '{print $1}' | sort | uniq -c | sort -n
```

or

```bash
netstat -antu | awk '$5 ~ /[0-9]:/{split($5, a, ":"); ips[a[1]]++} END {for (ip in ips) print ips[ip], ip | "sort -k1 -nr"}'
```

Check Open Ports (both ipv4 and ipv6)

```bash
netstat -plntu
```

Check Open Ports (both ipv4 and ipv6)

```bash
netstat -plnt
```

Number of Open Connections per IP

```bash
netstat -an | grep 80 | wc -l
```

Active Internet Connections

```bash
netstat -pnut -w | column -t -s $'\t'
```
