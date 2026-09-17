# Labbmiljö, Git, CLI och AI

Namn: Nathali Karlsson  
Datum: 2026-09-17  
Kurs: Introduktion till yrkesrollen och grunderna i IT-infrastruktur

Kort beskrivning av labbmiljön:
Labben består av en Linux-VM och en Windows-VM
som är placerade i samma interna nätverk och används för att genomföra
nätverks-, behörighets- och CLI-övningar.

## GIT/github


skapar konto på github.com
git laddas ner från den officiella hemsidan git-scm.com och installerades i windows
installationen verifieras med git-version, sedan lägger jag till min mail som används till mitt githubkonto och mitt fullständiga namn och efternamn, får en key som jag klistrar in på github under inställningar-ssh keys. fungerar inte den, sök i dola mappar på datorn efter .ssh och leta efter id_ed25519.pub-högerklicka-välj öppna med anteckningar, kopiera den nyckeln och klistra in.
```
git --version
git config --global user.name "skriv ditt namn inom parentesen"
git --config global user.email "skriv din mail här"
ssh-keygen -t ed25519 -C "eposten som är kopplat till github"
```

## Labbmiljö & Nätverk

Här beskriver jag hur de två virtuella maskinerna sattes upp
 

```
| Hostname       | Operativsystem  | IP-adress    | Subnätmask | Standard Gateway |
  natta00          ubuntu 26.04        192.168.40.20        /24          ingen gateway
  desktop-EA9HHF8  windows 11     192.168.40.10        /24          ingen gateway
```

### Nätverksuppsättning

Kort beskrivning av:
- vilken hypervisor som används
- vilket internt nätverk som används
- vilka statiska IP-adresser maskinerna har
- att maskinerna ligger i samma subnät 

1. hypervisor VMware jag använder PRO versionen
1. använder enligt genomförandemallen samma nät 40.10 samt 40.20.
1. static ip sattes på Ubuntu i inställningar-network-ipv4 sätt static ip.
1. static ip sattes i windows 11 genom inställningar-nätverk och internet ip-tilldelning-ändra från dhcp till manuell/static ange IP-adress och subnätmask 255.255.255.0(/24)

 ## Kommandoradsgenomförande
  Här ser man ip-adress, broadcast, ipv4 samt ipv6-adresser med subnätmask och gateway
 kontroll av ip-adress görs i powershell/Bash med följande kommando:
  
 ```
 ipconfig
 ```

 ```
 ip a
 ```


### Linux

#### Skapa mapp och fil
skapar loggkatalog för mappen. -p flaggan skapar även överliggande mappar om de saknas. katalogen skapades till slut utan felmeddelande.
```bash
kommando sudo mkdir -p /var/systementor/konsultdata
sudo touch /var/systementor/konsultdata/anteckningar.txt
ls -l /var/systementor/konsultdata
```
#### Skapa grupp och rättigheter
skapar grupp och regler för skriv/läsrättigheter. -m skapar hemkatalog och -g sätter användarens primära grupp. till sist kontrollerar man rättigheter med ls -l. -la skriver ut hela mappen med alla filer uppradade för lättare översikt över rättigheter.
```bash
sudo groupadd konsulter
sudo useradd -m -g konsulter konsultanv
sudo passwd konsultanv
sudo chown konsultanv:konsulter /var/systementor/konsultdata
sudo chown konsultanv:konsulter /var/systementor/konusltdata/anteckningar.txt
sudo chmod 750/var/systementor/konsultdata
sudo chmod 640 /var/systementor/konsultdata/anteckningar.txt
sudo ls -l /var/systementor/konsultdata
ls -l /var/systementor/konsultdata
sudo ls -la /var/systementor/konsultdata
```

### Windows 
i windows använder jag powershell för att skriva in kommandon, och det behöver göras med administratör-rättigheter. set-location flyttar mig till mappen, get-childitem skriver jag för att verifiera att mappen systementor listar konsultdata. listar acl-regler med get-acl, resultatet visar vilka användare och grupper som har åtkomst samt vilka rättigheter de har.flera av behörigheterna är ärvda från den överordnade mappen.
```Powershell
set-location c:\systementor
new-item -itemtype directory -path c:\systementor\konsultdata -force
get-childitem c:\systementor
(get-acl .\konsultdata).access
```






