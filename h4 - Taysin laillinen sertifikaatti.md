### h4 Täysin Laillinen Sertifikaatti [tehtävänanto](https://terokarvinen.com/tunkeutumistestaus/#h4-taysin-laillinen-sertifikaatti)

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

### OWASP 2021: OWASP Top 10:2021

- Pääsynhallinnalla pyritään varmistamaan, että käyttäjä ei pysty toimimaan hänelle määriteltyjen oikeuksien ulkopuolella.
- 94%:ssa testatuista sovelluksista on löytynyt puutteita pääsynhallinnassa.
- Tyypillisimpiä haavoittuvuuksia:
  - Vähimpien oikeuksien periaatteiden rikkominen
  - Pääsynhallinnan tarkistusten ohitus
  - Oikeuksien laajentaminen (privilege escalation)
  - Metadatan manipulointi
  - Virheellinen CORS-konfiguraatio

### PortSwigger Academy: Insecure direct object references (IDOR)

- IDOR haavoittuvuus: Voi esiintyä silloin, kun sovellus käyttää käyttäjän antamaa syötettä objektien hakemiseen ilman riittäviä pääsynvalvontatarkistuksia.
- Liittyy tyypillisesti horisontaaliseen oikeuksien laajentamiseen, mutta ei poissulje vertikaalisten oikeuksien laajentamista.

### PortSwigger Academy: Path traversal

- Tunnetaan myös nimellä directory traversal.
- Haavoittuvuus, jossa hyökkääjä voi lukea (joissakin tapauksissa kirjoittaa) palvelimen tiedostoja.
- Hyökkääjä pyrkii kiertämään hakemistorajoitukset manipuloimalla tiedostopolkuja URL-parametreissa.

### PortSwigger Academy: Cross-site scripting

- Haavoittuvuus, jossa hyökkääjä pyrkii manipuloimaan verkkosivustoa siten, että se palauttaa haitallista JavaScript-koodia käyttäjille.
- Hyökkäysten tyyppejä:
  - Reflected XSS: haitallinen skripti tulee nykyisestä HTTP-pyynnöstä.
  - Stored XSS: haitallinen skripti tulee weppisivun tietokannasta.
  - DOM-based XSS: haavoittuvuus sijaitsee asiakaspuolen koodissa palvelinpuolen koodin sijaan.

## a) Totally Legit Sertificate. Asenna OWASP ZAP, generoi CA-sertifikaatti ja asenna se selaimeesi. Laita ZAP proxyksi selaimeesi. Laita ZAP sieppaamaan myös kuvat, niitä tarvitaan tämän kerran kotitehtävissä. Osoita, että hakupyynnöt ilmestyvät ZAP:n käyttöliittymään. (Voi vaatia Firefox about:config network.proxy.allow_hijacking_localhost. Foxyproxy laittoi tämän aiemmin päälle itse. Kalin Firefox ESR oli viimeksi ongelmia Foxyproxyn kanssa - vaihtoehtona on asettaa Proxy käsin Settings, hakusana "proxy")

Asensin OWASP ZAP:in komennolla: `sudo apt-get install zaproxy`

<img width="876" height="320" alt="image" src="https://github.com/user-attachments/assets/1bc1695a-e515-49f2-9ab0-ea6436bddd88" />

Sitten ohjelma käyntiin komennolla: `zaproxy`, ohjelma kysyi alkuun istunnon tallentamisesta, johon valitsin ei:

<img width="611" height="248" alt="image" src="https://github.com/user-attachments/assets/5c2af541-8e9b-4338-8e0d-0d425ebdc701" />

CA-sertifikaatin generoiminen löytyi navigoimalla `Tools` > `Options` > `Network` > `Server Certificates`

<img width="744" height="577" alt="image" src="https://github.com/user-attachments/assets/bf6e9da6-5b0e-461f-b8d9-e3dd1cf8c11d" />

Loin uuden sertifikaatin painikkeesta `Generate`. Kävin tekemässä uuden kansion sertifikaatteja varten terminaalissa `mkdir certificates` ja tallensin sertifikaatin tuohon kansioon `Save` painikkeella.

Tämän jälkeen navigoin Firefox -selaimessa settings ja hakukenttään hakusanaksi "certificate". `View Certificates` -painikkeesta Certificate Manager auki ja `Import...` -painikkeesta kävin lataamassa tuon sertifikaatin:

<img width="663" height="467" alt="image" src="https://github.com/user-attachments/assets/e206a079-cda1-4576-b5b9-069a05970244" />

Navigoin `Settings` > `Network Settings` ja kävin antamassa Proxy määritykset:

<img width="749" height="449" alt="image" src="https://github.com/user-attachments/assets/5e9d7449-4820-4d18-aaca-0e3882c1f789" />


Kuvien sieppaus päälle ZAPissa `Tools` > `Options` > `Display` > `Process images in HTTP requests/responses`

<img width="744" height="580" alt="image" src="https://github.com/user-attachments/assets/1e9842c9-a50a-405e-934b-aefc5da0ccd2" />

Sitten koeajo Metasploitable weppipalvelimen kanssa ja tarkistin että hakupyynnöt ilmestyvät ZAPiin:

<img width="1138" height="119" alt="image" src="https://github.com/user-attachments/assets/1d4d67e9-8223-4f79-a08d-ea19f4c6b46b" />

## b) Kettumaista. Asenna "FoxyProxy Standard" Firefox Addon, ja lisää ZAP proxyksi siihen. Käytä FoxyProxyn "Patterns" -toimintoa, niin että vain valitsemasi weppisivut ohjataan Proxyyn. (Läksyssä ohjataan varmaankin PortSwigger Labs ja localhost.)

Aluksi `Settings` > `Network Settings` kävin laittamassa takaisin täpän kohtaan `No proxy`, jotta FoxyProxy toimii oikein, eikä ZAP sieppaa kaikkea liikennettä.

Sitten siirryin Firefoxissa `Extensions` painikkeella Add-ons Manager -sivulle ja etsin lisäosaa hakukentän kautta "foxyproxy":

<img width="1301" height="270" alt="image" src="https://github.com/user-attachments/assets/89ab7483-02cd-45ea-8106-88e0415caebd" />

Latasin FoxyProxyn ja lisäsin pikakuvakkeen työkaluriville, jotta löydän sen nopeasti jatkossakin:

<img width="469" height="164" alt="image" src="https://github.com/user-attachments/assets/1c1fea1d-bae1-4ec4-b9c0-f9ed5470379a" />

Sitten klikkasin tuosta pikakuvakkeesta ja siirryin asetuksiin `Options` painikkeesta:

<img width="365" height="412" alt="image" src="https://github.com/user-attachments/assets/8dcf1609-78c8-45a7-86d0-9666ee1f45e7" />

Proxies -välilehdellä tein määritykset ja tarkistin Proxy by Patterns -määritykset [täältä](https://help.getfoxyproxy.org/index.php/knowledge-base/url-patterns/)

<img width="1169" height="483" alt="image" src="https://github.com/user-attachments/assets/9ee887f8-689c-4fe0-bc92-68b779c6da4b" />

Proxy by Patterns päällä:

<img width="299" height="331" alt="image" src="https://github.com/user-attachments/assets/c3db125e-78b6-4c51-b473-7383c021c8fb" />

Testasin toimivuutta navigoimalla PortSwiggerin labra harjoitukseen ja katsoin mitä tietoja ZAP kerää:

<img width="1416" height="727" alt="image" src="https://github.com/user-attachments/assets/4264d5dc-ce86-4bb5-b355-8909025e0340" />

## PortSwigger Labs. Ratkaise tehtävät. Selitä ratkaisusi: mitä palvelimella tapahtuu, mitä eri osat tekevät, miten hyökkäys löytyi, mistä vika johtuu. Kannattaa käyttää ZAPia, vaikka malliratkaisut käyttävät harjoitusten tekijän maksullista ohjelmaa. Monet tehtävät voi ratkaista myös pelkällä selaimella. Malliratkaisun kopioiminen ZAP:n tai selaimeen ei ole vastaus tehtävään, vaan ratkaisu ja haavoittuvuuden etsiminen on selitettävä ja perusteltava.
## c) Reflected XSS into HTML context with nothing encoded

Labran tehtävänannossa vihjataan, että haavoittuvuus löytyy hakutoiminnosta. Aloitin labraharjoituksen syöttämällä hakukenttään tekstin "hello".

Tämän jälkeen tarkastelin URL-osoitetta, joka oli muotoa `https://0a27000303a820fe8069218500110030.web-security-academy.net/?search=hello`

Palasin lukaisemaan [PortSwigger Academyn](https://portswigger.net/web-security/cross-site-scripting) materiaalia ja lähdin manipuloimaan tuota URLia vaihtamalla "hello" hakusanan tilalle `<script>alert("hello")</script>`

<img width="482" height="129" alt="image" src="https://github.com/user-attachments/assets/d56096d0-cfb8-414a-b420-ad1ec41016ef" />

Labraharjoitus oli sillä taputeltu:

<img width="409" height="56" alt="image" src="https://github.com/user-attachments/assets/11a53f77-c70b-431a-b608-8efe1fa02f53" />

Toistin vaiheet vielä uudelleen ja katsoin miltä ne näyttävät ZAPissa:

<img width="1314" height="488" alt="image" src="https://github.com/user-attachments/assets/6b04d7ff-6999-4ac1-83a3-70b4c76b792c" />

Mitä tapahtui?
- Selain lähetti hakukentän tekstin (tässä tapauksessa <script>alert("hello")</script>) palvelimelle GET-pyyntönä.
- Palvelin käsitteli pyynnön ja palautti HTML-sivun.
- <script>alert("hello")</script> sijoitettiin h1-elementtiin.
- javascript suoritettiin selaimessa, koska palvelin ei enkoodannut syötettä.

## d) Stored XSS into HTML context with nothing encoded

Tällä kertaa kerrotaan, että haavoittuvuus löytyy kommenttitoiminnosta. Valitsin ensimmäisen postauksen ja scrollasin sivun alalaitaan josta "Leave a comment" -kenttä löytyy.

Itse skripti sijoitetaan suoraan kommenttikenttään. Nimi ja sähköposti kentät olivat pakollisia, joten nimi kenttään jotain satunnaista ja sähköposti kenttään copy-pastella nopeasti 10minutemail -osoite:

<img width="750" height="658" alt="image" src="https://github.com/user-attachments/assets/10e88082-3fd0-4e0a-8388-3b84e11b1396" />

<img width="700" height="273" alt="image" src="https://github.com/user-attachments/assets/2d3a6b96-5e26-4f92-82ce-3da6e2e144f4" />

Klikkasin `< Back to Blog` jolloin pop-up ikkuna hyppäsi ruudulle:

<img width="476" height="129" alt="image" src="https://github.com/user-attachments/assets/7a32b6cc-6032-4f1f-866d-a9c376c5990a" />

Kommenteissa itse "kommenttia" ei näy:

<img width="730" height="330" alt="image" src="https://github.com/user-attachments/assets/460168bd-ad08-4dc1-a528-c2acf3e11135" />

Näkymä ZAPissa:

<img width="1808" height="636" alt="image" src="https://github.com/user-attachments/assets/a56534ea-6501-43e8-a1a1-bfe3b64ac6cb" />


Mitä tapahtui?
- ZAPista nähdään, että selain teki palvelimelle POST-pyynnön.
- Hyökkäys: comment=%3Cscript%3Ealert%28%22Hello+again%22%29%3C%2Fscript%3E
- Palvelin tallensi tiedon sellaisenaan tietokantaan.
- Kun siirryttiin takaisin sivulle (jossa haitallinen kommentti on) selain tekee GET-pyynnön ja palvelin palauttaa myrkyllisen kommentin, jonka selain suorittaa.

<img width="1151" height="580" alt="image" src="https://github.com/user-attachments/assets/958c76ca-1dd0-446e-a4e1-4141a3f524ca" />

## e) Selitä esimerkin avulla, mitä hyökkääjä hyötyy XSS-hyökkäyksestä. Alert("Hei Tero!") ei vielä tarjoa kummoista pääsyä. (Tässä alakohdassa ei tarvitse tehdä testejä tietokoneella, pelkkä lyhyt ja selkeä selitys riittää.)

Hyökkääjä voi XSS-hyökkäyksen avulla yrittää varastaa esimerkiksi kirjautumistiedot.  Uhrin syöttämät kirjautumistunnukset voidaan kaapata ja lähettää hyökkääjälle, kun uhrin selaimessa suoritetaan haitallinen JavaScript -koodi.

## Path traversal
## f) File path traversal, simple case. Laita tarvittaessa Zapissa kuvien sieppaus päälle.

Vihje tällä kertaa viittaa sivustolla oleviin kuviin, joiden kautta tulisi päästä käsiksi /etc/passwd -tiedostoon.

Siirryin labran etusivulta ensimmäisen kuvan kohdalta `View details`.

<img width="1239" height="484" alt="image" src="https://github.com/user-attachments/assets/b608980d-266d-4a8a-94f8-e110a6d04c57" />

ZAPista viimeisin GET-pyyntö näyttääkin kyseisen kuvan kohdalla käytetyn URLin `https://0ada007e04fe07a3831697ef009500ad.web-security-academy.net/image?filename=1.jpg`:

<img width="732" height="187" alt="image" src="https://github.com/user-attachments/assets/8f544bba-9cac-4296-b8d5-c96cd6fc89bf" />

Voidaan varmistaa vielä, antamalla kyseinen URL suoraan selaimeen:

<img width="1060" height="729" alt="image" src="https://github.com/user-attachments/assets/5b716b59-b7f9-497f-a94a-260d963661b2" />

[PortSwigger -materiaalista](https://portswigger.net/web-security/file-path-traversal) löytyikin suoraan vastaus tähän tehtävään.

<img width="898" height="118" alt="image" src="https://github.com/user-attachments/assets/2395ff1f-afa0-4f62-aec2-3ef96bcebe6b" />

Uusi URL on siis `https://0ada007e04fe07a3831697ef009500ad.web-security-academy.net/image?filename=../../../etc/passwd`

Selaimeen tulee vastauksena:

<img width="1407" height="53" alt="image" src="https://github.com/user-attachments/assets/d1e362fd-d94c-4361-bdd3-22665acf3946" />

Kävin katsomassa ZAPista viimeisintä GET-pyynnön vastausta. Siitä ei ensin irronnut mitään, labraharjoituksen etusivulle tuli kuitenkin ilmoitus "Congratulations, you solved the lab!"...

Ei meinannut aueta, katselin esimerkki ratkaisuja läpi että mitä minulta oli mennyt ohi. Selailin ZAPin käyttöliittymää ja ratkaisu olikin aika yksinkertainen, Responsen alta löytyi alasveto valikko, jossa oli oletuksena `Body: Image`. Tuohon kun vaihtoi tyypiksi `Body: Text` niin päästiin katselemaan tiedoston sisältöä:

<img width="636" height="555" alt="image" src="https://github.com/user-attachments/assets/71ec615c-36fe-4aa7-9c67-1cf49b508fc3" />

## g) File path traversal, traversal sequences blocked with absolute path bypass

Tämä olikin nopeasti ratkaistu. Vastaava kuin edellinen, mutta sovellus yrittää estää hakemistojen läpikäyntisekvenssit, mutta käsittelee annetun tiedostonimen suhteessa oletustyöhakemistoon.

Toisinsanoen jätetään vain ylimääräiset `../` -vaiheet osoitteesta pois ja syötetään URL `https://0a6700790322b9c582c4ec71006e008e.web-security-academy.net/image?filename=/etc/passwd`

<img width="1359" height="828" alt="image" src="https://github.com/user-attachments/assets/45b8224c-400a-46ae-b901-a0e0408d97f9" />

## h) File path traversal, traversal sequences stripped non-recursively

Vihjeessä kerrotaan, että sovellus karsii läpikäyntisekvenssit. Toisin sanoen `../` pätkät filename -parametrista siivotaan pois. 

Selailin hieman esimerkkiratkaisuja läpi, kun ei meinannut aueta että miten saan tuon kierrettyä. Selvisi, että `....//` toimii, koska se ei vastaa suoraan siivottavaa `../` merkkijonoa, mutta voidaan silti myöhemmin tulkita hakemistopolun osana.

Ratkaisu siis saadaan manipuloimalla filename parametria, antamalla sille arvoksi `....//....//....//etc/passwd`.

## Insecure Direct Object Reference (IDOR)
## i) Insecure direct object references

Vihjeen mukaan ratkaisu piilee tällä kertaa palvelimelle tallennettavissa chat logeissa.

Siirryin sivulla Live chat -osioon ja naputtelin sinne jotain satunnaista. Klikkasin View transcript -painiketta ja siirryin ZAPiin katselemaan, josko sieltä löytyisi jotain mielenkiintoista.

<img width="783" height="229" alt="image" src="https://github.com/user-attachments/assets/73a511e2-c889-4f97-b479-1896f7f9684b" />

Intuitio kertoo että vastaus ratkeaa tuolta riviltä. Jos kerran 2.txt sisältää oman keskusteluni, niin 0.txt, 1.txt, 3.txt jne. voisivat sisältää jotain mielenkiintoista.

Teron ohjeissa oli maininta Requester -tabista (hotkey: Ctrl+W), joten lähdin kokeilemaan sitä.

<img width="1461" height="526" alt="image" src="https://github.com/user-attachments/assets/fd5d4206-0451-47e4-b0f9-792b200427be" />

Request kenttään muokkasin 1.txt -> 0.txt ja painoin ´Send´, mutta se ei tuottanut tulosta:

<img width="490" height="399" alt="image" src="https://github.com/user-attachments/assets/9059d0e1-ec4b-4d23-8981-dfb79965a450" />

1.txt tärppäsi ja sisälsi salasanan:

<img width="544" height="394" alt="image" src="https://github.com/user-attachments/assets/d2d7096d-0089-4d1d-b34c-3ef368732fd8" />

Testailin huvikseen myös muita vaihtoehtoja, mutta niistä ei löytynyt easter eggejä.

Siirryin lopuksi `My account` -sivulle, annoin tehtävänannossa annetun käyttäjätunnuksen `carlos` ja tuon löytämäni salasanan, jolloin pääsin sisään:

<img width="750" height="455" alt="image" src="https://github.com/user-attachments/assets/544cf189-37cf-497f-b8a3-0912408d0487" />

## Lähteet

FoxyProxy. URL Patterns. https://help.getfoxyproxy.org/index.php/knowledge-base/url-patterns/

Kali.org. kali-platforms. https://www.kali.org/get-kali/#kali-platforms

Karvinen, T. 22.3.2026. Tunkeutumistestaus. https://terokarvinen.com/tunkeutumistestaus/

Owasp. A01:2021 – Broken Access Control. https://owasp.org/Top10/2021/A01_2021-Broken_Access_Control/index.html

PortSwigger. Insecure direct object references (IDOR). https://portswigger.net/web-security/access-control/idor

PortSwigger. Path traversal. https://portswigger.net/web-security/file-path-traversal

PortSwigger. Cross-site scripting. https://portswigger.net/web-security/cross-site-scripting
