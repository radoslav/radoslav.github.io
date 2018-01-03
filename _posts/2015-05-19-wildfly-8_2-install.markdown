---
layout:     post
title:      "Installing and configuring WildFly 8.2 as service (GNU/Linux)."
subtitle:   "WildFly (JBoss AS) is a java application server"
date:       2015-05-19 16:00:00
author:     "radoslav"
header-img: "img/post-bg-03.jpg"
---

## Install wildfly 8.2 as service

### script for install wildfly

info: [https://gist.github.com/sukharevd/6087988](https://gist.github.com/sukharevd/6087988)

```bash
wget https://gist.githubusercontent.com/sukharevd/6087988/raw/efcab01424e739f509a0537728ab3764d3d4fa26/wildfly-install.sh -O wildfly-install.sh
```

### Cut section echo "Configuring application server..."

Because I prefer manual configuration.
Install wildfly and run as service

```bash
bash wildfly-install.sh
```

### set to automatically started every time

```bash
chkconfig wildfly on --level 35
```

## Configure Wildfly (standalone)

### Add new management user

```bash
/opt/wildfly/bin/add-user.sh
```

### Generate new certificate

What is your first and last name? = must be a DNS server name (server.company.pl)
add kyestore to /opt/wildfly/standalone/configuration

```bash
JAVA_HOME/bin/keytool -genkey -alias server -keyalg RSA -keystore server.keystore -validity 3650
```

### Configuration file (standalone)

/opt/wildfly/standalne/configuration/standalone.xml

### Secure the Management-Console

add keystore to ManagementRealm (security-realm name="ManagementRealm")

```xml
            <security-realm name="ManagementRealm">
                  <server-identities>
                    <ssl>
                        <keystore path="server.keystore" relative-to="jboss.server.config.dir" keystore-password="secret"/>
                    </ssl>
                  </server-identities>
		...........
           <security-realm name="ManagementRealm">
```

Replace Soccet-Binding from http to https ( management-interfaces -> http-interface security-realm="ManagementRealm" http-upgrade-enabled="true"

```xml
	<socket-binding https="management-https"/>
```

remove unsecure Management-Binding

```xml
	<!-- <socket-binding name="management-http" interface="management" port="${jboss.management.http.port:9990}"/> -->
```

### add SslRealm

```xml
            <security-realm name="SslRealm">
                <server-identities>
                    <ssl>
                         <keystore path="server.keystore" relative-to="jboss.server.config.dir" keystore-password="secret"/>
                    </ssl>
                </server-identities>
            </security-realm>
```

add https-listener https (subsystem xmlns="urn:jboss:domain:undertow:1.2" -> <server name="default-server">)

```xml
	<https-listener name="default-ssl" socket-binding="https" security-realm="SslRealm"/>
```

edit http-listner, add redirect http to https (subsystem xmlns="urn:jboss:domain:undertow:1.2" -> <server name="default-server">)

```xml
	<http-listener name="default" socket-binding="http" redirect-socket="https"/>
```


### Listen for "any-address" on interfaces

Edit section

```xml
	<interfaces>
		<interface name="management">
 			<any-address/>
		</interface>
		<interface name="public">
			<any-address/>
		</interface>
		<interface name="unsecure">
			<any-address/>
		</interface>
	</interfaces>
```

### Set UTF-8 Encoding

change line from:

```xml
	<servlet-container name="default">
```

to:

```xml
	<servlet-container name="default" use-listener-encoding="true" default-encoding="UTF-8">
```

## PREROUTING port 80 and 443

Wildfly can not open port 80 and 443 because is not running as root.

```bash
iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080
iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 443 -j REDIRECT --to-port 8443
```

Show PREROUTING

```bash
	iptables -t nat -L
```

Save PREROUTING

```bash
	service iptables save
```

### Save on ubuntu 16.04

```bash
sudo apt-get install iptables-persistent
sudo netfilter-persistent save
sudo netfilter-persistent reload
```
