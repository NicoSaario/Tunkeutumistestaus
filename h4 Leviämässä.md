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

### € [Kennedy et al 2025: Metasploit: File-Format Exploits (sivun loppuun, eli Wrapping Up loppuun)](https://www.oreilly.com/library/view/metasploit-2nd-edition/9798341620032/xhtml/chapter9.xhtml#:-:text=File-Format%20Exploits)

### € [Singh 2025: The Ultimate Kali Linux Book: Understanding Active Directory (Vain tuo kappale, ei enää "Enumerating Active Directory")](https://learning.oreilly.com/library/view/the-ultimate-kali/9781835085806/Text/Chapter_12.xhtml#_idParaDest-272)

### € [Vapaaehtoinen: Kennedy et al 2025: Metasploit: Basic Meterpreter Commands](https://learning.oreilly.com/library/view/metasploit-2nd-edition/9798341620032/xhtml/chapter6.xhtml#toc-link_85)






## Lähteet: 

Cracking Passwords with Hashcat, Karvinen 2022, Luettavissa: https://terokarvinen.com/2022/cracking-passwords-with-hashcat/, Luettu 22/04/2025

Cracking File Password With John, Karvinen 2022, Luettavissa: https://terokarvinen.com/2023/crack-file-password-with-john/

€ Security Penetration Testing - The Art of Hacking Series LiveLessons: Lesson 6: Hacking User Credentials (8 videos, about 30 min), Santos et al 2017, Katsottavissa: https://learning.oreilly.com/videos/security-penetration-testing/9780134833989/9780134833989-sptt_00_06_00_00, Katsottu 21/04/2025

€ Metasploit: File-Format Exploits (sivun loppuun, eli Wrapping Up loppuun), Kennedy et al 2025, Luettavissa: https://learning.oreilly.com/library/view/metasploit-2nd-edition/9798341620032/xhtml/chapter9.xhtml#:-:text=File-Format%20Exploits

€ The Ultimate Kali Linux Book: Understanding Active Directory (Vain tuo kappale, ei enää "Enumerating Active Directory"),  Singh 2025, Luettavissa: https://learning.oreilly.com/library/view/the-ultimate-kali/9781835085806/Text/Chapter_12.xhtml#_idParaDest-272, Luettu 21/04/2025

€ Metasploit: Basic Meterpreter Commands, Kennedy et al 2025, Luettavissa: https://www.oreilly.com/library/view/metasploit-2nd-edition/9798341620032/xhtml/chapter6.xhtml#toc-link_85, Luettu 21/04/2025




