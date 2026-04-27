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
