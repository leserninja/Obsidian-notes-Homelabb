## Installation
Jag Installerade containern via helperscripts och sedan la till tailscale som en addon.

### Config
Det mesta jag gjorde med config är att jag startade upp en minecraft server med ett modpack från moderinth men det specifika är att jag lade till en domain till server som förklaras här nedan: 

#### Config (Domain)

Du börjar med att följa **[[Npm]]** Sedan gå in i settings i Discopanel och går till routing och skriver in din base domain, sedan kolla att den lyssnar på port 25565.

Därefter gå in på din server och navigera till Routing där och skriv in på hostname ditt önskade namn och följ det med din base domain som här nere:

```
Namn.Base.domain.org
```

Alltså det namn du vill ha för att komma till din server samt din domain.

Sedan efter det navigerar du till npm och lägger till en streams

där du skriver in på "Incomming port" 25565 och samma för forward port och sist Forward host skriver du in ip addressen på docker container på panelen eller generellt ip addressen för din server, och bokkar i TCP.