● 𝘼𝙒𝙎 𝘿𝙃𝘾𝙋 𝘼𝙉𝘿 𝘿𝙉𝙎 ●

➤ AWS :
 
     ● Create a Default VPC;
     ● Create a subnet in the VPC Range IP (I Will use 172.31.128.0)
     ● Create 2 EC2-Machines (EC2-Server | Ubuntu-Client)
     ● Connect to them via Termius or Putty!
     ● Actions | Networking | Change source/destination check | STOP
         

➤ EC2-Server 
   
     | THIS FIRST STEP IS TO GIVE INTERNET TO THE CLIENT | 
     ● sudo hostnamectl set-hostname (Domain) Example: luxsrv.azores.pt
     ● nano key.pem (the same public key you used before, copy and past here)
     ● chmod 600 key.pem
     ● sudo su -
     ● yum update
     ● yum install iptables-services
     ● iptables -F
     ● iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
     ● iptables -t nat -L -nv ( Check NAT )
     ● service iptables save
     ● nano /etc/sysctl.conf
     ● net.ipv4.ip_forward=1 (Add this in the end of the Documment)
     ● sysctl -p (Check if ip forward is 1)
     ● service network restart
     
➤ Ubuntu-Client
 
     ● sudo hostnamectl set-hostname (Domain) Example: luxcli.azores.pt
     ● apt update
     ● apt upgrade
     ● cd /etc/netplan/
     ● nano 50-cloud-init.yaml (Becarfull editing this documment, dont do TABS!)
     ● (CHECK THE IMAGE ABOVE AND DO THE EXACT SAME THING)
     ● netplan try
     ● netplan apply
     ● ping 1.1.1.1 or 8.8.8.8 to check internet connection!     
![NETPLAN](https://user-images.githubusercontent.com/85712710/147394019-86977df7-4c1e-4177-9885-830b47cc8260.png)

➤ EC2-Server (DNS Set-UP)

     ● yum install bind bind-utils
     ● systemctl enable --now named
     ● systemctl status named
     ● nano /etc/named.conf
          > listen (…) any;
          > listen (…) any;
          > dns(…)enable no;
          > dns(…)tion no;
          > Paste in the end of the File;
             zone "DOMAIN" {
                type master;
                file "/var/named/db.DOMAIN";
             };
             zone "31.172.in-addr.arpa" {
                type master;
                file "/var/named/db.31.172";
             };
      
      ● cd /var/named
      ● nano db.DOMAIN (Example: db.azores.pt) |FORWARD ZONE FILE|
      ● Copy Example from /etc/bind/db.local
      ● Configure the Forward Zone
      ● nano db.31.172 |REVERSE ZONE|
      ● Copy Example from /etc/bind/db.127
      ● Configure the Reverse Zone
      ● systemctl restart named
      ● nano /etc/sysconfig/network-scripts/ifcfg-eth0
          > DNS1: 172.31.128.100
          > DNS2: 8.8.8.8
      ● systemctl restart network

➤ Ubuntu-Client (Set DNS on Netplan)
      
      ● nano /etc/netplan/50-cloud-init.yaml
      ● Change the Google DNS(8.8.8.8) to Server IP(172.31.128.100)
      
➤ Ubuntu-Client DHCP Configuration

      ● apt install isc-dhcp-server
      ● ip a (Check Connections)
      ● nano /etc/default/isc-dhcp-server | INTERFACESv4="CONNECTION" | Exemple "eth1" or "eth2" | 
      ● cd /etc/dhcp/
      ● nano dhcpd.conf
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
      ● systemctl status isc-dhcp-server    
      ● systemctl enable –now isc-dhcp-server
