# Jarkom-Modul-3-I03-2022

Computational Networking Module 3 Practicum Report

* Erlangga Wahyu Utomo 5025201118
* Venia Sollery Aliyya Hasna 5025201161
* Selomita Zhafirah 5025201120


## The Topology :

![Preview](https://github.com/Vsollery/Jarkom-Modul-3-I03-2022/blob/main/Topology_Jarkom-Modul3-I03.jpg)

## Question 1

> Loid with Franky plan to create the map above with criteria WISE as DNS Server, Westalis as DHCP Server, Berlint as Proxy Server (1), and Ostania as DHCP Relay (2).

First We use command : iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.208.0.0/16
.After that we check it with cat /etc/resolv.conf . We use the command in Ostania.

![Preview](https://github.com/Vsollery/Jarkom-Modul-3-I03-2022/blob/main/Check-Ostania.jpg)

After that, we move to Wise and use command : echo nameserver 192.168.122.1 > /etc/resolv.conf \
To make sure that the server is match. Then we apt-get update and apt-get install bind9 -y to start the server as DNS.

![Preview]( https://github.com/Vsollery/Jarkom-Modul-3-I03-2022/blob/main/Updat-Instal-Bind9.jpg)

We use command 'service bind9 start' to start the domain

![Preview]( https://github.com/Vsollery/Jarkom-Modul-3-I03-2022/blob/main/Service-Start-Bind.jpg)

We move to Westalis and do the some command 'echo nameserver 192.168.122.1 > /etc/resolv.conf' 'apt-get update' \
but, in here we instal the dhcp server with command 'apt-get install isc-dhcp-server -y'

![Preview](https://github.com/Vsollery/Jarkom-Modul-3-I03-2022/blob/main/dhcp-server-install.jpg)

Use the command 'echo ‘INTERFACES="eth0" ‘ > /etc/default/isc-dhcp-server' and 'service isc-dhcp-server start' to start the dhcp server

![Preview]( https://github.com/Vsollery/Jarkom-Modul-3-I03-2022/blob/main/interface-eth0-westalis.jpg)

We move to Berlint, update and install the squid. to update use command 'apt-get update' . Install the squid with command 'apt-get install squid -y'

![Preview](https://github.com/Vsollery/Jarkom-Modul-3-I03-2022/blob/main/Install-Squid.jpg)

after that start with command 'service squid start'

![Preview]( https://github.com/Vsollery/Jarkom-Modul-3-I03-2022/blob/main/service-squid.jpg)

## QUESTION 2
> Ostania as DHCP Relay (2).

-Ostania \
Install the dhcp relay with command 'apt-get install isc-dhcp-relay -y' and start the dhcp 'service isc-dhcp-relay start'

![Preview](https://github.com/Vsollery/Jarkom-Modul-3-I03-2022/blob/main/install-dhcp-relay.jpg)
![Preview](https://github.com/Vsollery/Jarkom-Modul-3-I03-2022/blob/main/service-dhcp-relay.jpg)

##  Question 3 & 4
Loid and Franky construct the map carefully and thoroughly.There are several criteria that Loid and Franky want to make, which are:
-All existing clients MUST use the IP configuration from the DHCP Server. \
-Client that go through Switch1 have the IP range from [prefix IP].1.50 - [prefix IP].1.88 and [prefix IP].1.120 - [prefix IP].1.155 (3) \
-Client that go through Switch3 have the IP range from [prefix IP].3.10 - [prefix IP].3.30 dan [prefix IP].3.60 - [prefix IP].3.85 (4)

-Westalis \
echo "
subnet 192.208.1.0 netmask 255.255.255.0 {
    range 192.208.1.50 192.208.1.88;
    range 192.208.1.120 192.208.1.155;
    option routers 192.208.1.1;
    option broadcast-address 192.208.1.255;
    option domain-name-servers 192.208.2.2;
    default-lease-time 300;
    max-lease-time 6900;
}

subnet 192.208.2.0 netmask 255.255.255.0 {
}

subnet 192.208.3.0 netmask 255.255.255.0 {
    range 192.208.3.10 192.208.3.30;
    range 192.208.3.60 192.208.3.85;
    option routers 192.208.3.1;
    option broadcast-address 192.208.3.255;
    option domain-name-servers 192.208.2.2;
    default-lease-time 600;
    max-lease-time 6900;
}
" > /etc/dhcp/dhcpd.conf

After use command above, make sure stop the dhcp server and start again to reload.use command 'service isc-dhcp-server stop' and then 'service isc-dhcp-server start'

![Preview](https://github.com/Vsollery/Jarkom-Modul-3-I03-2022/blob/main/dhcpd.conf.jpg) 


