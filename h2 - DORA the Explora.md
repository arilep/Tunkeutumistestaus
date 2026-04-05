### h2 DORA the Explora [tehtävänanto](https://terokarvinen.com/tunkeutumistestaus/#h2-dora-the-explora)

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

### Buuri 2026: DORA and TLPT testing - Lecture for Haaga-Helia on 31 March 2026
- DORA (Digital Operations Resilience Act)
  - EU:n laajuinen sääntely finanssialan digitaalisen häiriönsietokyvyn varmistamiseksi.
  - Astui voimaan tammikuussa -25.
  - Jokaisen EU:n jäsenvaltion on nimettävä DORAa valvova toimivaltainen viranomainen. Suomessa sääntelyä valvoo Finanssivalvonta.
  - Velvoittaa kaikkia finanssialan toimijoita suorittamaan perustason testausta. Merkittäville toimijoille vaaditaan lisäksi uhkapohjaista penetraatiotestausta (TLPT).

### DORA (Regulation ... on digital operational resilience for the financial sector)
- Artikla 26 käsittelee uhkapohjaista penetraatiotestausta
  - Mitkä toimijat se käsittää
  - Testauksen tiheys ja laajuus
  - Kolmansien osapuolien ICT-palveluntarjoajien osallistumisen

- Artikla 27 käsittelee uhkapohjaisen penetraatiotestauksen testaajien vaatimuksia

### TIBER-FI procedures and guidelines
- Kappaleessa käsitellään red team -toimintaa
  - Testaus jakaantuu kahteen vaiheeseen: suunnittelu (Red Team Test Plan) ja aktiiviseen testaukseen.
  - Aktiivinen testaus seuraa usein kybertappoketjun prosessia.

## a) Asenna Metasploitable 2 virtuaalikoneeseen.

Latasin Metasploitable 2:n osoitteesta https://www.rapid7.com/products/metasploit/metasploitable/

Purin zippi tiedoston ja valitsin VirtualBoxista "New" ja annoin seuraavat määritykset:

### Name and Operating System

<img width="775" height="363" alt="image" src="https://github.com/user-attachments/assets/f1ca1faf-61c7-4846-9fbb-548b68ef49bf" />

### Hardware

<img width="778" height="280" alt="image" src="https://github.com/user-attachments/assets/9839b03b-d961-4512-80c3-e82d815218da" />

### Hard Disk

<img width="771" height="470" alt="image" src="https://github.com/user-attachments/assets/3fc6fa7a-00d9-447a-bdb4-e797a2354730" />

### Display

<img width="751" height="439" alt="image" src="https://github.com/user-attachments/assets/82e6aaa3-ced3-4861-8166-8cd1ee574bd9" />

### Network

<img width="760" height="471" alt="image" src="https://github.com/user-attachments/assets/00235bb2-5f3d-4d20-bf36-f69e1d575345" />


## b) ja c) Tee Kalin ja Metasploitablen välille virtuaaliverkko. Harjoittelemme omassa virtuaaliverkossa, jossa on Kali ja Metaspoitable. Osoita testein, että 1) koneet eivät saa yhteyttä Internetiin 2) Koneet saavat yhteyden toisiinsa.

Lisäsin Kaliin toisen verkkokortin:

<img width="758" height="473" alt="image" src="https://github.com/user-attachments/assets/5e61040d-21ab-4361-abb8-2bc1e5ab6e9d" />

ja ennen käynnistystä NAT -verkkokortista "piuha" irti, jottei se saa yhteyttä internetiin:

<img width="760" height="467" alt="image" src="https://github.com/user-attachments/assets/cf83965f-7ae1-4312-9ccc-e2c4822af1ac" />

Varmistin, ettei Kalilla saa yhteyttä internetiin:

<img width="1012" height="429" alt="image" src="https://github.com/user-attachments/assets/37efdc30-28b4-4e88-9d0d-4ba8b3e2d72e" />

Sama varmistus Metasploitable2 koneelle:

<img width="488" height="119" alt="image" src="https://github.com/user-attachments/assets/a02265c7-b357-4a31-840f-f6d5bc7946ae" />

Sitten testasin että koneet pystyvät pingaamaan toisiaan ja Metasploitablen weppipalvelin vastaa:

<img width="1028" height="630" alt="image" src="https://github.com/user-attachments/assets/5a70f292-5b7d-4d9b-b07f-b6f34850e561" />

<img width="678" height="410" alt="image" src="https://github.com/user-attachments/assets/135f4e1a-8faa-4d32-84c6-36de488e8901" />

## d) Etsi Metasploitable porttiskannaamalla (nmap -sn). Tarkista selaimella, että löysit oikean IP:n - Metasploitablen weppipalvelimen etusivulla lukee Metasploitable.

Verkon osoite selvisi komennolla `ip a`, jossa Kalin osoite 192.168.56.101/24. Lähdin etsimään Metasploitable komennolla `nmap -sn 192.168.56.0/24`:

<img width="819" height="542" alt="image" src="https://github.com/user-attachments/assets/2fc04a15-f496-446a-93d3-d11e1bb2f168" />

Skannauksella löytyi neljä osoitetta: 192.168.56.1, 192.168.56.100, 192.168.56.101 ja 192.168.56.102.

Kävin osoitteet läpi ja weppipalvelimen etusivu löytyi osoitteella 192.168.56.102:

<img width="840" height="494" alt="image" src="https://github.com/user-attachments/assets/d9c952d6-12b0-414d-997e-f95d19ce8f7c" />

## e) Porttiskannaa Metasploitable huolellisesti ja kaikki portit (nmap -A -T4 -p-). Poimi 2-3 hyökkääjälle kiinnostavinta porttia. Analysoi ja selitä tulokset näiden porttien osalta. Voit hakea analyysin tueksi tietoa verkosta, muista merkitä lähteet.

Ajoin komennon `nmap -A -T4 -p- 192.168.56.102`. Tulokset:

```
──(kali㉿kali)-[~]
└─$ nmap -A -T4 -p- 192.168.56.102
Starting Nmap 7.98 ( https://nmap.org ) at 2026-04-03 11:04 -0400
Nmap scan report for 192.168.56.102
Host is up (0.00010s latency).
Not shown: 65505 closed tcp ports (reset)
PORT      STATE SERVICE     VERSION
21/tcp    open  ftp         vsftpd 2.3.4
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 192.168.56.101
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
22/tcp    open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey: 
|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
|_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
23/tcp    open  telnet      Linux telnetd
25/tcp    open  smtp        Postfix smtpd
|_smtp-commands: metasploitable.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN
| sslv2: 
|   SSLv2 supported
|   ciphers: 
|     SSL2_RC4_128_WITH_MD5
|     SSL2_RC2_128_CBC_EXPORT40_WITH_MD5
|     SSL2_DES_192_EDE3_CBC_WITH_MD5
|     SSL2_DES_64_CBC_WITH_MD5
|     SSL2_RC2_128_CBC_WITH_MD5
|_    SSL2_RC4_128_EXPORT40_WITH_MD5
| ssl-cert: Subject: commonName=ubuntu804-base.localdomain/organizationName=OCOSA/stateOrProvinceName=There is no such thing outside US/countryName=XX
| Not valid before: 2010-03-17T14:07:45
|_Not valid after:  2010-04-16T14:07:45
|_ssl-date: 2026-04-03T15:06:32+00:00; -1s from scanner time.
|_smtp-ntlm-info: ERROR: Script execution failed (use -d to debug)
53/tcp    open  domain      ISC BIND 9.4.2
| dns-nsid: 
|_  bind.version: 9.4.2
80/tcp    open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
|_http-server-header: Apache/2.2.8 (Ubuntu) DAV/2
|_http-title: Metasploitable2 - Linux
111/tcp   open  rpcbind     2 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2            111/tcp   rpcbind
|   100000  2            111/udp   rpcbind
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/udp   nfs
|   100005  1,2,3      52786/tcp   mountd
|   100005  1,2,3      60299/udp   mountd
|   100021  1,3,4      33994/tcp   nlockmgr
|   100021  1,3,4      45608/udp   nlockmgr
|   100024  1          46533/udp   status
|_  100024  1          53131/tcp   status
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp   open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
512/tcp   open  exec        netkit-rsh rexecd
513/tcp   open  login
514/tcp   open  shell       Netkit rshd
1099/tcp  open  java-rmi    GNU Classpath grmiregistry
1524/tcp  open  bindshell   Metasploitable root shell
2049/tcp  open  nfs         2-4 (RPC #100003)
2121/tcp  open  ftp         ProFTPD 1.3.1
3306/tcp  open  mysql       MySQL 5.0.51a-3ubuntu5
| mysql-info: 
|   Protocol: 10
|   Version: 5.0.51a-3ubuntu5
|   Thread ID: 11
|   Capabilities flags: 43564
|   Some Capabilities: SupportsTransactions, LongColumnFlag, Support41Auth, SupportsCompression, Speaks41ProtocolNew, SwitchToSSLAfterHandshake, ConnectWithDatabase
|   Status: Autocommit
|_  Salt: rp[V!;F/XLS>xgwD*DVe
3632/tcp  open  distccd     distccd v1 ((GNU) 4.2.4 (Ubuntu 4.2.4-1ubuntu4))
5432/tcp  open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
| ssl-cert: Subject: commonName=ubuntu804-base.localdomain/organizationName=OCOSA/stateOrProvinceName=There is no such thing outside US/countryName=XX
| Not valid before: 2010-03-17T14:07:45
|_Not valid after:  2010-04-16T14:07:45
|_ssl-date: 2026-04-03T15:06:32+00:00; -1s from scanner time.
5900/tcp  open  vnc         VNC (protocol 3.3)
| vnc-info: 
|   Protocol version: 3.3
|   Security types: 
|_    VNC Authentication (2)
6000/tcp  open  X11         (access denied)
6667/tcp  open  irc         UnrealIRCd
6697/tcp  open  irc         UnrealIRCd
8009/tcp  open  ajp13       Apache Jserv (Protocol v1.3)
|_ajp-methods: Failed to get a valid response for the OPTION request
8180/tcp  open  http        Apache Tomcat/Coyote JSP engine 1.1
|_http-server-header: Apache-Coyote/1.1
|_http-title: Apache Tomcat/5.5
|_http-favicon: Apache Tomcat
8787/tcp  open  drb         Ruby DRb RMI (Ruby 1.8; path /usr/lib/ruby/1.8/drb)
33994/tcp open  nlockmgr    1-4 (RPC #100021)
43743/tcp open  java-rmi    GNU Classpath grmiregistry
52786/tcp open  mountd      1-3 (RPC #100005)
53131/tcp open  status      1 (RPC #100024)
MAC Address: 08:00:27:9C:63:A2 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 2.6.X
OS CPE: cpe:/o:linux:linux_kernel:2.6
OS details: Linux 2.6.9 - 2.6.33
Network Distance: 1 hop
Service Info: Host:  metasploitable.localdomain; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_smb2-time: Protocol negotiation failed (SMB2)
| smb-os-discovery: 
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: metasploitable
|   NetBIOS computer name: 
|   Domain name: localdomain
|   FQDN: metasploitable.localdomain
|_  System time: 2026-04-03T11:06:24-04:00
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_clock-skew: mean: 59m59s, deviation: 2h00m00s, median: -1s
|_nbstat: NetBIOS name: METASPLOITABLE, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)

TRACEROUTE
HOP RTT     ADDRESS
1   0.10 ms 192.168.56.102

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 137.67 seconds
```

### Mielenkiintoisimmat portit

|PORT|STATE|SERVICE|VERSION|
|:--:|:---:|:-----------:|:-----:|
|21/tcp|open|ftp|vsftpd 2.3.4|
|23/tcp|open|telnet|Linux telnetd|
|139/tcp|open|netbios-ssn|Samba smbd 3.X - 4.X (workgroup: WORKGROUP)|
|445/tcp|open|netbios-ssn|Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)|

### 21/tcp FTP

Portti 21 kiinnitti huomioni ensimmäisenä:

```diff
-21/tcp    open  ftp         vsftpd 2.3.4
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 192.168.56.101
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
-|      Control connection is plain text
-|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
-|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
```

Huomiot:

- Control connection is plain text
  - Käyttäjänimet ja salasanat kulkevat salaamattomana.
- Data connections will be plain text
  - Myös tiedonsiirrossa data kulkee salaamattomana.
- ftp-anon: Anonymous FTP login allowed (FTP code 230)
  - Anonymous FTP -kirjautuminen sallittu, mahdollistaa pääsyn ilman henkilökohtaisia tunnuksia.
- Versio vsftpd 2.3.4
  - Tunnettu backdoor -haavoittuvuus CVE-2011–2523, mahdollistaa luvattoman pääsyn shelliin lähettämällä erityisesti muotoillun käyttäjänimen [lähde](https://medium.com/@ucheomaokoma_/exploiting-vsftpd-2-3-4-a-hands-on-penetration-test-on-a-backdoored-ftp-service-c869b2ffd43d).

### 23/tcp Telnet

- Vanhentunut etäyhteysprotokolla.
- Kaikki liikenne (käyttäjätunnukset, salasanat, data) kulkee salaamattomana, jolloin sitä on helppo salakuunnella.

### 139/tcp ja 445/tcp Samba smbd

```diff
-139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
-445/tcp   open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)

Host script results:
|_smb2-time: Protocol negotiation failed (SMB2)
| smb-os-discovery: 
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: metasploitable
|   NetBIOS computer name: 
|   Domain name: localdomain
|   FQDN: metasploitable.localdomain
|_  System time: 2026-04-03T11:06:24-04:00
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
-|_  message_signing: disabled (dangerous, but default)
```

- message_signing: disabled (dangerous, but default)
  - Viestien allekirjoitusta ei vaadita, jolloin palvelin on haavoittuva man-in-the-middle -hyökkäyksille tai SMB-relay -hyökkäyksille [Lähde](https://nmap.org/nsedoc/scripts/smb-security-mode.html).
- Versio Samba smbd 3.0.20-Debian
  - Tunnettu haavoittuvuus CVE-2007-2447, mahdollistaa hyökkääjän suorittaa mielivaltaisia komentoja shellin metamerkkien avulla [Lähde](https://nvd.nist.gov/vuln/detail/CVE-2007-2447).

## Lähteet

Ananthakrishnan. 19.8.2023. FTP control and Data connections. https://medium.com/@ananthakrish16ks/ftp-control-and-data-connections-f74ceb9eae95

Buuri, M. 31.3.2026. Dora and TLPT testing. https://terokarvinen.com/buuri-2026-dora-and-threat-lead-penetration-testing/buuri-2026-dora-and-threat-lead-penetration-testing--teros-pentest-course.pdf

Dora Regulation. https://eur-lex.europa.eu/eli/reg/2022/2554/oj/eng

Karvinen, T. 22.3.2026. Tunkeutumistestaus. https://terokarvinen.com/tunkeutumistestaus/

Nist. CVE-2007-2447 Detail. https://nvd.nist.gov/vuln/detail/CVE-2007-2447

Nmap.org. Script smb-security-mode. https://nmap.org/nsedoc/scripts/smb-security-mode.html

Okoma, U. 13.5.2025. Exploiting vsftpd 2.3.4: A Hands-On Penetration Test on a Backdoored FTP Service. https://medium.com/@ucheomaokoma_/exploiting-vsftpd-2-3-4-a-hands-on-penetration-test-on-a-backdoored-ftp-service-c869b2ffd43d

Suomen Pankki. 12.3.2025. TIBER-FI Procedures and Guidelines. https://www.suomenpankki.fi/globalassets/bof/en/money-and-payments/the-bank-of-finland-as-catalyst-payments-council/tiber-fi/tiber-fi-2.0-procedures-and-guidelines.pdf

Wikipedia. Telnet Security Vulnerabilities. https://en.wikipedia.org/wiki/Telnet#Security_vulnerabilities
