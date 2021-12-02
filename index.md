# **Cloud VPN Docker Project**
After creating a droplet on Digital Ocean, open the console and begin installing docker.

### **INSTALLING DOCKER**
1) install necessary tools <br>
>-    `sudo apt install apt-transport-https ca-certificates curl software-properties-common -y` <br>

2) Add docker key <br>
>-    `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -` <br>

3) Add docker repo (32 bit / 64 bit OS) <br>
>-   ` sudo add-apt-repository \` <br> `"deb [arch=amd64] https://download.docker.com/linux/ubuntu \` <br> `  $(lsb_release -cs) \` <br> ` stable"` <br>

4) Switch to correct repo <br>
>-   `apt-cache policy docker-ce` <br>

5) Install Docker <br>
>-   `sudo apt install docker-ce -y` <br>

6) Install Docker-Compose <br>
>-   ` sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose` <br>

7) Set Permissions <br>
>-   `sudo chmod +x /usr/local/bin/docker-compose` <br>

END OF DOCKER INSTALL
 
 
Now its time to set up Wireguard

### **WIREGUARD SET UP**
1) Run these commands on the server to create the directories needed and then enter the .yml file 
>-    `mkdir -p ~/wireguard/`
>-    `mkdir -p ~/wireguard/config/`
>-    `nano ~/wireguard/docker-compose.yml`<br>

2) Enter the following into the .yml file and dont forget to edit your timezone, use the proper ip address for your server, as well as set your ports to the desired numbers. Check the **bold** properties for refrence.
  >    version: '3.8' <br>
  >    services: <br>
  >    wireguard: <br>
  >    &nbsp;&nbsp;container_name: wireguard <br>
  >    &nbsp;&nbsp;image: linuxserver/wireguard <br>
  >    &nbsp;&nbsp;environment: <br>
  >    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- PUID=1000 <br>
  >    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- PGID=1000 <br>
  >    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- **TZ=America/Chicago** <br>
  >    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- **SERVERURL=165.232.140.117** <br>
  >    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- **SERVERPORT=80** <br>
  >    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- PEERS=pc1,pc2,phone1 <br>
  >    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- PEERDNS=auto <br>
  >    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- INTERNAL_SUBNET=10.0.0.0 <br>
  >    &nbsp;&nbsp;ports: <br>
  >    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - **80:51820/udp** <br>
  >    &nbsp;&nbsp;volumes: <br>
  >    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - type: bind <br>
  >    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; source: ./config/ <br>
  >    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; target: /config/ <br>
  >    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - type: bind <br>
  >    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; source: /lib/modules <br>
  >    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; target: /lib/modules <br>
  >    &nbsp;&nbsp;restart: always <br>
  >    &nbsp;&nbsp;cap_add: <br>
  >    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - NET_ADMIN <br>
  >    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - SYS_MODULE <br>
  >    &nbsp;&nbsp;sysctls: <br>
  >    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - net.ipv4.conf.all.src_valid_mark=1 <br>
 
 3) Save and exit the file after edit is complete <br>
 
 4) Start Wireguard by using cd to enter the wireguard directory and then run it <br>
 >-   `cd ~/wireguard/`
 >-   `docker-compose up -d` <br>
 
END OF WIREGUARD SETUP

Now its time to connect your phone to Wireguard

### **CONNECT PHONE TO WIREGUARD**
1) Download wireguard app on oyour phone <br>

2) Run this code to access a QR code that you can scan
      docker-compose logs -f wireguard <br>
      
3) Name your tunnel and turn it on 

END OF CONNECT PHONE TO WIREGUARD

Now connect your pc to Wireguard

### **CONNECT COMPUTER TO WIREGUARD**
1) You need to install Wireguard VPN on to your computer from the website <br>
>- `https://www.wireguard.com/install/` <br>


2) Next you need to create a new empty tunnel on Wireguard VPN and add the information found in the configuration file. In order to access this information enter the following commands
>- `cd ~/wireguard/` <br>
> Look at what is inside with the `ls` command and enter into the ***config*** directory <br>
>- `cd config` <br>
> Again use `ls` to look inside the ***config*** directory <br>
> Now choose a user directory. I chose the ***peer_pc2*** directory and went inside <br>
>- `cd peer_pc2` <br> 
> Again use `ls` to look inside the ***peer_pc2*** directory <br>
> Here you will see the configuration file "***peer_pc2.conf***" and look at what is inside with the command <br>
>- `cat peer_pc2.conf` <br> 
> Copy this file's information and paste it into the Wireguard VPN on your computer to create the tunnel. <br>


3) Finish creating the tunnel and activate the VPN for your computer.

END OF CONNECT COMPUTER TO WIREGUARD

## ***Have fun!***
