### h5 Fuzzy [tehtävänanto](https://terokarvinen.com/tunkeutumistestaus/#h5-fuzzy)

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

### Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf

- Weppipalvelimilla on usein salaisia hakemistoja, joihin ei löydy linkkejä mistään.
- Näitä hakemistoja voi yrittää etsiä manuaalisesti lisäämällä URL-osoitteen perään esimerkiksi /admin, /root, /secret jne.
- ffuf automatisoi piilotettujen hakemistojen fuzzauksen. Ohjelma hyödyntää tekstimuotoista sanalistaa ja käy läpi tuhansia URL-osoitteita hetkessä.
- Peruskomento: `ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ`, jossa `-w` määrittää sanalistan, `-u` kohdeosoitteen ja `FUZZ` korvataan sanalistan arvoilla.

### Hoikkala 2023: ffuf README.md

- ffuf (Fuzz Faster U Fool) on Joona "joohoi" Hoikkalan web-fuzzer -työkalu.
- Tukee monipuolista fuzzingia, kuten esimerkiksi hakemistot, GET/POST parametrit ja virtual hostit.
- Käyttäjä voi tehdä omia konfiguraatioita ja tallentaa ne oletusasetustiedostoon, joka sijaitsee $XDG_CONFIG_HOME/ffuf/ffufrc.

## a) Fuzzzz. Ratkaise dirfuz-1 artikkelista Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf.

Aloitin luomalla uuden työkansion harjoituksia varten `mkdir fuzz`, siirryin hakemistoon `cd fuzz` ja latasin sanalistan `wget https://raw.githubusercontent.com/danielmiessler/SecLists/master/Discovery/Web-Content/common.txt`

<img width="471" height="276" alt="image" src="https://github.com/user-attachments/assets/4dc3ddd5-1dad-4bdf-b1af-57273fdab923" />

Päivitin pakettilistauksen `sudo apt-get update`, ffufin asennus onnistuu komennolla `sudo apt-get install -y ffuf`. Löytyi tosin Kalista jo valmiina:

<img width="738" height="318" alt="image" src="https://github.com/user-attachments/assets/4086882f-98aa-4f13-a0c0-be186e61ab46" />

Latasin harjoitusmaalin `wget https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/dirfuzt-1` ja annoin suoritusoikeudet `chmod u+x dirfuzt-1`

<img width="744" height="531" alt="image" src="https://github.com/user-attachments/assets/f0ac63f1-962f-4c03-a6bc-684fe861b5bd" />

Harjoitusmaali käyntiin komennolla `./dirfuzt-1`, tässä vaiheessa irrotin Kalin verkosta ja varmistin ettei se saa yhteyttä nettiin. Katsoin että palvelin vastaa osoitteessa `http://127.0.0.2:8000`

<img width="1013" height="578" alt="image" src="https://github.com/user-attachments/assets/6381de9e-4b2e-48dc-b7d2-6887c29eef76" />

Aloitin ajamalla `ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ`, tuloksia tuli 4751 kappaletta ja valtaosassa tulosten koko näytti olevan 154 tavua. Karsin tuloksia parametrilla `-fs 154` ja katsoin mitä jää jäljelle:

<img width="725" height="578" alt="image" src="https://github.com/user-attachments/assets/06f1a103-90b2-475c-858a-fb9f5b16f788" />

Löytyi kaksi eri flagia. Ensimmäinen polusta `/.git` (ja sen alihakemistoista .git/index, .git/logs jne.) ja toinen polusta `/wp-admin`

<img width="605" height="511" alt="image" src="https://github.com/user-attachments/assets/7869782d-8106-4ee9-a3d6-2d4fc4e35fe8" />

## b) Fuff me. Asenna FuffMe-harjoitusmaali. Karvinen 2023: Fuffme - Install Web Fuzzing Target on Debian

Harjoitusmaalin asennus ohjeiden mukaan:

- Asennetaan paketit docker.io, git ja ffuf:
  - `sudo apt-get install docker.io git ffuf`
- Ladataan harjoitusmaali ja rakennetaan docker-kontti:
  - `git clone https://github.com/adamtlangley/ffufme`
  - `cd ffufme/`
  - `sudo docker build -t ffufme .`
- Suoritetaan harjoitusmaali:
  - `sudo docker run -d -p 80:80 ffufme`
  - `curl localhost`

Sitten tarkistin että maali vastaa selaimella osoitteessa localhost:

<img width="797" height="599" alt="image" src="https://github.com/user-attachments/assets/00a682ba-2aff-4d97-a8f8-a56d705e29b6" />

Latasin fuzzausta varten sanalistat:

```
wget http://ffuf.me/wordlist/common.txt
wget http://ffuf.me/wordlist/parameters.txt
wget http://ffuf.me/wordlist/subdomains.txt
```

Irrotin harjoituksia varten Kalin netistä ja varmistin sen komennolla `ping 8.8.8.8`. 

<img width="344" height="126" alt="image" src="https://github.com/user-attachments/assets/b678a7ab-1425-4b95-ba21-5946512f39e1" />

## Ratkaise ffufme harjoitukset - kaikki paitsi ei "Content Discovery - Pipes".
## c) Basic Content Discovery

<img width="839" height="310" alt="image" src="https://github.com/user-attachments/assets/4a02a74b-968a-4261-b5e9-58e13cb1af74" />

Ensimmäisessä harjoituksessa käydään läpi perus ffuf-komento. Vastaukset `class` sekä `development.log` löytyivät komennolla:

`ffuf -w common.txt -u http://localhost/cd/basic/FUZZ`

<img width="775" height="489" alt="image" src="https://github.com/user-attachments/assets/135c22ec-2b27-4d82-935d-e7cc5cc7aea8" />

## d) Content Discovery With Recursion

<img width="847" height="323" alt="image" src="https://github.com/user-attachments/assets/373e272c-e14c-4f4e-a598-8c572279fc49" />

Komento `ffuf -w common.txt -u http://localhost/cd/recursion/FUZZ` paljasti hakemiston `admin`, mutta sillä ei saada vielä haluttua tulosta.

<img width="473" height="265" alt="image" src="https://github.com/user-attachments/assets/37c63743-9409-48a6-a236-86d7f885d448" />

Harjoituksen tarkoituksena on suorittaa rekursiivinen haku, jolloin nähdään vielä hakemiston ~/admin alla olevat hakemistot.

Uusi haku komennolla `ffuf -w common.txt -recursion -u http://localhost/cd/recursion/FUZZ`

<img width="757" height="620" alt="image" src="https://github.com/user-attachments/assets/f8ddd01a-20f0-48c7-9a25-4088b189b846" />

Löytyi uudet alikansiot `/admin/users` ja vielä `/admin/users/96`. Jälkimmäisellä tärppäsi:

<img width="602" height="146" alt="image" src="https://github.com/user-attachments/assets/7f4be2dd-95a8-458f-9950-6c67ebf077d3" />

## e) Content Discovery With File Extensions

<img width="830" height="365" alt="image" src="https://github.com/user-attachments/assets/0464592c-3527-43eb-acfa-b6123dad532f" />

Komennolla `ffuf -w common.txt -u http://localhost/cd/ext/FUZZ` löytyy hakemisto `logs`, mutta sillä ei päästä näkemään vielä mitään sisältöä:

<img width="471" height="250" alt="image" src="https://github.com/user-attachments/assets/86660594-3fc0-4f0e-999c-5dbf4ddd295a" />

Ohjeistuksessa oletetaan, että haettava tiedosto on .log -päätteinen. `-e .log` parametrilla voidaan muokata hakua paljastamaan nuo .log päättyvät tiedostot:

<img width="767" height="481" alt="image" src="https://github.com/user-attachments/assets/12523374-baed-4284-ae58-c3f90f84a3dc" />

Löytyy `users.log`, eli kaivattu polku on tällöin `http://localhost/cd/ext/logs/users.log`

<img width="579" height="162" alt="image" src="https://github.com/user-attachments/assets/65f7d362-aa30-483a-87a8-cb54e8ef8904" />

## f) No 404 Status

<img width="837" height="534" alt="image" src="https://github.com/user-attachments/assets/eaf84d4f-cfc7-415e-856c-2a6ed9b1a2ad" />

Komennolla `ffuf -w common.txt -u http://localhost/cd/no404/FUZZ` tulee 4686 kappaletta tuloksia:

<img width="778" height="191" alt="image" src="https://github.com/user-attachments/assets/aa9b6866-f9d4-40f5-b520-c9f40aeb8683" />

Valtaosassa tuloksia näkyy Size: 669, joten tuloksia voidaan suodattaa `-fs 669` parametrilla, jolloin tulokseksi jää ainoastaan `secret`:

<img width="793" height="472" alt="image" src="https://github.com/user-attachments/assets/553b8662-f9ff-45ad-b1d5-6cf9d4fdbaa0" />

Tiedoston sisältö voidaan hakea esim. `curl http://localhost/cd/no404/secret`

<img width="478" height="110" alt="image" src="https://github.com/user-attachments/assets/69b977e4-4688-4f5f-9d45-9718ca331d16" />

## g) Param Mining

<img width="829" height="364" alt="image" src="https://github.com/user-attachments/assets/3889707c-4068-4ddd-95bd-d26a773e9ed3" />

Tehtävässä on linkki /cd/param/data, jonka takaa löytyy viesti "Required Parameter Missing"

<img width="797" height="175" alt="image" src="https://github.com/user-attachments/assets/f76cd1ea-151b-42cd-84bf-150448f0bc22" />

Tehtävässä käytetään sanalistaa `parameters.txt` ja ohjeistetaan, että puuttuvaa parametria voidaan hakea lisäämällä URLin loppuun `?FUZZ=1`

Komento kokonaisuudessaan `ffuf -w parameters.txt -u http://localhost/cd/param/data?FUZZ=1` paljastaa tuloksen `debug`

<img width="733" height="461" alt="image" src="https://github.com/user-attachments/assets/220e86b0-5f32-4c99-b464-228eac863994" />

Eli selaimeen voidaan syöttää nyt `http://localhost/cd/param/data?debug`

<img width="792" height="193" alt="image" src="https://github.com/user-attachments/assets/ef9becb3-052b-4875-a6e8-2395f708bcc4" />

## h) Rate Limited

<img width="825" height="519" alt="image" src="https://github.com/user-attachments/assets/8be654fe-4d99-42dc-ab1c-9ea49020f882" />

Tehtävänannossa kerrotaan, että palvelin on rajoitettu 50 pyyntöön sekunnissa.

Ajamalla komennon `ffuf -w common.txt -u http://localhost/cd/rate/FUZZ -mc 200,429`, joka palauttaa vastaukset statuksilla 200 tai 429
palauttaa odotetusti vain vastauksia statuksella 429:

<img width="751" height="264" alt="image" src="https://github.com/user-attachments/assets/425cf3fc-8547-414f-a1df-61fa0b7d8307" />

Ongelma voidaan kiertää lisäämällä parametri `-p 0.1`, jolloin ohjelma lisää 0,1 sekunnin viiveen pyyntöjen välille ja `-t 5` parametrilla ajetaan viisi rinnakkaista säiettä, kun se oletuksena on 40 [lähde](https://github.com/ffuf/ffuf/blob/master/README.md).

Tällöin pyyntöjen määrä pysyy tuossa max 50 pyyntöä / sekunti, eivätkä palvelimen rajoitukset tule enää vastaan. Haku on odotetusti huomattavasti hitaampi, mutta ei palauta enää 429 virheitä ja löydetään oracle tiedosto:

<img width="751" height="503" alt="image" src="https://github.com/user-attachments/assets/31a781b9-5130-4c22-b90d-1c46e59e0726" />

<img width="562" height="159" alt="image" src="https://github.com/user-attachments/assets/7004bef4-6cad-4eaf-9371-4154d4c57b49" />

## i) Subdomains - Virtual Host Enumeration

<img width="857" height="475" alt="image" src="https://github.com/user-attachments/assets/874f8522-56fc-4883-85de-1ca9addadb8a" />

Viimeisessä tehtävässä etsitään subdomaineja käyttämällä virtual host -menetelmää. Tuloksia voidaan etsiä `-H "Host:FUZZ.ffuf.me"` HTTP-headerilla, jossa `FUZZ` sijoitetaan aliverkkotunnuksen paikalle.

Tehtävässä käytetään sanalistaa subdomains.txt. Ajoin komennon `ffuf -w subdomains.txt -H "Host:FUZZ.ffuf.me" -u http://localhost`.

<img width="748" height="226" alt="image" src="https://github.com/user-attachments/assets/64de30e6-739a-4286-8f32-068544674641" />

Tuloksia tuli sietämätön määrä. Karsin hakutuloksia `-fs 1495`, sillä valtaosa tuloksista näytti olevan 1495 tavun kokoisia.

`ffuf -w subdomains.txt -H "Host:FUZZ.ffuf.me" -u http://localhost -fs 1495`

Tuloksista löytyi redhat:

<img width="796" height="452" alt="image" src="https://github.com/user-attachments/assets/1db64fc5-e48b-4b06-ab09-62e87dc06284" />

## Lähteet

Hoikkala 2023. ffuf/README. https://github.com/ffuf/ffuf/blob/master/README.md

Karvinen, T. 30.10.2023. Fuffme - Install Web Fuzzing Target on Debian. https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/

Karvinen, T. 10.5.2023. Find Hidden Web Directories - Fuzz URLs with ffuf. https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/

Karvinen, T. 22.3.2026. Tunkeutumistestaus. https://terokarvinen.com/tunkeutumistestaus/
