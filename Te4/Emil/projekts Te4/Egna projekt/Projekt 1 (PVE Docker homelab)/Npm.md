### första steg
Jag tog mig tag på pve helper scripts för att installera nginx proxy manager. 

då jag installerade npm men sedan behöver man även ha en form utav domain/ dns, i detta fall använder jag mig av duckdns.org då man gör ett konto där och skapar en dns som man villha, den kommer se ut så här (Exempel.duckdns.org) därefter så behöver du säga att du vill använda nginx med denna dns, då om du använder dig av an vpn tjänst som tailscale använder du dens vpn ip address.

Efter du har en dns så går du till certificates och lägger till en via dns, skriv in din domain som, Exempel.duckdns.org sedan tryck enter och sedan lägg även till * . Exempel.duckdns.org då den vill säga att med den så kan du använde vilken subdomain som du vill med din domain. Sedan lägg in vad för dns provider du använder i mitt fall duckdns, därefter byt ut "your-duckdns-token" till din token du bör ha fått från duckdns eller din dns provider och sedan tryck save.
### andra steg
Nu har du ett certifikat och är redo att lägga till hoster, då du nu addar proxy host och fyller i domain name som ([pve].exemepl.duckdns.org) sedan vad för scheme den använder alltså http eller https, där efter lägger du in ip addresen och porten din servies går på, det är bra att boka för block commaon exploits och webbsockets support. 

Tryck nu på SSL sidan på samma ruta och välj din SSL/ certifikat och force SSL samt HTTP/2 Support sedan är du klar.

