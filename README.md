β πΌππ πΏππΎπ πΌππΏ πΏππ β

β€ AWS :
 
     β Create a Default VPC;
     β Create a subnet in the VPC Range IP (I Will use 172.31.128.0)
     β Create 2 EC2-Machines (EC2-Server | Ubuntu-Client)
     β Connect to them via Termius or Putty!
     β Actions | Networking | Change source/destination check | STOP
         

β€ EC2-Server 
   
     | THIS FIRST STEP IS TO GIVE INTERNET TO THE CLIENT | 
     β sudo hostnamectl set-hostname (Domain) Example: luxsrv.azores.pt
     β nano key.pem (the same public key you used before, copy and past here)
     β chmod 600 key.pem
     β sudo su -
     β yum update
     β yum install iptables-services
     β iptables -F
     β iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
     β iptables -t nat -L -nv ( Check NAT )
     β service iptables save
     β nano /etc/sysctl.conf
     β net.ipv4.ip_forward=1 (Add this in the end of the Documment)
     β sysctl -p (Check if ip forward is 1)
     β service network restart
     
β€ Ubuntu-Client
 
     β sudo hostnamectl set-hostname (Domain) Example: luxcli.azores.pt
     β apt update
     β apt upgrade
     β cd /etc/netplan/
     β nano 50-cloud-init.yaml (Becarfull editing this documment, dont do TABS!)
     β (CHECK THE IMAGE ABOVE AND DO THE EXACT SAME THING)
     β netplan try
     β netplan apply
     β ping 1.1.1.1 or 8.8.8.8 to check internet connection!     
![NETPLAN](https://user-images.githubusercontent.com/85712710/147394019-86977df7-4c1e-4177-9885-830b47cc8260.png)

β€ EC2-Server (DNS Set-UP)

     β yum install bind bind-utils
     β systemctl enable --now named
     β systemctl status named
     β nano /etc/named.conf
          > listen (β¦) any;
          > listen (β¦) any;
          > dns(β¦)enable no;
          > dns(β¦)tion no;
          > Paste in the end of the File;
             zone "DOMAIN" {
                type master;
                file "/var/named/db.DOMAIN";
             };
             zone "31.172.in-addr.arpa" {
                type master;
                file "/var/named/db.31.172";
             };
      
      β cd /var/named
      β nano db.DOMAIN (Example: db.azores.pt) |FORWARD ZONE FILE|
      β Copy Example from /etc/bind/db.local
      β Configure the Forward Zone
      β nano db.31.172 |REVERSE ZONE|
      β Copy Example from /etc/bind/db.127
      β Configure the Reverse Zone
      β systemctl restart named
      β nano /etc/sysconfig/network-scripts/ifcfg-eth0
          > DNS1: 172.31.128.100
          > DNS2: 8.8.8.8
      β systemctl restart network

β€ Ubuntu-Client (Set DNS on Netplan)
      
      β nano /etc/netplan/50-cloud-init.yaml
      β Change the Google DNS(8.8.8.8) to Server IP(172.31.128.100)
      
β€ Ubuntu-Client DHCP Configuration

      β apt install isc-dhcp-server
      β ip a (Check Connections)
      β nano /etc/default/isc-dhcp-server | INTERFACESv4="CONNECTION" | Exemple "eth1" or "eth2" | 
      β cd /etc/dhcp/
      β nano dhcpd.conf
        > #authoritative (Remove de #);
        > subnet x.x.x.x netmask x.x.x.x {
             range x.x.x.x x.x.x.x;
             option domain-name-servers ns1.internal.DOMAIN;
             option domain-name "internal.DOMAIN";
             option subnet-mask x.x.x.x;
             option routers x.x.x.x;
             option broadcast-address x.x.x.x;
             default-lease-time 800;
             max-lease-time 7200;
         }
      β systemctl status isc-dhcp-server    
      β systemctl enable βnow isc-dhcp-server
      β apt install build-essential
      β cd ~ 
      β git clone https://github.com/saravana815/dhtest.git
      β cd dhtest/
      β make
      
      β To check if the DHCP is working, you need to duplicate ubuntu-client session.
      β ./dhtest -m 00:00:11:22:33:44 -i eth0
      β In the duplicated session:
           > tcpdump -i eth0 -nv port 67
