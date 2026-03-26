### h1 Kybertappoketju [tehtävänanto](https://terokarvinen.com/tunkeutumistestaus/#h1-kybertappoketju)

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

## x) Lue/katso/kuuntele ja tiivistä

### Herrasmieshakkerit - Kuka puolustaa Suomea? Vieraana prikaatikenraali Mikko Heiskanen
- Vastuu Suomalaisen yhteiskunnan puolustamisesta on hajautettu. Puolustusvoimat puolustavat ensisijaisesti omia järjestelmiään. Tämä on kansallisessa kyberturvallisuusstrategiassa 2013 puolustusvoimille määritetty rooli.
- Kyberaseen käyttöä edellyttää tiedustelu, jonka jälkeen valitaan siihen sopiva ase (= hyötykuorma (eng. payload)) ja tarkoitus mihin asetta käytetään.
- Edistyneet kyberaseet ovat tyypillisesti kertakäyttöisiä, koska niiden hyödyntämät haavoittuvuudet ovat käytön jälkeen tiedossa.
- Kyberulottuvuudessa kovimmat kyvykkyydet omaavat maat ovat Kiina, Venäjä, Yhdysvallat, Israel, Ranska ja Hollanti.
- Puolustusvoimien johtamisjärjestelmäpäällikkö vastaa noin 10:stä prosentista puolustusbudjetista, joka on noin 300 miljoonaa euroa. Vertailun vuoksi F-Securen liikevaihto on noin 200 miljoonaa.
- Sama koskee henkilöstöä; noin 10% henkilöstöstä toimii johtamisjärjestelmien ja kybertoimintojen parissa.

### Hutchins et al 2011: Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains, chapters Abstract, 3.2 Intrusion Kill Chain.
- Kybertappoketjua kuvataan päästä-päähän prosessina.
- Ketjun vaiheet: Tiedustelu (Reconnaissance) > Aseistaminen (Weaponization) > Toimitus (Delivery) > Hyödyntäminen (Exploitation) > Asennus (Installation) > Komento ja hallinta (Command and Control) > Toiminta kohteessa (Actions on Objectives)
- Puhutaan ketjusta, sillä minkä tahansa prosessin vaiheen katkeaminen keskeyttää koko prosessin.

### € Santos et al: The Art of Hacking (Video Collection): 4.3 Surveying Essential Tools for Active Reconnaissance.
- Videolla kerrotaan porttiskannauksesta, siihen liittyvistä työkaluista ja käydään niiden toimintoja läpi.
- Nmap: Tunnetuin ja monipuolisin porttiskannaustyökalu.
- Masscan: Ehdottomasti nopein, ei sisällä yhtä monipuolisia työkaluja kuin Nmap.
- Udpprotoscanner: Nopea UDP porttiskannaustyökalu.
- EyeWitness: auttaa priorisoimaan kohdesivustoja palauttamalla sivuista kuvankaappauksen IP-osoitteen perusteella.

### KKO 2003:36
- Käräjäoikeuden syyte ja tuomio 1998 tapahtuneesta porttiskannauksesta Osuuspankkikeskus-OPK osuuskunnan tietojärjestelmään. Tekijänä 17-vuotias.
- Valitus eteni hovioikeuteen asti.
- Skannaus ei läpäissyt tietojärjestelmän palomuuria. Syytteiksi luettiin tietoliikenteen häirintä ja tietomurron yritys.
- Tekijä velvoitettiin suorittamaan vahingonkorvauksena osuuskunnalle 20 000 markkaa ja yhtiölle 55 000 markkaa korkoineen.

## a) Asenna Kali virtuaalikoneeseen. (Jos asennuksessa ei ole mitään ongelmia tai olet asentanut jo aiemmin, tarkkaa raporttia tästä alakohdasta ei tarvita. Kerro silloin kuitenkin, mikä versio ja millä asennustavalla. Jos on ongelmia, niin tarkka ja toistettava raportti).
Latasin Kalista Pre-built virtuaalikoneen [täältä](https://www.kali.org/get-kali/#kali-virtual-machines). Asennus oli yksinkertainen, eikä mitään ongelmia tullut vastaan.

<img width="392" height="222" alt="image" src="https://github.com/user-attachments/assets/87e4b86d-95fb-4f4b-a80c-bbcb42251d36" />

## b) Irrota Kali-virtuaalikone verkosta. Todista testein, että kone ei saa yhteyttä Internetiin (esim. 'ping 8.8.8.8')

Nopein tapa irrottaa kone verkosta on valita työpöydän oikeasta yläkulmasta Ethernet Network -> Disconnect.

<img width="481" height="187" alt="image" src="https://github.com/user-attachments/assets/a52c7c79-0687-476b-a53d-8d0dd90baeac" />

<img width="413" height="124" alt="image" src="https://github.com/user-attachments/assets/2ea6f85a-8ce3-4d31-8f4b-4455d0b90d65" />

Ping -komento ei mene läpi eikä selaimella pääse esimerkiksi www.google.com -sivulle.

<img width="864" height="458" alt="image" src="https://github.com/user-attachments/assets/f85ed7d9-9dc1-43ed-8a1e-113bd20c7c67" />

Toinen vaihtoehto on poistaa cable connected -valinta virtuaalikoneen asetuksista:

<img width="630" height="708" alt="image" src="https://github.com/user-attachments/assets/2799d6cc-7800-41ab-ada7-61a7cc3fea9d" />

<img width="1057" height="572" alt="image" src="https://github.com/user-attachments/assets/ba97f66e-d3cf-440b-b789-24e4659564b5" />

## c) Porttiskannaa 1000 tavallisinta tcp-porttia omasta koneestasi (nmap -T4 -A localhost). Selitä komennon paramterit. Analysoi ja selitä tulokset.

Skannauksen tulokset, kommentit vihreällä pohjalla:

```diff
┌──(kali㉿kali)-[~]
└─$ nmap -T4 -A localhost  
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-25 18:01 -0400
+ Käynnistetään ohjelma ja näytetään käyttäjälle ohjelman versio ja ajankohta.
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
+ DNS palvelimia ei löytynyt, käänteinen DNS-kysely poistettu käytöstä.
Failed to resolve "localhost".
+ Ei saatu selvitettyä localhost IP-osoitetta.
WARNING: No targets were specified, so 0 hosts scanned.
+ Kohteen IP-osoitetta ei saatu selvitettyä -> kelvollista kohdetta ei voitu määrittää.
Nmap done: 0 IP addresses (0 hosts up) scanned in 0.26 seconds
+ Skannauksen tulos ja suoritusaika.
                                                                                 
+ Uusi ajo localhost IP-osoitteella
┌──(kali㉿kali)-[~]
└─$ nmap -T4 -A 127.0.0.1  
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-25 18:01 -0400
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 127.0.0.1
Host is up (0.000018s latency).
All 1000 scanned ports on 127.0.0.1 are in ignored states.
Not shown: 1000 closed tcp ports (reset)
+ Kaikki 1000 skannattua TCP-porttia ovat suljettuja.
Too many fingerprints match this host to give specific OS details
+ Liikaa mahdollisia osumia, ei voida yksilöidä käyttöjärjestelmää.
Network Distance: 0 hops
+ Skannattiin paikallinen kohde, joten hops = 0.
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 1.87 seconds
```

`man nmap` löydetään parametrien merkitykset

<img width="613" height="211" alt="image" src="https://github.com/user-attachments/assets/e2006384-c6f6-4850-a79c-3df4b116407d" />

-T<0-5>: Säätää skannauksen nopeutta. Suurempi luku = nopeampi ("aggressiivisempi"), mutta myös helpommin havaittava. Asteikot menevät [manuaalin](https://nmap.org/book/man-performance.html) mukaan:
0 = paranoid, 1 = sneaky, 2 = polite, 3 = normal, 4 = aggressive, 5 = insane.

<img width="636" height="145" alt="image" src="https://github.com/user-attachments/assets/687f8714-d89a-41bf-8dc0-0269d34bedc2" />

-A: Laajempi skannaus ottaa mukaan seuraavat: OS detection, version detection, script scanning, traceroute.

## d) d) Asenna kaksi vapaavalintaista demonia ja skannaa uudelleen. Analysoi ja selitä erot.

apache2 ja openssh-server olivat jo valmiiksi asennettuina:

<img width="544" height="254" alt="image" src="https://github.com/user-attachments/assets/14d67413-6656-4a62-93be-ac0a86198005" />

Apache käyntiin:

<img width="804" height="557" alt="image" src="https://github.com/user-attachments/assets/e7e9efc8-5ed2-42dd-9f6e-719bf55c3241" />

Openssh-server käyntiin:

<img width="795" height="475" alt="image" src="https://github.com/user-attachments/assets/6be4e5e0-9cbc-4c47-b7ab-f1692113eafa" />

Uudessa porttiskannauksessa näkyy nyt avoinna olevat portit 22 (ssh) ja 80 (http) ja löydettiin vielä käyttöjärjestelmänkin tiedot:

<img width="807" height="383" alt="image" src="https://github.com/user-attachments/assets/dff6f5fd-bacc-473b-b63b-d8f34837a096" />

## e) e) Ratkaise vapaavalintainen kone HackTheBoxista. Omalle tasolle sopiva, useimmille varmaan Starting Pointista. Valitse kone, jota et ole ratkaissut vielä. Ei tunnilla näytetty Meow. (Propellihatuille: jos teet vaikeampia ei-starting-point koneita, niin retired tai vastaava kone, josta saa julkaista writeupin).

Tein järjestyksessä kolme seuraavaa konetta, vastaukset alla:

<details>
  <summary> Näytä Fawn ratkaisu</summary>

Aloitin tekemällä porttiskannauksen `nmap -T4 -A 10.129.46.89`

<img width="790" height="623" alt="image" src="https://github.com/user-attachments/assets/66398f96-c607-4fe3-b699-5952c3c38b54" />

Otetaan yhteys FTP-palvelimeen komennolla `ftp 10.129.46.89` ja kirjaudutaan sisään käyttäjänä 'Anonymous'. Kirjautuminen onnistuu ilman salasanaa (230 login succesful):

<img width="333" height="200" alt="image" src="https://github.com/user-attachments/assets/68affd9c-9e1e-4142-a3c4-5aa3de55baae" />

`ls` listaa tiedostot. Nähdään että löytyy tiedosto 'flag.txt', ladataan se komennolla `get flag.txt` ja tiedostosta löytyy flagi:

<img width="800" height="365" alt="image" src="https://github.com/user-attachments/assets/b4d2e474-f863-42c6-9781-09de3b72b52d" />

  
</details>

<details>
  <summary> Näytä Dancing ratkaisu</summary>

Aloitin tekemällä porttiskannauksen `nmap -T4 -A 10.129.46.93`

<img width="830" height="708" alt="image" src="https://github.com/user-attachments/assets/4991340c-040c-4450-8655-e8f8b04f2979" />

Nopean porttien googlettelun jälkeen löysin [tälle](https://www.speedguide.net/port.php?port=445) sivulle. Lähdin googlesta etsimään tietoa tuosta microsoft-ds palvelusta ja päädyin [tänne](https://security.stackexchange.com/questions/229820/microsoft-ds-vulnerability).
Selvisi että smb = Server Message Block ja microsoft-ds -palvelua käytetään verkon yli tapahtuvaan resurssien jakoon. `man smb` ja tabulaattoria hyväksikäyttäen löysin oikean manuaalisivun, jotta pääsin eteenpäin:

<img width="834" height="142" alt="image" src="https://github.com/user-attachments/assets/1f086d17-d389-49d9-862d-7569d1d39daa" />

-L parametrilla saadaan list=HOST. Ajoin seuraavaksi komennon `smbclient -L 10.129.46.93`

<img width="740" height="230" alt="image" src="https://github.com/user-attachments/assets/d2b9673f-a79d-4439-9b7a-2371402a6d86" />

Löytyy 4 sharea: ADMIN$, C$, IPC$ ja WorkShares.

Manuaalista löytyy ohjeet, miten shareen yhdistetään:

<img width="951" height="110" alt="image" src="https://github.com/user-attachments/assets/77098cc9-ab9c-42c7-9add-cf39bd224913" />

Kokeilin kaikki sharet järjestyksessä, josko sisään pääsisi ilman salasanaa. IPC$ pääsi sisään, mutta se ei sisällä tiedostoja. WorkShares -resurssi näyttää lupaavalta:

<img width="624" height="500" alt="image" src="https://github.com/user-attachments/assets/f79e12a3-23db-4f82-adaa-ccc83a8d7549" />

Amy.J käyttäjältä löytyi tiedosto worknotes.txt, nappasin talteen uteliaisuuttani. Kaivattu flagi löytyy kuitenkin käyttäjältä James.P:

<img width="912" height="335" alt="image" src="https://github.com/user-attachments/assets/4081fd84-94e4-41c5-ad72-799364343e02" />

Löytyi flagi ja muuta mukavaa:

<img width="856" height="221" alt="image" src="https://github.com/user-attachments/assets/1375a859-84af-4f5c-8031-884196510c8d" />

  
</details>

<details>
  <summary> Näytä Redeemer ratkaisu</summary>

Aloitin tekemällä porttiskannauksen `nmap -T4 -A 10.129.46.101`

<img width="846" height="285" alt="image" src="https://github.com/user-attachments/assets/115fa254-fe59-4e84-a61b-e41d9e2c139a" />

Haulla ei löytynyt avoimia portteja, joten piti laajentaa haravointia. Manuaali käteen:

<img width="652" height="109" alt="image" src="https://github.com/user-attachments/assets/86c647db-84a3-46d6-a92e-08377003c811" />

Tällä kertaa skannasin kaikki portit komennolla `nmap -T4 -A 10.129.46.101 -p 1-65535`

<img width="850" height="347" alt="image" src="https://github.com/user-attachments/assets/5219edb7-272c-44ce-a2fa-8a0a8acdc286" />

Löytyi avoin portti 6379. Komennolla `redis-cli 10.129.46.101` tuli kuitenkin virheilmoitus:

<img width="554" height="104" alt="image" src="https://github.com/user-attachments/assets/4456c1db-149d-45a2-9678-a4ab4cd78f9d" />

Perehdyin komentoihin [täältä](https://redis.io/docs/latest/operate/rs/references/cli-utilities/redis-cli/). Hahmottelin komennoksi `redis-cli -h 10.129.46.101`

<img width="361" height="79" alt="image" src="https://github.com/user-attachments/assets/5691082d-9fd7-4ebe-8b51-66e4f90326a1" />

Promptilla 'help' pääsin eteenpäin ja selailin tabulaattorilla vaihtoehtoja kunnes kohdalle osui 'help @server' ja sieltä löytyi kaivattu komento 'info'

<img width="724" height="226" alt="image" src="https://github.com/user-attachments/assets/935c7e52-ecf0-42a4-9ce3-f4207fbbed18" />

<img width="548" height="64" alt="image" src="https://github.com/user-attachments/assets/2345bdc5-1367-4977-9f6b-f9f0fb2a5a03" />

indeksillä 0 löytyy 4 avainta. help KEYS kertoi komennon olevan muotoa `KEYS pattern`, heitin komennon perään jokerin (*) ja alkoi näkyä tuloksia:

<img width="479" height="272" alt="image" src="https://github.com/user-attachments/assets/29a50dad-d6f7-44ff-9800-2ed1c037a578" />

key 3) "flag" nimen perusteella sisältää flagin. Ajoin komennon `get flag` ja sain lipun:

<img width="337" height="153" alt="image" src="https://github.com/user-attachments/assets/1e9c96f6-20a7-4ff0-962a-20ea0b3dd9f7" />

</details>

## Lähteet

Herrasmieshakkerit. 1.6.2020. Kuka puolustaa Suomea? Vieraana prikaatikenraali Mikko Heiskanen. https://open.spotify.com/episode/1B8Lf6o1oMJIb5d4GHN8VR

Information Security. Microsoft DS vulnerability? https://security.stackexchange.com/questions/229820/microsoft-ds-vulnerability

Kali.org. kali-platforms. https://www.kali.org/get-kali/#kali-platforms

Karvinen, T. 22.3.2026. Tunkeutumistestaus. https://terokarvinen.com/tunkeutumistestaus/

Nmap.org. Nmap Reference Guide. https://nmap.org/book/man-performance.html

nixCraft. Ubuntu Linux install OpenSSH server. https://www.cyberciti.biz/faq/ubuntu-linux-install-openssh-server/

Redis. redis-cli commands. https://redis.io/docs/latest/operate/rs/references/cli-utilities/redis-cli/

speedguide.net. Port 445 Details. https://www.speedguide.net/port.php?port=445
