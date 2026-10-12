## Glance
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
