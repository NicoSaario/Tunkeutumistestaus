# Kotitehtävät

Kotitehtävät ovat kurssilta "Tunkeutumistestaus - Penetration Testing course" ja löytyvät osoitteesta https://terokarvinen.com/tunkeutumistestaus/.

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 01/04/2025 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+3) 01/04/2025 (pvm/kk/v)

Kaikissa testaukseen liittyvässä: Oracle VM VirtualBox ja Kali Linux Point release live image (2025.1a)

## h1 Kybertappoketju

*Kill Chain #1: Recon. Porttiskannauksen lisäksi alamme rakentaa hakkerilabraa.*

Tehtäviä saa aloittaa vasta, kun on hyväksynyt kurssin säännöt. Koneet on eristettävä Internetistä hyökkäysten harjoittelun ajaksi.

    x) Lue/katso/kuuntele ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)
        
        Hutchins et al 2011: Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains, chapters Abstract, 3.2 Intrusion Kill Chain.
        € Santos et al: The Art of Hacking (Video Collection): 4.3 Surveying Essential Tools for Active Reconnaissance. Sisältää porttiskannauksen. 5 videota, yhteensä noin 20 min.
        KKO 2003:36.

### Herrasmieshakkerit https://www.withsecure.com/fi/whats-new/podcasts/herrasmieshakkerit (RSS) tai Darknet Diaries https://darknetdiaries.com/episode/ (RSS) , yksi vapaavalintainen jakso jommasta kummasta. Voi kuunnella myös lenkillä, pyykiä viikatessa tms. Siisti koti / hyvä kunto kaupan päälle.

- Valitsin Herrasmieshakkereilta Tietoturvan Niksipirkka - nimisen jakson, jossa vieraana Keskon tietoturvajohtaja Juho Rikala.
- Aluksi puhuttiin siitä, voiko rikoksia tehnyt hakkeri olla vielä hyvä ja korostettiin tiimin luottamuksen merkitystä.
- Aikaisemmin tietosuojamaksut eivät ole koskeneet julkista sektoria, joka on vähän omaankin korvaan erikoista. Kuten podcastissa mainitaan, on hyvin epämääräistä, ettei julkishallinnon toimijoille tule seuraamusmaksuja,
vaikka he käsittelevät suuria määriä ihmisten dataa.
- Ensimmäiset 15 - minuuttia oikeastaan käsittelee vain Keskoa, heidän palveluitaan ja merkitystä IT - maailmassa.
- Tämän jälkeen puhutaan siitä, mitä dataa asiakkaista kerätään. Asiakas antaa itse luvan tehdä niin. Kysymyksenä oli se, että kerääkö esimerkiksi K-plussakortti asiakkaasta tietoja ja annetaan tarjouksia sekä bonuksia, jotta asiakaasta saataisiin dataa kerättyä.
- Vähän ympäripyöreä vastaus, mutta lopputuloksena kyseessä on enemmänkin se, että saadaan asiakas pysymään Keskolla ja saamaan hyötyä kerätystä datasta.
- Pääpointtina se, että datamäärän ollessa hyvin laaja, sen pitää olla hyvin hajautettu ja pääsy vain rajatuilla henkilöillä.
- Toivottavasti en ammu itseäni jalkaan, mutta kyllä tämä aika pitkälti vaikutti vain Keskon mainokselta.
- Jätetään tää nyt tähän, mutta kuuntelen luultavasti vielä toisen podcastin.

###  Hutchins et al 2011: Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains, chapters Abstract, 3.2 Intrusion Kill Chain. https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf

- Kybertappoketju on systemaattinen prosessi jolla pyritään kohdistamaan ja sitouttamaan kohde, jotta saadaan haluttu vaikutus aikaiseksi.
- Vaiheet on määritelty seuraavasti:
  1. Etsi - Tunnista kohteet, jotka soveltuvat hyökkäykseen
  2. Paikanna - Määritä tarkka sijainti
  3. Seuraa - Havainnoi ja kerää tietoja kohteesta
  4. Kohdenna - Valitse sopiva keino tai resurssi vaikutuksen aikaansaamiseksi
  5. Sitoudu - Toteuta hyökkäys
  6. Arvioi - Tarkastele ja arvioi vaikutus
- Jos yksikin vaihe puuttuu, ketju katkeaa ja voi keskeyttää koko prosessin
- Tunkeutumisen tarkoituksena on se, että hyökkääjä kehittää hyötykuorman, jotta voi murtautua sisään, vakiinnuttaa asemansa, suorittaa tavoitteitaan liikkumalla ympäristössä ja vaarantamalla järjestelmän CIA (Confidentiality, Ingegrity, Availability)

- Koostuu seuraavista vaiheista:
  
  1. Reconnaisssance  (Tiedustelu) -> Tutkiminen, tunnistaminen ja valinta -> Verkkosivujen, s-postilistan selaaminen, some tai muut tiedot

  2. Weaponization (Aseistaminen) -> Etätroijalainen, esim. PDF - ja M Office - tiedostot

  3. Delivery (Toimitus) -> Troijalaisen toimittaminen -> S-postiliitteellä, Verkkosivulla, USB-tikulla esim.
    
  4. Explotation (Hyväksikäyttö) -> Koodi aktivoituu

  6. Installation (Asennus) -> Takaoven tai troijalaisen asentaminen järjeselmään -> Pysyvä pääsy

  7. Command and Control, C2 (Komentoyhteys) -> Vaarantunut laite lähettää signaaleja ohjauspalvelimille -> C2 kanava -> Suora pääsy kohdeympäristöön

  8. Actions on Objectives (Tavoitteisiin kohdistuvat toimet) -> Tavoitteiden toteuttaminen -> Tietojen varastaminen, kerääminen, salaaminen, poistaminen -> Verkossa liikkuminen, muut kohteet
 
 ### € Santos et al: The Art of Hacking (Video Collection): 4.3 Surveying Essential Tools for Active Reconnaissance. Sisältää porttiskannauksen. 5 videota, yhteensä noin 20 min. https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00

- Eroaa passiivisesta siinä, että vasta aktiivisessa vaiheessa lähetetään tietoa kohteen verkkoon, eli skannataan portteja, tutkitaan haavoittuvuuksia, kuunnellaan verkkoa ja se antaa kohteelle samalla infoa ja hälytyksiä, jos joku tutkii logeja
- Joskus tarkoituksella halutaan jäädä kiinni, sillä moni asia "lakaistaan maton alle"
- Pitää tehdä huolellinen taustatyö ja suunnittelu laitteista ja verkoista. Muuten tulee liikaa informaatiota ja mahdollisia kohteita kerralla -> Mitä kauemmin etanoit, sitä helpommin kiinni
- Porttiskannaus -> Halutaan nähdä, että passiivinen tiedustelu tuotti tulosta ja että ne portit ovat oikeasti auki. Luultavasti nähdään muitakin auki olevia portteja. Weppisovellusten skannaus -> Mihin halutaan hyökätä? -> Haavoittuvuuden skannaus -> Älä hypi vaiheiden ohi!

Työkaluja:
- Porttiskannaus
1. Nmap -> Suosituin, mukautuvuin ja vakain! -> https://nmap.org/ -> Sillä voi myös OS ja haavoittuvuuksia selvittää
   - ```nmap -sS -vv -T4 -A <ip-osoite>``` lopuksi aina se IP - osoite.
   - ```-sS``` tarkoittaa TCP SIN - skannausta "Half-open-connection"
   -  ```-vv``` - Näkee ja tietää tasan tarkkaan, mitä skannauksessa tapahtuu ja nopeuttaa
   -  ```T4``` - nopeuttaa yhä enemmän
   -  ```A``` OS - tunnistaminen, version tunnistaminen, skriptiskannaus ja traceroute ```
   -  ```-Pn``` -> Treat all host as online -- palomuurit blokkaa yleensä tietyt, joten jos tätä ei laita, skannaus saattaa skipata ne
   -  OUTPUT -> Tärkeä! Voit tehdä siitä tiedoston, vaikka TXT
   -  ```-p <port ranges>```tietyt portit ```-p1-66535;```skannaa kaikki portit

    
3. Masscan -> https://github.com/robertdavidgraham/masscan.git -> Nopein!
   - ```masscan``` hyvin samantyylinen, kuin nmap
   - ```man masscan``` - manuaali
   - ```masscan -p80,443 <ip-osoite> --rate=10000```
     
5. Udpprotoscanner -> https://github.com/portcullislabs/udp-proto-scanner.git -> Nopein UDP porttiscanneri
   ```./udp-proto-scanner.pl <ip-osoite>```

-  Web service review
1. EyeWitness https://github.com/ChrisTruncer/EyeWitness
   - Työkalu näyttää nopeasti ne, mihin kannattaa priorisoida
   - Käy jokaisella sivulla, ottaa screenshotit
   - Porttiskannaus -> tekstitiedostoon -> cat ip_list.txt -> ```./EyeWitness.py --web -f ip_list.txt```
  
- Network vulnerability scanners
  1. OpenVAS - Free & Open Source
  2. Nessus - maksu
  3. Nexpose - maksu
  4. Qualys - maksu
  5. Nmap - rajoitettu
     - ```sC``` - scriptscan ```ls usr/share/nmap/scripts```
     - Webbisivulta nopea kattoa https://nmap.org/nsedoc/
     - Sivulta näkee suoraan ja voi ladata scriptit koneelle. Hyvät ohjeet myös
 
- Web Vulnerability Scanners
  1. Nikto - Suosituin, Helppokäyttöinen
  2. WPScan - WordPress
  3. SQLMap - Pentest Tietokantoihin - SQL injektioita
  4. Burp Suite - Standardeja, erittäin suosittu, web application proxy myös
  5. Zed Attack Proxy - -||- 
### Lähteet

Herrasmieshakkerit, Tietoturvan Niksipirkka, vieraana Juho Rikala | 0x34, 25/09/2024, Kuunneltavissa: https://podcasts.apple.com/fi/podcast/herrasmieshakkerit/id1479000931, Kuunneltu 01/04/2025
