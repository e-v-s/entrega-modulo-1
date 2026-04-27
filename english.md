# Executive Summary

This document presents a mapping of the company's corporate network and outlines the risks identified throughout the study.
Using methodologies already established in the market, three networks were found: infra_net, corp_net, and guest_net. The first contains six devices, the second contains four, and the third also contains four devices.

## General network risks:

- The networks communicate with each other (very high risk).

- There is no zone where servers can be placed, such as a DMZ (very high risk).

## Main risks found in the infra_net network:

- Servers can be accessed from the guest network: Very high risk.

- There is a legacy server on the same network: Medium-high risk, because a legacy system is not updated nor has security patches, thus containing well-known vulnerabilities. What makes the risk high is that having such a server on the same network as other important servers makes lateral movement of malware between machines very likely.

- Zabbix server with default login and password (login: Admin, password: zabbix): High risk, because any user who knows the default login and password for this type of system can gain access to the company's monitoring system with full privileges. An attacker could manipulate log data to hide intrusions.

- Database server (MySQL) with administrator password matching the login (login: root, password: root): Extremely severe risk, because this user has full privileged access to the company's database.

- Samba server with anonymous connection: Medium/High risk, because through this connection it was possible to view the password settings for this server. The password settings are not compatible with cybersecurity standards, making the system vulnerable to brute-force password cracking attacks.

## Risks found in the corp_net network:

- Devices can be accessed from the guest network.

## Risks found in the guest_net network:

- Device names identify users: facilitates social engineering attacks.

- Two devices have the model name listed: facilitates the use of attack tools specific to that model.

# Objective

To analyze the simulated network in order to identify exposure, segmentation, and existing risks in the infrastructure and the systems found.

# Scope

Virtualized environment running LinuxLite with a Docker environment simulating three networks.

# Methodology

Using the `analyst` machine, an initial reconnaissance of this machine's network interfaces was performed using `ifconfig` to list the available network addresses. With the IP addresses of the machine's interfaces, pings were sent between the network "switches" as follows:

    # ping -c4 -I switch-interface-1 switch-interface-2

This process was repeated until all interfaces had been tested bidirectionally, in order to continue building the network map.

To build the network map and discover whether devices communicate across networks, IP addresses corresponding to the device being tested were added to an interface of the `analyst` machine, and then an ICMP packet was sent, as follows:

    # ip addr add test-ip-of-a-device-on-switch-X-network dev eth1
    # ip addr add test-ip-of-a-device-on-switch-Y-network dev eth2

To test whether devices on different networks communicate, pings were sent between them using the IPs, as follows:

    # ping -c 4 -I test-ip-of-a-device-on-switch-X-network test-ip-of-a-device-on-switch-Y-network

These results were used to build the network diagram, giving insight into whether the network has a firewall, DMZ, network segmentation, etc. After the test, the IPs were deleted from the interface as follows:

    # ip addr del test-ip-of-a-device-on-switch-X-network dev eth1
    # ip addr del test-ip-of-a-device-on-switch-Y-network dev eth2

For IP analysis, the Nmap tool was used to scan each network address to find connected devices.

    # nmap -sn -T4 network-address/subnet-mask

For each device found using the command above, Rustscan was used to check for open ports and running services. The command used was (where ip1,ip2,...,ipn is the selection of IPs of the discovered devices):

    # rustscan -a ip1,ip2,...,ipn

For devices with open ports, login attempts were made on the system/server using:
- Default login + default password
- Most common default logins + passwords matching the login

# Asset Inventory

The following assets were inventoried:

| Networks | IPs | MAC | Devices |
|----------|-----|-----|---------|
| 10.10.30.0/24 | 10.10.30.10<br>10.10.30.11<br>10.10.30.15<br>10.10.30.17<br>10.10.30.117<br>10.10.30.227<br>10.10.30.2 | 7A:28:B1:0F:C7:35<br>5A:3C:C8:CC:8B:84<br>6A:75:46:80:C0:1E<br>1E:9D:EC:AB:BD:D7<br>EA:3B:D2:C5:BD:0F<br>06:DA:AD:F0:BB:94<br> | FTP Server<br>MySQL Server<br>Samba Server<br>OpenLDAP Server<br>Zabbix Server<br>Legacy Server<br> |
| 10.10.10.0/24 | 10.10.10.10<br>10.10.10.101<br>10.10.10.127<br>10.10.10.222<br>10.10.10.2 | FE:76:D9:6A:2F:B0<br>8E:B6:4E:86:DB:B4<br>EA:5A:DE:86:5F:70<br>AE:4F:64:D2:F3:AF<br> | WS_001<br>WS_002<br>WS_003<br>WS_004<br> |
| 10.10.50.0/24 | 10.10.50.2<br>10.10.50.3<br>10.10.50.4<br>10.10.50.5<br>10.10.50.6 | 7E:00:91:09:DC:0C<br>66:7F:CD:DB:04:E5<br>C6:31:AB:FC:8D:BD<br>52:2E:B9:14:B6:C9<br> | laptop-vastro<br>macbook-aline<br>laptop-carlos<br>laptop-luiz<br> |

# Findings and Evidence

## 1 Network

The results of the `ifconfig` command show three networks:
- `10.10.30.0/24` – infra_net
- `10.10.10.0/24` – corp_net
- `10.10.50.0/24` – guest_net

![Figure 1 - Results of the `ifconfig` command on the "analyst" machine](https://github.com/e-v-s/entrega-modulo-1/blob/main/01.png)

It was also found that devices, even when on different networks, communicate with each other. This is a serious problem.

## 1.1 Network Diagram

![](https://github.com/e-v-s/entrega-modulo-1/blob/main/02.png)

### 1.1.1 Risks

- Devices communicate with each other, even when on different networks.
- There is no DMZ for servers with external contact.
- The monitoring server should be isolated.
- The legacy server should be isolated.
- The corporate network should be an internal network (10.10.10.0/24).
- The guest network should be an external network (10.10.50.0/24).
- The ability to ping using the method described in the methodology proves that the network does not go through firewalls.

## 2 Network 10.10.30.0/24 – infra_net

The scan result returned six IP addresses (each one a device), as shown in Figure 2 below.

