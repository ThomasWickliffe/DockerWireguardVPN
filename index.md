After creating a droplet on digital ocean, open the console and begin downloading docker.

INSTALLING DOCKER
1) install necessary tools
    sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
2) Add docker key
     curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
3) Add docker repo (32 bit / 64 bit OS)
     sudo add-apt-repository \
     "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
       $(lsb_release -cs) \
   stable"
4) switch to correct repo
      apt-cache policy docker-ce
5) Install Docker
      sudo apt install docker-ce -y
6) Install Docker-Compose
      sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
7) Set Permissions
      sudo chmod +x /usr/local/bin/docker-compose
 END OF DOCKER INSTALL
 
 
Now its time to set up Wireguard

WIREGUARD SET UP
1) Run this code on the server to create the directories needed and then enter the .yml file 
      mkdir -p ~/wireguard/
      mkdir -p ~/wireguard/config/
      nano ~/wireguard/docker-compose.yml
2) Enter the following into the .yml file and dont forget to edit your timezone, the proper ip address for your server, as well as set your ports to the desired numbers
      version: '3.8'
      services:
      wireguard:
         container_name: wireguard
         image: linuxserver/wireguard
         environment:
            - PUID=1000
            - PGID=1000
            - TZ=America/Chicago
            - SERVERURL=
            - SERVERPORT=80
            - PEERS=pc1,pc2,phone1
            - PEERDNS=auto
            - INTERNAL_SUBNET=10.0.0.0
          ports: 165.232.140.117
            - 80:51820/udp
          volumes:
            - type: bind
              source: ./config/
              target: /config/
            - type: bind
              source: /lib/modules
              target: /lib/modules
          restart: always
          cap_add:
            - NET_ADMIN
            - SYS_MODULE
          sysctls:
            - net.ipv4.conf.all.src_valid_mark=1
 
 3) Save and exit the file after edit is complete
 4) Start Wireguard by using cd to enter the wireguard directory and then run it
      cd ~/wireguard/
      docker-compose up -d
END OF WIREGUARD SETUP

Now its time to connect your phone to wireguard

CONNECT PHONE TO WIREGUARD
1) Download wireguard app on oyour phone
2) Run this code to access a QR code that you can scan
      docker-compose logs -f wireguard
3) Name your tunnel and turn it on 
END OF CONNECT PHONE TO WIREGUARD
