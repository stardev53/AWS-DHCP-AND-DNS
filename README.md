● 𝘼𝙒𝙎 𝘿𝙃𝘾𝙋 𝘼𝙉𝘿 𝘿𝙉𝙎 ●

➤ AWS :
 
     ● Create a Default VPC;
     ● Create a subnet in the VPC Range IP (I Will use 172.31.128.0)
     ● Create 2 EC2-Machines (EC2-Server | Ubuntu-Client)
     ● Connect to them via Termius or Putty!
     ● Stop Destination Source/Check 
         

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
