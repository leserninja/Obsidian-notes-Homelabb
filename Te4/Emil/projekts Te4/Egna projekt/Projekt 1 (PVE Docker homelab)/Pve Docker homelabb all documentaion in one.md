> [!important]
[[Links]]

# Vad vill jag ha med ? 

- ###### InfraManagement? (Proxmox, Cosmos)
- ###### Glance dashboard
- ###### file sharing
- ###### Networking/ VPN 
- ###### Simpel MediaManagement server
- ###### Downloaders for media server
- ###### to have control or begin able to see status over all nodes at home
- ###### automerade Ai agenter för att få in live info till ett och samma ställe 
- ###### Rclone MEGA files (might connect to google drive)
- ###### nginx reverse proxy server
 - ###### Something with dns (blocky, Pi-hole or adguard most likely)
 - ###### Home assistant (annan form utav dashboard)
 - ###### Knoledgebase connected to an ai ??? 
 -  (Mail automation??)
 - ###### simpel health checks / monitoring connecting to the dashbord (uptime kuma, Beszel)  
 - ###### * (PhotoManagement for grandma)
 - ###### * New bitwarden vault [Security] 
 ---
## Check lista

[[To Do List]]
#### first steps
- [x] InfraManagement? (Proxmox, Cosmos) ✅ 2026-10-12
- [x] Glance dashboard (--inprog) -- (Up and running only needs configuring) http://100.75.94.81:8080/home ✅ 2026-10-12
- [ ]  file sharing (mabye)
- [ ] Rclone MEGA files (might connect to google drive)
- [ ] omv? or truenas ?


#### networking stuff
- [x] nginx reverse proxy server 🔼 ✅ 2026-10-12
- [x] Something with dns (blocky, Pi-hole or adguard most likely) (thru Tailscale) ✅ 2026-10-12
- [x] tailscale installed on pve ✅ 2026-10-12
- [x] simpel health checks / monitoring connecting to the dashbord (uptime kuma, Beszel) ✅ 2026-10-12
#### media
- [ ] Simpel MediaManagement server
- [ ]   Downloaders for media server
- [ ] (PhotoManagement for grandma) / needs a nas first
#### fun general stuff
- [ ]   to have control or begin able to see status over all nodes/enheter at home
- [ ]   automerade Ai agenter för att få in live info till ett och samma ställe ?
- [ ]   Home assistant (annan form utav dashboard)
- [ ]   Knoledgebase connected to an ai ??? (wiki)
- [ ]   (Mail automation??)
- [ ]   New bitwarden vault [Security] 

----
# Hur/ vad jag gör 

## [[Början]]
Börjar med att flasha en usb sticka med en fräsh proxmox eftersom att den som jag hade på den usb stickan var en dålig verision som inte var up till datum.

Därefter gick jag med installationen som är väldigt enkel med deras grafiska installations guide. O.B.S man får inte glömma att sätta den på sitt egna subnet som man har, som exempel 192.168.[0].x då i detta fall nollan representerar ett subnät.

Sedan routerns ip och gateway vilket i mesta fall brukar vara: 192.168.0.1
Detta är viktigt för att servern och routern ska kunna kommunicera med varandra, så att du också kan komma in i den grafiska webb UI. 

Sedan har jag satt up alla inställningar som jag behövde för att börja min första vm med en docker container i. detta valde jag för att jag ska kunna organisera bättre. 


## [[Glance]]
Glance = http://100.75.94.81:8080/home

 glance kommer att vara min dashboard, då jag hittade hur jag skulle fixa containern, och hur jag sätter up glance, sedan så configurerade jag den så att den ser ut som jag vill lite gran på deras github.
 https://github.com/glanceapp/glance



Docker compose download or git pull
```
mkdir glance && cd glance && curl -sL https://github.com/glanceapp/docker-compose-template/archive/refs/heads/main.tar.gz | tar -xzf - --strip-components 2
```

* `docker-compose.yml` to configure the port, volumes and other containery things 
* `config/home.yml` to configure the widgets or layout of the home page
* `config/glance.yml` if you want to change the theme or add more pages
###### other files to edit 
*  `.env` to configure environment variables that will be available inside configuration files
- `assets/user.css` to add custom CSS

When you want to run use: 
```
docker compose up -d
```

To close use:

```
docker compose down
```

If you encounter any problems, you can check logs by running
```
docker compose logs
```

==🔴(Är bytt till en lxc continaer  i stället för mer simplicitet)==
## [[Uptime kuma]]

Dashboard= http://100.75.94.81:3001/

Gjorde en ny mapp på samma server som Glance som heter uptime-kumma då jag sedan skapade en docker compose file med nano som jag sedan la in detta: 

om du vill så kan du ändra port, volym, timezone, networks osv men det är inget måste, du kan även ta bort saker som du inte behöver.

```
services:
  uptime-kuma:
    image: louislam/uptime-kuma:2
    container_name: uptime-kuma
    restart: always
    ports:
      - "3001:3001"  # This maps the container port "3001" to the host port "3001"
    volumes:
      - /path/to/data:/app/data  # Configuring persistent storage
    environment:
      - TZ=UTC  # Set the timezone (change to your preferred local timezone so monitoring times are the same)
      - UMASK=0022  # Set your file permissions manually
    networks:
      - kuma_network  # add your own custom network config
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3001"]
      interval: 30s
      retries: 3
      start_period: 10s
      timeout: 5s
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"

networks:
  kuma_network:
    driver: bridge
```

Starta dockern med: 

```
docker-compose up -d
```

Nu så kan du gå till din webbläsare och gå och söka på ip med 3001 som vissas här nere.

```
http://<your-server-ip-here>:3001
```

Nu när vi är här inne så kan man göra lite allt möjligt, då jag valde att sätte up en allmän notification så att när något går fel så får jag ett meddelade på discord.

configurationen när man vill lägga till något är fairly simpel referens till bild nedan:

![[2026-10-13_15-35.png|285]]


![[2026-10-13_15-33.png|302]]


==🔴(Är bytt till en lxc continaer  i stället för mer simplicitet)==
 
## [[Pi-hole]]

https://100.91.154.61/admin/login

Installera en ny vm för detta som också är debian baserad, då jag börja med installera tailscale så att jag kan ssh in till den, därefter så skapade jag en mapp i home dir som heter pihole.

Sedan efter det så gjorde jag en docker compose med detta innehåll:

```
services:  
 pihole:  
   container_name: pihole  
   image: pihole/pihole:latest  
   ports:  
     # DNS Ports  
     - "53:53/tcp"  
     - "53:53/udp"  
     # Default HTTP Port  
     - "80:80/tcp"  
     # Default HTTPs Port. FTL will generate a self-signed certificate  
     - "443:443/tcp"  
     # Uncomment below if using Pi-hole as your DHCP Server  
     [[-]] "67:67/udp"  
   environment:  
     TZ: 'Europe/Stockholm'  
     # Set a password to access the web interface. Not setting one will result in a random password being assigned  
     FTLCONF_webserver_api_password: '***'  
     # If using Docker's default `bridge` network setting the dns listening mode should be set to 'ALL'  
     FTLCONF_dns_listeningMode: 'ALL'  
  
   volumes:  
    - './etc-pihole:/etc/pihole'  
   [[cap_add]]:  
     [[-]] NET_ADMIN  
     [[-]] SYS_TIME  
     [[-]] SYS_NICE  
  
   restart: unless-stopped
```

Därefter det så istället för att koppla Pihole till min router so kopplade jag den till mitt tailnet vilket gör att när jag är inne på vpnen så förs all trafik först igenom där och sedan vidare om de blev godkänt.



## [[Npm]] 


Jag tog mig tag på pve helper scripts för att installera nginx proxy manager. 

då jag installerade npm men sedan behöver man även ha en form utav domain/ dns, i detta fall använder jag mig av duckdns.org då man gör ett konto där och skapar en dns som man villha, den kommer se ut så här (Exempel.duckdns.org) därefter så behöver du säga att du vill använda nginx med denna dns, då om du använder dig av an vpn tjänst som tailscale använder du dens vpn ip address.

Efter du har en dns så går du till certificates och lägger till en via dns, skriv in din domain som, Exempel.duckdns.org sedan tryck enter och sedan lägg även till * . Exempel.duckdns.org då den vill säga att med den så kan du använde vilken subdomain som du vill med din domain. Sedan lägg in vad för dns provider du använder i mitt fall duckdns, därefter byt ut "your-duckdns-token" till din token du bör ha fått från duckdns eller din dns provider och sedan tryck save.

Nu har du ett certifikat och är redo att lägga till hoster, då du nu addar proxy host och fyller i domain name som ([pve].exemepl.duckdns.org) sedan vad för scheme den använder alltså http eller https, där efter lägger du in ip addresen och porten din servies går på, det är bra att boka för block commaon exploits och webbsockets support. 

Tryck nu på SSL sidan på samma ruta och välj din SSL/ certifikat och force SSL samt HTTP/2 Support sedan är du klar.


## [[NAS]]

Jag installerade Truenas vm via pve helper community scripts:
https://community-scripts.org/scripts?q=storage&preview=truenas-vm

Sedan när den är klar installerad gå igenom wizarden i consolen.

Jag kom tilll lite problem då min vm sjkut up sig till max ram vilket är konstigt.

## [[OpenGym]]
### Links
> https://opengym.ch/#download
> https://opengym.cronahem.duckdns.org/#/home

### Setup
Detta är en simpel och komplett gym tracking och weight tracking app som du kan hosta själv elller bara ha localt på din mobil men fördelen med att ha det hostad själv är att du får möjlighet till att ha många users och syncad data.

## [[vautlwarden]]

### Installation
Jag installerade denna också med ett script som är :
```
https://community-scripts.org/scripts/vaultwarden
```
```
var_os='alpine' bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/ct/vaultwarden.sh)"
```

Denna setup är väldigt simpel, du skapar bara ett konto så är du klar.

### Export
Vill du ha dina passwords från någon annan form av passwordmanager som exempel chrome så kan du importera dem, och om du redan har en bitwarden vault så går du enkelt in i det valvet som kanske är ifrån .eu servern.

Då du går in i settings och sedan vault option och export.


## 
