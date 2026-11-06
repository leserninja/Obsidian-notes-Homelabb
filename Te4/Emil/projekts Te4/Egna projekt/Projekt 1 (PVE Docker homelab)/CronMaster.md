Detta är en Server manager som hanterar scheduled tasks och scripts för att automera saker och ting, jag använder den enbart för att ha automerade reboots varje söndag klockan 00:00

Installeras på din server node och inte som en separat container eller vm, så på din node runnar du denna command. 

```
bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/addon/cronmaster.sh)"
```

OBS! Går inte att ha en proxy på denna, vet ännu inte varför men jag har prövat lite olika saker.
