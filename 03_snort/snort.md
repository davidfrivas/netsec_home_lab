# Snort IPS

**I. Update the System**

Ensure the Ubuntu environment is up to date before installing Snort.

```sh
sudo apt update
sudo apt upgrade -y
```

**II. Install Snort**

Snort can be installed directly using `apt`.

```sh
sudo apt install snort -y
```

![Screenshot 2025-10-28 at 8.40.21 PM](./img/Screenshot%202025-10-28%20at%208.40.21%20PM.png)

Snort is installed to the `/etc/snort` directory.

![preview](./img/preview.jpeg)

We first need to determine the network interface we want Snort to monitor.

```sh
ip a
```

**III. Configure Snort**

We can configure Snort using the `snort.conf` configuration file.

```sh
sudo nano /etc/snort/snort.conf
```

![Screenshot 2025-10-28 at 8.38.58 PM](./img/Screenshot%202025-10-28%20at%208.38.58%20PM.png)

Then we can monitor the `ens160` network interface.

![Screenshot 2025-10-28 at 8.42.39 PM](./img/Screenshot%202025-10-28%20at%208.42.39%20PM.png)

**IV. Update and Manage Snort Rules**

Snort comes pre-installed with rules by default. We can download community rules which can enhance threat detection.

```sh
sudo wget https://www.snort.org/downloads/community/community-rules.tar.gz
sudo tar -xvzf community-rules.tar.gz
sudo cp community-rules/* /etc/snort/rules/
```

Additionally, we can add our own rules by manually editing the local rule file.

```sh
sudo nano /etc/snort/rules/local.rules
```

![Screenshot 2025-10-28 at 9.04.25 PM](./img/Screenshot%202025-10-28%20at%209.04.25%20PM.png)

In the example above, a rule was added to detect ICMP (Ping) traffic.

There should now be various rule files in the `/etc/snort/rules/` directory.

![Screenshot 2025-10-28 at 9.01.25 PM](./img/Screenshot%202025-10-28%20at%209.01.25%20PM.png)
Some notable rule files are:
- `ddos.rules`: Denial of service attacks
- `exploit.rules`: Known exploits and vulnerabilities
- `scan.rules`: Port scans and network reconnaissance

**V. Test Snort Configuration**

Now that Snort is configured, we can run a configuration test.

```sh
sudo snort -T -c /etc/snort/snort.conf
```

![Screenshot 2025-10-28 at 9.06.37 PM](./img/Screenshot%202025-10-28%20at%209.06.37%20PM.png)

**VI. Running Snort in IDS Mode**

We can then run Snort in IDS mode to monitor traffic on `ens160`. To exit, use `^C`.

```sh
sudo snort -c /etc/snort/snort.conf -i eth0
```

![Screenshot 2025-10-28 at 9.09.34 PM](./img/Screenshot%202025-10-28%20at%209.09.34%20PM.png)

**VII. Viewing Snort Logs**

Snort log alerts are found in the `/var/log/snort/` directory.

![Screenshot 2025-10-28 at 10.34.18 PM](./img/Screenshot%202025-10-28%20at%2010.34.18%20PM.png)

**VIII. Running Snort as a Daemon**

The following command will run Snort in the background as a daemon, monitoring `ens160`.

We can verify what processes are currently running in the `top` command.

![Screenshot 2025-10-28 at 9.17.45 PM](./img/Screenshot%202025-10-28%20at%209.17.45%20PM.png)

The Snort process can be killed by executing the following:

```sh
sudo killall snort
```
![Screenshot 2025-10-28 at 9.18.47 PM 1](./img/Screenshot%202025-10-28%20at%209.18.47%20PM%201.png)