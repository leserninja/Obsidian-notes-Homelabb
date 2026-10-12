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
