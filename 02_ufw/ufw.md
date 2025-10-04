# Ubuntu UFW (Uncomplicated Firewall)

**I. Enable UFW**

1. We can check the status of the Uncomplicated Firewall (UFW) using this command:
```sh
# Check status of UFW
sudo ufw status
```
![Pasted image 20251003201408](./img/Pasted%20image%2020251003201408.png)

2. Before we enable UFW, we must ensure the connection to SSH is allowed using this command:
```sh
# Allow traffic through port 22
sudo ufw allow 22/tcp
```
![Pasted image 20251003201426](./img/Pasted%20image%2020251003201426.png)

If we are remotely accessing an Ubuntu server, it's important to allow traffic through port 22 before enabling UFW because by default, UFW denies all incoming connections. Port 22 is important for establishing a connection to the server via SSH.

3. We can list open sockets (network connections) on the server using this command:
```sh
# List open sockets on the server
# `ss` option: check network statistics
# `-t`: show TCP sockets
# `-u`: show UDP sockets
# `-l`: show listening sockets
# `-n`: show numeric addresses
sudo ss -tuln
```
![Pasted image 20251003201445](./img/Pasted%20image%2020251003201445.png)

4. We can now enable UFW
```sh
# Enable UFW
sudo ufw enable
```
![Pasted image 20251003201510](./img/Pasted%20image%2020251003201510.png)

5. Let's check the status of UFW
```sh
# Check status of UFW
sudo ufw status
```
![Pasted image 20251003201523](./img/Pasted%20image%2020251003201523.png)

6. If this server is a web server, we must allow specific ports through the UFW:
	* SSH (port 22)
	* HTTP (port 80)
	* HTTPS (port 443)
```sh
# Allow ports needed for a web server through the UFW
sudo ufw allow 22/tcp # SSH
sudo ufw allow 80/tcp # HTTP
sudo ufw allow 443/tcp # HTTPS
```
![Pasted image 20251003201540](./img/Pasted%20image%2020251003201540.png)

7. We can also check the status using the verbose command:
```sh
sudo ufw status verbose
```
![Screenshot 2025-10-03 at 7.28.48 PM](./img/Screenshot%202025-10-03%20at%207.28.48%20PM.png)

The `verbose` command gives extra details of the UFW configuration such as default policies, logging status and IPv6 status.

8. Imagine a scenario in which we must block an IP address, 10.0.0.0, from the server using UFW. This is how it would be done:
```sh
# Block a network address from UFW
sudo ufw deny from 10.0.0.0/8
```
![Pasted image 20251003201724](./img/Pasted%20image%2020251003201724.png)

9. If we want to allow 192.168.1.50 access to port 587, this is how it would be done:
```sh
# Allow 192.168.1.50 access to port 587
sudo ufw allow from 192.168.1.50 to any port 587
```
![Pasted image 20251003201739](./img/Pasted%20image%2020251003201739.png)

Port 587 is the submission port (SMTP), a standard port for sending outgoing email securely.

**II. Enable UFW Logging**

1. We can enable UFW logging using the following command:
```sh
# Enable UFW logging
sudo ufw logging on
```

2. If we want more details in the `verbose` log, we can change the logging level:
* **low**: Minimal logging, mainly for blocked incoming packets
* **medium**: Includes blocked incoming packets with additional packet header details
* **high**: Includes all blocked packets and connection information
* **full**: Extensive logging of all UFW events
```sh
# High detail verbose logs
sudo ufw logging high
```
![Pasted image 20251003202105](./img/Pasted%20image%2020251003202105.png)

3. Each UFW log entry contains the following information:
	* `MAC`: Mac address
	* `SRC`: Source IP address
	* `DST`: Destination IP address
	* `SPT`: Source port
	* `DPT`: Destination port
	* `[UFW BLOCK]`: indicates packet was blocked by UFW

The UFW log entry is useful because it gives details of what traffic is being blocked or allowed on the Ubuntu server. This could help troubleshoot connectivity, spot malicious activity, and fine-tune firewall rules.

4. UFW logs, stored in `/var/log/ufw.log`, can be found using this command:
```sh
# Print UFW log
# `-f` option: monitor logs real-time
sudo tail -f /var/log/ufw.log
```
![Screenshot 2025-10-03 at 7.34.53 PM](./img/Screenshot%202025-10-03%20at%207.34.53%20PM.png)

5. Specific entries in the UFW log can be filtered using the `grep` command.
```sh
# Filtering denied traffic
sudo grep 'DENY' /var/log/ufw.log

# Filtering allowed traffic
sudo grep 'ALLOW' /var/log/ufw.log
```
![Pasted image 20251003202255](./img/Pasted%20image%2020251003202255.png)

There is no output for DENY because, by default, UFW drops (denies) packets without logging them.