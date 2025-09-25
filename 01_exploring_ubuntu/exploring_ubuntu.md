David Rivas  
Professor Hoffmann  
CPS593-10 Network Security  
25 September 2025  

# Assignment 4

## Identify Network Interfaces and IP Addresses

Identifying network interfaces and IP addresses on a server can help administrators understand its network configuration.

```sh
# Display all network interfaces and their associated IP addresses
ip a 
# OR 
ifconfig
```
![Pasted image 20250925163510](./img/Pasted%20image%2020250925163510.png)
![Pasted image 20250925163535](./img/Pasted%20image%2020250925163535.png)

## Check Open Ports

By listing open ports on the server and the services listening on them can help identify unnecessary open ports that can be potential attack surfaces. 

```sh
# Lists all network connections, routing tables, interface stats, masquerade connections, and multicast memberships
sudo netstat -tuln 
# OR 
ss -tuln
```
![Pasted image 20250925163818](./img/Pasted%20image%2020250925163818.png)
![Pasted image 20250925163842](./img/Pasted%20image%2020250925163842.png)

## Analyze Network Connections

Analyzing network connections can consist of listing open network connections, which helps identify unauthorized connections to the server.

```sh
# Lists open network connections and files, including associated processes
# `-i` flag: lists all network files and their associated processes
# `-P` and `-n` flags: prevent the resolution of port numbers and IP addresses
sudo lsof -i -P -n
```
![Pasted image 20250925164143](./img/Pasted%20image%2020250925164143.png)

## Perform Network Scanning with Nmap

Nmap (Network Mapper) is a network scanning tool commonly used to discover hosts and services on a network.

```sh
# Network scanning tool to identify open ports, running services, and the operating system
# `-sS` option: perform a stealth TCP SYN scan
# `-O` option: attempts to determine the OS of the target
sudo nmap -sS -0 localhost
```
![Pasted image 20250925164321](./img/Pasted%20image%2020250925164321.png)

## Check for Open Ports on the Server's Network

Identifying the open ports on a server's network ultimately identifies all live hosts on the local network. This lists the devices present in the network and ensures there are no unauthorized connections.

```sh
# Identify live hosts on local network
# -sP: Ping scan: discovers hosts on a network without performing port scan
sudo nmap -sP 192.168.1.0/24
```
![Pasted image 20250925193859](./img/Pasted%20image%2020250925193859.png)
![Pasted image 20250925193915](./img/Pasted%20image%2020250925193915.png)

## Check for Services and Versions

Checking a server for services and their versions present helps identify outdated or vulnerable software. This consists of scanning open ports and determining what services are running and their respective versions.

```sh
# `-sV` option: enables version detection
sudo nmap -sV localhost
```
![Pasted image 20250925190154](./img/Pasted%20image%2020250925190154.png)

## Identify Potential Vulnerabilities

Nmap contains vulnerability scanning scripts to identify known vulnerabilities on a server. This can find common security issues in a server's installed software.

```sh
# `--script vuln`: runs scripts that check for various vulnerabilities
sudo nmap --script vuln localhost
```
![Pasted image 20250925190408](./img/Pasted%20image%2020250925190408.png)
![Pasted image 20250925190509](./img/Pasted%20image%2020250925190509.png)

## Inspect Network Traffic

Inspecting a server's network traffic on a specific network interface can help administrators observe real-time traffic and detect malicious activity.

```sh
# Capture network traffic on an interface namedÂ `eth0`
sudo tcpdump -i eth0

# Find interface names
ip link show

# Capture network traffic on `ens160`, a common interface name pattern for virtualized Ubuntu systems
sudo tcpdump -i ens160
```
![Pasted image 20250925190938](./img/Pasted%20image%2020250925190938.png)

## Monitor Network Connections in Real-Time

Monitoring network connections in real-time can consist of running a specified command at regular intervals with the `watch` command. In this example, `netstat` is ran every second to monitor network connections.

```sh
# `-n`: number of seconds at which network connections are monitored
sudo watch -n 1 netstat -tulnp
```
![Pasted image 20250925191411](./img/Pasted%20image%2020250925191411.png)

## Check Firewall Rules

`ufw` (Uncomplicated Firewall) is a front-end tool used to manage iptables and make it easier for an administrator to configure its firewall.

```sh
# `verbose` option: provides a detailed view of firewall configuration
sudo ufw status verbose
```
![Pasted image 20250925191507](./img/Pasted%20image%2020250925191507.png)