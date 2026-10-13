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