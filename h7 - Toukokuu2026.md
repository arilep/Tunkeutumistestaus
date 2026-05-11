### h7 Toukokuu2026! [tehtävänanto](https://terokarvinen.com/tunkeutumistestaus/#h7-toukokuu2026)

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

## x) Lue/katso ja tiivistä.

### Karvinen 2022: Cracking Passwords with Hashcat

- Järjestelmät eivät tallenna salasanoja sellaisenaan, vaan sen sijaan käytetään niiden tiivisteitä (hash).
- Hashaus toimii vain yhteen suuntaan, joten salasanaa ei voi palauttaa tiivisteestä suoraan.
- Tietokoneella voidaan kuitenkin vertailla sanalistassa olevien salasanojen hash-arvoja ja mahdollisesti murtaa salasana löytämällä oikea vastine.
- Suosituimpiin sanalistoihin kuuluva Rockyou sisältää yli 14 miljoonaa sanaa.

### Karvinen 2023: Crack File Password With John

- Monet tiedostot tukevat salausta salasanan avulla.
- John the Ripper -työkalulla voidaan murtaa nämä salasanat sanakirja -hyökkäyksen avulla.

## a) Asenna Hashcat ja testaa sen toiminta murtamalla esimerkkisalasana.

Lähdin liikkeelle seuraamalla [Teron ohjeita](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/), ensin pakettien päivitys ja tarvittavien ohjelmien asennus.

    sudo apt-get update
    sudo apt-get -y install hashid hashcat wget

<img width="798" height="454" alt="image" src="https://github.com/user-attachments/assets/89fbf7e9-76d9-42ec-acce-6a3f851c44de" />

hashid ja wget löytyikin jo valmiina. hashcat ja hashcat-data päivitettiin.

Sitten uusi kansio "hashed" tehtävää varten:

    mkdir hashed
    cd hashed

Tehtäviä varten tarvitaan sanalista, latasin Teron [ohjeissa](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/) mainitun Rockyou.txt -sanalistan (.tar.gz paketti), purin sanalistan tuohon uuteen kansioon ja poistin sen jälkeen paketin:

    wget https://github.com/danielmiessler/SecLists/raw/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz
    tar xf rockyou.txt.tar.gz
    rm rockyou.txt.tar.gz

Ladatusta sanalistasta löytyy yli 14 miljoonaa sanaa:

<img width="302" height="435" alt="image" src="https://github.com/user-attachments/assets/f5b27db0-b740-41fa-9015-0ef377adb3b4" />

Päätin lähteä testaamaan samoissa ohjeissa käytettyä salasanatiivistettä `6b1628b016dff46e6fa35684be6acc96`

Ensin täytyy selvittää hash-tyyppi, joka onnistuu komennolla:

    hashid -m 6b1628b016dff46e6fa35684be6acc96

<img width="450" height="355" alt="image" src="https://github.com/user-attachments/assets/96f81c0d-86a2-4d62-9ff1-f48d2988162c" />

`-m` parametri lisää tulosteeseen Hashcatissa käytettävän `-m` parametrin arvon, jota käytetään salasanan murtamisessa. Parametrien käytöstä löytyy lisää `man hashid` sivulta.

<img width="609" height="309" alt="image" src="https://github.com/user-attachments/assets/dfe68600-58dd-48fa-b978-715cf986add2" />

ilman `-m` parametria hashid tulostaa vain listan mahdollisista tiivistetyypeistä:

<img width="419" height="405" alt="image" src="https://github.com/user-attachments/assets/5d5a42ae-c867-4593-aac0-671f40752977" />

[Teron ohjeiden](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/) mukaan oikea hash-tyyppi on usein jokin kolmesta ensimmäisestä tulostetusta vaihtoehdosta (MD2, MD5, MD4). Tiivistetyyppiä voidaan pyrkiä rajaamaan sen mukaan, mistä tiiviste on peräisin (esim. Windows- tai Linuxkäyttöjärjestelmä). Ohjeissa paljastetaankin, että kaivattu hash-tyyppi on kahta muuta vaihtoehtoa yleisemmin käytetty MD5.

Murtamisessa käytetty komento `man hashcat` mukaan on muotoa `hashcat [options] hashfile [mask|wordfiles|directories]`.

`-o` parametrilla voidaan tallentaa tulos omaan tiedostoonsa, muutoin tulos on nähtävissä lisäämällä komennon perään `--show`

Käytetty komento on siis muotoa:

    hashcat -m 0 6b1628b016dff46e6fa35684be6acc96 rockyou.txt -o solved

Jossa:
- `hashcat` = hashcat-työkalu
- `-m 0` = hashcat mode-parametri arvolla 0, joka vastaa hash-tyyppiä MD5 joka selvitettiin hashid -komennolla
- `6b1628b016dff46e6fa35684be6acc96` = käytetty tiiviste
- `rockyou.txt` = käytetty sanalista
- `-o solved` = tallenna tulos "solved" nimiseen tiedostoon

Ensimmäinen ajo epäonnistui:

<img width="752" height="670" alt="image" src="https://github.com/user-attachments/assets/5ee53bf1-e8c7-4211-b308-7ad1f9482c4d" />

Virheilmoitus herjaa, ettei hashcat saanut tarpeeksi muistia ajoa varten. Kävin nostamassa koneen muistin määrää reippaasti 2048 MB --> 8192 MB, jotta ei jää ainakaan siitä kiinni.

<img width="759" height="474" alt="image" src="https://github.com/user-attachments/assets/01edcaf5-d129-4f00-a899-8b9600e3f7d4" />

Nyt ajo meni läpi, eikä aikaakaan kulunut kuin muutama sekunti:

```
┌──(kali㉿kali)-[~/hashed]
└─$ hashcat -m 0 '6b1628b016dff46e6fa35684be6acc96' rockyou.txt -o solved
hashcat (v7.1.2) starting

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, SPIR-V, LLVM 18.1.8, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
====================================================================================================================================================
* Device #01: cpu-haswell-AMD Ryzen 7 5800X 8-Core Processor, 2948/5897 MB (1024 MB allocatable), 2MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Early-Skip
* Not-Salted
* Not-Iterated
* Single-Hash
* Single-Salt
* Raw-Hash

ATTENTION! Pure (unoptimized) backend kernels selected.
Pure kernels can crack longer passwords, but drastically reduce performance.
If you want to switch to optimized kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Temperature abort trigger set to 90c

Host memory allocated for this attack: 512 MB (6665 MB free)

Dictionary cache built:
* Filename..: rockyou.txt
* Passwords.: 14344391
* Bytes.....: 139921497
* Keyspace..: 14344384
* Runtime...: 0 secs

                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 0 (MD5)
Hash.Target......: 6b1628b016dff46e6fa35684be6acc96
Time.Started.....: Sun May 10 16:03:23 2026 (0 secs)
Time.Estimated...: Sun May 10 16:03:23 2026 (0 secs)
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)
Guess.Base.......: File (rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#01........:    42981 H/s (0.17ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 2048/14344384 (0.01%)
Rejected.........: 0/2048 (0.00%)
Restore.Point....: 0/14344384 (0.00%)
Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#01...: 123456 -> lovers1
Hardware.Mon.#01.: Util: 47%

Started: Sun May 10 16:03:21 2026
Stopped: Sun May 10 16:03:25 2026
```

Tulos löytyi nyt solved-tiedostosta selkokielisenä tekstinä (ASCII):

<img width="491" height="279" alt="image" src="https://github.com/user-attachments/assets/afee504e-34a4-48f3-9dee-2ffb65cd9bee" />

Tulos vielä ilman tuota `-o` parametria `--show` komennon avulla:

<img width="606" height="111" alt="image" src="https://github.com/user-attachments/assets/82442fce-3184-4ddc-9802-33cb8c1e556b" />


## c) Asenna John the Ripper ja testaa sen toiminta murtamalla jonkin esimerkkitiedoston salasana.

Työkalu näytti löytyvän Kalista jo valmiina:

<img width="943" height="391" alt="image" src="https://github.com/user-attachments/assets/81f371c2-24e4-4947-87a2-d67c3f32b3b3" />

Latasin valmiiksi salatun harjoitus ZIP-tiedoston, joka löytyy Teron [Crack File Password With John](https://terokarvinen.com/2023/crack-file-password-with-john/) artikkelista:

    wget https://terokarvinen.com/2023/crack-file-password-with-john/tero.zip

<img width="1065" height="331" alt="image" src="https://github.com/user-attachments/assets/04205a5d-9417-4016-a713-7f18757dd665" />

Koitetaan ensin tuurilla ja testataan salasanoja `admin`, `foo` ja `bar` jos tärppäisi (ei tärpännyt):

<img width="484" height="429" alt="image" src="https://github.com/user-attachments/assets/6e8b3f75-9e27-41aa-bd92-a972be79ff5d" />

Poistin turhan (tyhjän) kansion `rmdir secretFiles` ja lähdin seuraavaksi ratkomaan salasanaa John the Ripperin avulla.

ZIP-tiedoston salasanan murtaminen tapahtuu kahdessa vaiheessa. Ensin puretaan hash uuteen tiedostoon, jonka jälkeen tehdään sanalista hyökkäys hashia vastaan.

Hashin purku:

    zip2john tero.zip > tero.zip.hash

<img width="1104" height="386" alt="image" src="https://github.com/user-attachments/assets/87afe7ab-54ee-419c-a957-67bf214c1914" />

Ja sitten selvitetään itse salasana:

    john tero.zip.hash

<img width="785" height="271" alt="image" src="https://github.com/user-attachments/assets/3876827f-e793-46ba-b9f5-7899e346061b" />

Salasana näkyy tulosteessa oranssilla:

    butterfly        (tero.zip/secretFiles/SECRET.md)

Tiedosto aukesi tuolla salasanalla "butterfly" ja kansio saatiin purettua onnistuneesti:

<img width="628" height="580" alt="image" src="https://github.com/user-attachments/assets/b63e6e0c-023e-4855-89ac-be6d9a6fc878" />

## e) Tiedosto. Tee itse tai etsi verkosta jokin salakirjoitettu tiedosto, jonka saat auki. Murra sen salaus. (Jokin muu formaatti kuin aiemmissa alakohdissa kokeilemasi).

Googlaamalla löytyi mielenkiintoinen sivu, jossa oli paljon erilaisia harjoitusmaaleja https://github.com/openwall/john-samples.

Testasin kolmea eri maalia. Kaksi ensimmäistä vei niin pitkään, että päädyin etsimään helpompaa maalia. Kolmas aukesikin sitten hetkessä.

### 7z-easy.7z

7-Zip samplet löytyivät [täältä](https://github.com/openwall/john-samples/tree/main/7-Zip)

<img width="910" height="135" alt="image" src="https://github.com/user-attachments/assets/01b589d7-cfad-478c-8c0e-b83b6ca9c089" />

Ensimmäinen vaihe suoritettu, mutta toinen kesti ikuisuuden ja päädyin etsimään toista maalia:

<img width="874" height="639" alt="image" src="https://github.com/user-attachments/assets/cc13f9d9-a3cf-41b7-ab38-ceae79f92446" />

### Challenge8_pro_easy.pdf

Pdf samplet löytyivät [täältä](https://github.com/openwall/john-samples/tree/main/PDF)

Jälleen ensimmäinen vaihe meni nopeasti läpi, mutta itse salasanan murtaminen venähti taas niin pitkäksi, että koitin etsiä helpomman maalin:

<img width="834" height="415" alt="image" src="https://github.com/user-attachments/assets/d232d10b-1b5d-47ef-b90a-3e8eb242204d" />

### PDF-Example-Password.pdf

Pdf samplet löytyivät [täältä](https://github.com/openwall/john-samples/tree/main/PDF)
    
Tällä kertaa tärppäsi, ja löytyi maali joka mureni hetkessä. Ensin tiiviste uuteen tiedostoon ja sitten murretaan salasana:

    pdf2john PDF-Example-Password.pdf > PDF-Example-Password.pdf.hash
    john PDF-Example-Password.pdf.hash

<img width="862" height="335" alt="image" src="https://github.com/user-attachments/assets/d49e7dc5-3db4-46e6-9e85-3a8b0ab03fe4" />

Salasanaksi paljastui `test`. Avasin tiedoston ja se pyysi salasanaa:

<img width="795" height="704" alt="image" src="https://github.com/user-attachments/assets/cc471b31-6862-482e-991d-d225dd28ad86" />

Salasanalla `test` aukesi:

<img width="1662" height="627" alt="image" src="https://github.com/user-attachments/assets/4e5a3d10-4168-4568-86f6-73a04ea01267" />

## f) Tiiviste. Tee itse tai etsi verkosta salasanan tiiviste, jonka saat auki. Murra sen salaus. (Jokin muu formaatti kuin aiemmissa alakohdissa kokeilemasi. Voit esim. tehdä käyttäjän Linuxiin ja murtaa sen salasanan.)

Googlailemalla löysin [Hashcat wiki](https://hashcat.net/wiki/doku.php?id=example_hashes) -sivuston, jossa oli esimerkki hasheja testausta varten.

Jonosta ensimmäinen:

|Hash-Mode|Hash-Name|Example|
|---|---|---|
|0|MD5|8743b52063cd84097a65d1633f5c74f5|

    hashid -m 8743b52063cd84097a65d1633f5c74f5

<img width="451" height="406" alt="image" src="https://github.com/user-attachments/assets/60c75425-dacd-441b-a0f0-793a25975c39" />

Jälleen tiivistetyypeiksi ehdotetaan MD2, MD5 ja MD4. Tiedossa on että kyseessä MD5, joten:

    hashcat -m 0 8743b52063cd84097a65d1633f5c74f5 rockyou.txt -o ratkaistu

Mutta...

<img width="596" height="308" alt="image" src="https://github.com/user-attachments/assets/122a4c9e-d924-46f4-a87d-4e7227f1421e" />

Status: Exhausted. Tutkin asiaa [Hashcatin wiki](https://hashcat.net/wiki/doku.php?id=frequently_asked_questions#what_does_statusexhausted_mean) sivulta.

Sivuston mukaan kaikki salasanat olisi käyty läpi eikä vastinetta löydy. Wiki sivustolla kerrotaan myös, että salasana tiivisteelle on "hashcat". Ilmeisesti tuota sanaa ei siis löydy listalta?

<img width="448" height="281" alt="image" src="https://github.com/user-attachments/assets/736504e4-054e-436c-ad9c-c222e83fb54b" />

Uskottava se on. Lisäsin sanan listaan itse ja kokeilin uudestaan.

    echo "hashcat" >> rockyou.txt
    hashcat -O -m 0 8743b52063cd84097a65d1633f5c74f5 rockyou.txt -o ratkaistu

Muokatulla sanalistalla onnistui:

<img width="629" height="528" alt="image" src="https://github.com/user-attachments/assets/6b08cc50-8e68-4466-9520-bf2eb246c320" />

## g) Sanakirja. Oman sanakirjan teko parantaa onnistumismahdollisuuksia. Demonstroi, kuinka teet oman sanakirjan hashcat:n tai john:iin.

Tehtävänannon vinkeissä oli maininta Cewl-työkalusta, joten päätin tutustua siihen.

Työkalu löytyykin Kalista jo valmiina:

<img width="652" height="103" alt="image" src="https://github.com/user-attachments/assets/9dd97e78-15a5-416c-b80c-179134b743ab" />

Työkalussa on mielenkiintoinen ominaisuus, se voi annetun URL-osoitteen perusteella käydä sivuston läpi ja palauttaa sivustolta listan sanoja, joita voi käyttää sanalistassa [lähde](https://github.com/digininja/CeWL).

Muistelin että Apachen -testisivulla on paljon tekstiä. Päätin hostata tuon testisivun apachella ja katsoa saisinko siitä sanalistan aikaiseksi.

Apache2 käyntiin ja tilan tarkastus:

    sudo systemctl start apache2
    sudo systemctl status apache2

<img width="1097" height="496" alt="image" src="https://github.com/user-attachments/assets/450e40f9-f3b8-4252-b4b2-b00c575e5067" />

localhost sivu aukeaa:

<img width="1204" height="345" alt="image" src="https://github.com/user-attachments/assets/56fa37e9-9163-41ca-9722-f1bfd276f746" />

`man cewl` tutkailin vähän komennon rakennetta ja sen pitäisi onnistua näinkin yksinkertaisesti:

    cewl -w mytext.txt http://localhost

Ja hyvin toimii! Word countilla saatiin listan pituudeksi 238 sanaa:

<img width="287" height="329" alt="image" src="https://github.com/user-attachments/assets/5e9e91ea-8a3e-4555-a444-1995c51738d0" />


## h) Hash rules. Näytä esimerkki HashCatin sääntöjen käytöstä (rules).

Ensin aloin tutkimaan mihin kyseiset säännöt on tallennettuna. Löysin [Medium](https://medium.com/@fumn__/cracking-rules-and-masks-p-55w0rd-pattern-implications-5c43f9ed1044) -sivustolta ohjeet ja säännöt löytyvät polusta /usr/share/hashcat/rules.

<img width="754" height="583" alt="image" src="https://github.com/user-attachments/assets/5b03be7d-6b4b-45fe-bf76-f70622b90ec8" />

Perehdyin ensimmäiseen sääntöön "best66.rule" [täällä](https://github.com/hashcat/hashcat/blob/master/rules/best66.rule)

Sääntö tekee monia muutoksia, esimerkiksi lisää numeroita ja kirjaimia sanojen alkuun/loppuun, muuntaa kirjaimia numeroiksi jne.

Aiemmin todettiin, että sanaa "hashcat" ei löytynyt Rockyou.txt -listalta, ja se lisättiin jälkikäteen.

Loin uuden salasanan "hashcat2":

    echo -n "hashcat2" | md5sum

<img width="325" height="127" alt="image" src="https://github.com/user-attachments/assets/c239ae51-6f73-425e-9f42-f3c409b41fa8" />

Murrettava tiiviste on siis `055693a5717374b30689ad39b2146926`. Säännön best66.rule -avulla pitäisi nyt löytyä tuo uusi tiiviste.

Ajan säästämiseksi käytän edellisen osion pienempää sanalistaa "mytext.txt".

Lisäsin sanan "hashcat" listaan:

    echo "hashcat" >> mytext.txt

Sitten tiivisteen murtaminen mytext.txt -listalla ilman sääntöä, joka päättyy status: exhausted -lopputulokseen:

    hashcat -m 0 055693a5717374b30689ad39b2146926 mytext.txt -o newsolved

<img width="665" height="347" alt="image" src="https://github.com/user-attachments/assets/ecec81d3-f269-476c-ae4b-c53fe5b46e9c" />

Lisätään best66.rule ajoon, jolloin tiiviste pitäisi ratketa:

    hashcat -m 0 055693a5717374b30689ad39b2146926 mytext.txt -r /usr/share/hashcat/rules/best66.rule -o newsolved

Sääntö toimi odotetusti:

<img width="692" height="476" alt="image" src="https://github.com/user-attachments/assets/e7089b0e-d72b-4a0d-a540-81f3efea6168" />

## i) Lippuvalmistelu.

- Amd64 kone
- Koneesta pääsee nettiin, ja nettiyhteyden saa katkaistua virtuaalikoneesta
- Konetta on käytetty vain koulun kursseilla, joten ei salattavaa
- Ei omia muistiinpanoja
- Ei paikallista tekoälyä

## Lähteet

digininja. CeWL. https://github.com/digininja/CeWL

Hashcat.net. Example hashes. https://hashcat.net/wiki/doku.php?id=example_hashes

Hashcat.net. Faq. https://hashcat.net/wiki/doku.php?id=frequently_asked_questions#what_does_statusexhausted_mean

Hashcat. best66.rule. https://github.com/hashcat/hashcat/blob/master/rules/best66.rule

Karvinen, T. 22.3.2026. Tunkeutumistestaus. https://terokarvinen.com/tunkeutumistestaus/

Karvinen, T. 6.4.2022. Cracking Passwords with Hashcat. https://terokarvinen.com/2022/cracking-passwords-with-hashcat/

Karvinen, T. 9.2.2023. Crack File Password With John. https://terokarvinen.com/2023/crack-file-password-with-john/

Medium. Cracking Rules and Masks; P@55w0rd Pattern Implications. https://medium.com/@fumn__/cracking-rules-and-masks-p-55w0rd-pattern-implications-5c43f9ed1044

Openwall. john-samples. https://github.com/openwall/john-samples
