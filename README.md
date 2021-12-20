- 𝘼𝙒𝙎 𝘿𝙃𝘾𝙋 𝘼𝙉𝘿 𝘿𝙉𝙎 -


➤ First of all, in aws create a default VPC, it will be enough for this project.

![default vpc](https://user-images.githubusercontent.com/85712710/146819084-f9565722-f2b0-49e1-9c60-3dc46e2dd7ee.png)

➤ Go to subnets and create one in the range of IP's of your Deafult VPC.
  
      ● Choose the default VPC ↓      
![Screenshot_1](https://user-images.githubusercontent.com/85712710/146819470-c684ffa7-f68b-4839-bf08-961510d603bd.png)
      
      ● Subnet Settings  ↓
      
![Screenshot_2](https://user-images.githubusercontent.com/85712710/146820177-6f51651b-70d2-4445-8c2d-1702b0ecbf8d.png)

➤ Next go to EC2 (Virtual Servers)
          
           ● Launch Instance;
           ● I Will be using Amazon Linux (Red Hat Variant) as a Server
           ● t2.micro should be enough
           ● Subnet and Add device Steps Above ↓ , follow it and come back
           ● Size of the disk i will use 30, because its the free tier limit.
           ● Advance to security groups, and add your home IP to all traffic.
           ● then review and launch option, create your keys if you dont have and your server should be ready.
           ● Name it as EC2-SERVER
           
➤ Next step should look like this image ↓ , be careful to choose the default in the same zone where you create your subnet.

![Screenshot_3](https://user-images.githubusercontent.com/85712710/146825227-be52e9de-7d51-46c6-841d-3755d19df38e.png)
       
  
➤ Click Add Device ↓

![Screenshot_4](https://user-images.githubusercontent.com/85712710/146825651-d2bde713-14c8-4759-9eef-3ae572a14419.png)

➤ Then choose the subnet that you have create and put the gateway IP of that new device.

![Screenshot_5](https://user-images.githubusercontent.com/85712710/146825917-49372d6f-12d9-436c-829b-4006e1d5e963.png)
![Screenshot_6](https://user-images.githubusercontent.com/85712710/146826027-f45e7243-e3d1-44d4-be9c-3726ce84c9cf.png)

➤ Create Client now.
          
          ● Launch Instance; 
          ● I Will be using Ubuntu 20.04 LTS (Debian Variant) as a Client
          ● t2.micro should be enough
          ● Subnet Above ↓ , follow it and come back
          
 ➤ Client 

![Screenshot_7](https://user-images.githubusercontent.com/85712710/146827104-2577f90a-d010-4f68-9e21-3342b4b1fadc.png)
![Screenshot_8](https://user-images.githubusercontent.com/85712710/146827273-5a1c61b9-6fcd-4df8-9285-187852af01ce.png)


 
