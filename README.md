# minecraft-server-tuto

This tutorial will show you how to set up your own Minecraft server on Amazon Web Services (AWS). We are using an **Infrastructure as Code (IaC)** approach with AWS CloudFormation. This means the entire server and its firewall rules are created automatically using a single code file.

The script automatically installs the correct Java version and downloads the Minecraft server files, so all you have to do is start the game!

## Prerequisites

1. An **AWS account**
2. **Your Public IP Address and your friends**, so we can restrict the firewall access strictly to you and your friends.

### How to find your IP address
Open your terminal (Mac/Linux) or PowerShell (Windows) and type:
```
curl ifconfig.me
```
Remember to add /32 after your address

## Uploading Cloudformation template to AWS

Before uploading the template, this template uses t3.small instance. I recommend to switch t3.medium or better for smoother experience, by simply editing the template file and changing "InstanceType" value to your favourable instance.

Also for newer minecraft version for the server, go to minecraft server site and copy the download link and paste it to templates script part and change the url after the wget.

1. Login to AWS Console and go to cloudformation.
2. Press "Create stack" -> "With new resources (standard)".
3. Select "Upload a template file" and choose the file (cloudformationec2.yaml) and press "next".
4. Write own stack name and put your own IP address on the parameters box. Remember to put /32 after your address.
5. You can click next and go straight to "submit"

## Starting the server

When the stack is completed, you can navigate next to ec2 service via consoles search. 
Then:
1. Click your running instance
2. Click "Connect" and choose "SSM Session Manager" and connect. (This tutorial doesn't create SSH key pair, so we use session manager, and using SSM is good practice).
3. Next use these commands:
   ```
   sudo su - ec2-user

   cd minecraft/
   ```
   
   ```
   # This enables to keep your minecraft server running after you close session manager window
   screen -S minecraft
   ```

   ```
   # Start your server 
   ./start.sh
   ```
   When server is running and you want to keep it running after closing SSM do:
   CTRL + A and immediately press D to detach

   When you want to stop the minecraft server (not the instance) go to SSM and do:
   ```
   sudo su - ec2-user

   cd minecraft/

   screen -r minecraft

   stop
   ```

## Adding your friends to your server

1. Navigate to security groups and select security group that the stack created.
2. Go to inbound rules -> edit inbound rules.
3. "Add rule" -> Type: Custom TCP, port: 25565, source: friends IP address
4. Then save rules


# Author
Aapo Hampaala

   
   

