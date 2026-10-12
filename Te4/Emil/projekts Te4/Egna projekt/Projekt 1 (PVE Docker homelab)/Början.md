Börjar med att flasha en usb sticka med en fräsh proxmox eftersom att den som jag hade på den usb stickan var en dålig verision som inte var up till datum.

Därefter gick jag med installationen som är väldigt enkel med deras grafiska installations guide. O.B.S man får inte glömma att sätta den på sitt egna subnet som man har, som exempel 192.168.[0].x då i detta fall nollan representerar ett subnät.

Sedan routerns ip och gateway vilket i mesta fall brukar vara: 192.168.0.1
Detta är viktigt för att servern och routern ska kunna kommunicera med varandra, så att du också kan komma in i den grafiska webb UI. 

Sedan har jag satt up alla inställningar som jag behövde för att börja min första vm med en docker container i. detta valde jag för att jag ska kunna organisera bättre. 
