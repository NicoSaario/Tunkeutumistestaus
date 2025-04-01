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
 
### https://finlex.fi/fi/oikeuskaytanto/korkein-oikeus/ennakkopaatokset/2003/36
- Osuuspankkikeskus-OPK tietojärjestelmiin yrittänyt tunkeutua porttiskannauksella etsien avoimia välityspalvelimia.
- Käytännössä yritys tulkitaan suoraksi teoksi
-  Markka-aika, joten 75 000 markkaa tuli korvauksia maksetavaksi
-  Ei kannata leikkiä edes huvikseen.

## a) Asenna Kali virtuaalikoneeseen. (Jos asennuksessa ei ole mitään ongelmia tai olet asentanut jo aiemmin, tarkkaa raporttia tästä alakohdasta ei tarvita. Kerro silloin kuitenkin, mikä versio ja millä asennustavalla. Jos on ongelmia, niin tarkka ja toistettava raportti

- Asensin sen jo aikaisemmin
- Tämän iso - tiedoston ![image](https://github.com/user-attachments/assets/e87d54b5-f181-404e-b56a-c81d1dab3a23)
- Osoitteesta https://www.kali.org/get-kali/#kali-live
- Ei ongelmia, päivitykset ajettu tiivistelmien aikana

## b) Irrota Kali-virtuaalikone verkosta. Todista testein, että kone ei saa yhteyttä Internetiin (esim. 'ping 8.8.8.8')

- Muodon vuoksi ensin, että kaikki toimii

- ![image](https://github.com/user-attachments/assets/30ec7bd7-83ed-4d71-83d8-d4b8a40ffd18)

- Nopein tapa

- ![image](https://github.com/user-attachments/assets/fe5bd44c-2d19-4e93-9ae7-0f50c0e4f892)

- ![image](https://github.com/user-attachments/assets/b059b366-d883-48ae-b44d-71f4ea647341)

- Ja tuplavarmistus

- ![image](https://github.com/user-attachments/assets/4af14fce-3a00-4f36-aa45-0204ef443e86)

## c) Porttiskannaa 1000 tavallisinta tcp-porttia omasta koneestasi (nmap -T4 -A localhost). Selitä komennon paramterit. Analysoi ja selitä tulokset.
- Hyvistä muistiinpanoista johtuen, voin suoraa ylempää luntata parametrit, eli:
      - ```nmap``` kutsuu nmappia
       - ```-A``` OS - tunnistaminen, version tunnistaminen, skriptiskannaus ja traceroute
      - ```-T4``` -> Timing "aggressive", eli nopeuttaa hakua todella paljon
  
- ![image](https://github.com/user-attachments/assets/065204fc-194c-42bc-affc-e23a7ae35b86)
1. Näkee, että Nmappi ja sen versio aktivoituu -> Mihin kellonaikaan
2. Ei DNS - servereitä, koska ei olla verkossa
3. Mitä tapahtui, eli scan tulokset localhostille osoitteessa 127.0.0.1
4. Löysi Hostin 0.0000047s viiveellä
5. Löysi myös muuta, mutta jätti yhden skannaamatta
6. Kaikki portit ignored - statella -> Ilmeisesti palomuuri estänyt yhteyden ja ne ovat kiinni (Lähde: https://forum.hackthebox.com/t/nmap-all-ports-are-in-ignored-state/272778/8)
7. Suljettuja portteja 1000, mutta koska reset, ne reagoivat
8. Ei voi yksilöidä OS, joten se ei voi antaa tarkkaa tunnistetta
9. Tehtiin samalla koneella, ei hyppyjä verkoissa
10. Summa summarum, eli operaatio haki sen localhost IP ja skannasi sen 2.02 - sekunnissa

## d) Asenna kaksi vapaavalintaista demonia ja skannaa uudelleen. Analysoi ja selitä erot.

- Eli pitää kytkeä Kali takaisin verkkoon reversetyylillä, mitä aiemmin
- Asentaa ulkomuistista ssh ja apache2
- ```sudo apt-get -y install ssh apache2```
- Tsekata niiden status, koska uskon, että niiden täytyy olla päällä ollakseen vaiktutusta
- ![image](https://github.com/user-attachments/assets/b4273640-806f-4a08-9926-24a5978882e5)
- Ja lopuksi ```sudo systemctl enable --now ssh apache2```, joka aktivoi ne päälle

- Nähdään, että ne ovat päällä
- ![image](https://github.com/user-attachments/assets/d5976410-8105-4c46-813b-f87fa440387b)

- Taas verkko pois päältä -> Pingitestit ja Firefox

- ![image](https://github.com/user-attachments/assets/f611d965-3721-4698-afd2-fdd758d61b6a)
- Anteeksi jo etukäteen näistä olemattomista Gimp - taidoista
  
1. Sama alku, eli mitä on tapahtunut ja kellonaika ja lisäsin myös DNS - kohdan tähän
2. Raportti mistä
3. Kesti kauemmin
4. Ei skannattu 1 edelleen
5. Kaksi porttia näytetään, eli niissä on jotain
6. Sieltä löytyi äsken aktivoitu SSH portista 22/tcp ja se portti on auki! Näkyy myös lisätietoa, eli OpenSSh versio ja millä, eli Debian 1 protocolla 2.0. Näkyy myös ssh julkiset avaimet
7. Ja täältä puolestaan Apache2! Sen portit 80/tcp ja sekin portti on auki! Myös näkyy, miten se pyörii, missä se pyörii ja etenkin se, että se toimii "It works!"
8. Lopuksi nyt päästiin saamaan jopa OS - tietoja. Eli että Linuxissa pyöritään 2.6.32 ja 6.2 - version välillä. Käsittääkseni se arvioi version olevan 2.6x tai 5.X. Onhan se aika iso haarukka, mutta kertoo silti jotain. 
9. Skannaus kesti yhdessä osoitteessa 8.4 sekuntia, joten "huomattavasti" kauemmin aikaisempaan verrattuna


## e) Asenna Metasploitable 2 virtuaalikoneeseen

- Tähän väliin hauska kommentti, jonka löysin ladatessani Metasploit 2 täältä: https://sourceforge.net/projects/metasploitable/reviews/  ![image](https://github.com/user-attachments/assets/9adf8682-d492-4e2c-8f21-41bcfe3126db)

- Metaspoit 2 on siis tarkoituksellisesti tehty haavoittuvaksi ja pyörii Ubuntu Linux virtuaalikoneena. Sitä käytetään testaamaan yleisiä haavoittuvuuksia. Lähde: https://docs.rapid7.com/metasploit/metasploitable-2/ 

- Ei itseasiassa tähän mennessä oo tullut asenneltua muuta, kuin suoraa ISO - imagesta, joten hetken joutu tutkaileen, et mistä edetään
- Näillä valinnoilla eteenpäin ![image](https://github.com/user-attachments/assets/59ea0a2a-5e90-40a7-807e-d0075403d9e0)
- Tommosilla valinnoilla ![image](https://github.com/user-attachments/assets/861d6026-2865-41d4-8d39-f60bb02aca4f)
- Piti muuttaa nimee, koska Oracle ei hyväksyny tota _
- Tässä tuli pieni mutta vastaan, koska en pystynyt lisäämään tuota vmdk - tiedostoa, vaikka se on oikeassa formaatissa
  ![image](https://github.com/user-attachments/assets/b850be5f-8c14-41bf-a560-b5b571ec464f)
- Ei hätää! Hetken aikaa, kun yritin sitä etsiä, päädyin Storage - kohtaan ja löysin sen Hard Disk Selectorista.
- ![image](https://github.com/user-attachments/assets/54de2206-9fe0-440d-b730-9be88575cf49)
- Se ei vieläkään toiminut, joten kävin poistamassa vdi:n, jonka olin aiemmin lisännyt
- Tässä kävikin niin, ettei Oraclella ollut oikeuksia lukea kansiota. Tutkin tätä ohjetta: https://medium.com/@dangerhulk26022022/installing-metasploitable-2-in-virtual-box-for-windows-host-bf0a5b4f8375 ja hetken pohdittua ja kokeiltua noin 15 - kertaa asentaa sitä uudelleen, päätin unzipata eri kansioon ja testata, toimiiko se. Kaikki näytti hyvältä ja jopa "Starting up ... " - viestin, kunnes tajusin, että tässä tais tulla uus vika ja se syy Debian. Starting up ei siis vienyt mihinkään noin 5 - minuuttiin. Eli jos kyseessä on Ubuntu Linux, toimiiko Debian? Vähän epäilen
- Katsoin vielä varmuuden vuoksi https://tuomasvalkamo.com/PenTestCourse/week-2/ ohjeita, mutta eipä siitä kummoista apua ollut. Metasploitable ei siis vieläkään suostu käynnistymään.
- Harmaita hiuksia hetken haroneena, kävin tekoälylle syöttämässä tuon virheilmoituksen
- ![image](https://github.com/user-attachments/assets/54c7c7b4-4dd3-4089-938c-6c69a7ee55e9)
- Se ehdotti ottaa tuon pois. Lisäksi huomasin virheilmoitukset tuossa alhaalla, joten kävin ne korjaamassa niiden mukaan. Eli -> Display Graphics Controller: VMSVGA ja System -> Processors: 1
- Ennustaminen johtaa yleensä turmioon, mutta nyt Starting up vilkkuu enemmän, kuin 5 - sekuntia. Näyttää hyvältä!

Kirjoitin siis Promptiksi ChatGPT-4o minille  suurinpiirtein sen virheilmoituksen, jonka sain:
> .MP-BIOS bug: 8242 timer not connected to IO-APIC
7.860159 Kernel panic - not syncing : IO-APIC + timer doesn't work! Boot with apic=debug and send a report. Then try booting with the noapic option
Kyseessä siis Oracle VM ja yritän käynnistää Metasploitable 2

- Johon se vastasti:

> Tämä virhe (.MP-BIOS bug: 8242 timer not connected to IO-APIC) liittyy BIOS- ja APIC (Advanced Programmable Interrupt Controller) -asetuksiin Oracle VM VirtualBoxissa, ja se voi estää virtuaalikoneen käynnistymisen oikein. Tämä virhe ilmenee erityisesti vanhemmilla käyttöjärjestelmillä ja virtuaalikoneilla, kuten Metasploitable 2:lla.

> Tässä on muutama mahdollinen ratkaisu ongelmaan:
> 1. Poista APIC käytöstä

> Tämä voi ratkaista ongelman, koska Metasploitable 2 on vanhempi virtuaalikone, joka ei aina toimi oikein APIC:n kanssa. Voit poistaa APIC:n käytöstä käynnistyksen yhteydessä.

    Avaa VirtualBox ja valitse virtuaalikoneesi (Metasploitable 2).

    Mene Asetuksiin (Settings).

    Siirry Järjestelmä-välilehdelle (System).

    Valitse Emulaatio-välilehti ja poista valinta Enable IO APIC-kohdasta.

    Yritä käynnistää virtuaalikone uudelleen.

> Tämä poistaa APIC:n käytöstä, mikä voi ratkaista ongelman.

- Ehdotti se paljon muutakin, mutta niitä ei tähän tarvittu, enkä lähtenyt edes kokeilemaan. Sen verran kryptiseltä näytti. 
- Hetken pohdiskelin ja mietin, kunnes luin Teron ohjeita, jossa antaa hyvänä vinkkinä Kalin kahden verkkokortin hyödyntämistä ja "Host only networking" - tekemistä https://terokarvinen.com/tunkeutumistestaus/
- ![image](https://github.com/user-attachments/assets/1dbec070-e034-4f0a-81ca-a698f1fd8b15)
- Tuo siis Kalille tehty. Seuraavaksi Metasploitable 2 Networking - asetukset
  
- Eli jos nyt oikein ymmärsin omasta päättelystä, tämän pitäisi toimia
  ![image](https://github.com/user-attachments/assets/bb0f4a6c-41ca-4bf5-99bd-727c6580f961)


### Lähteet

Herrasmieshakkerit, Tietoturvan Niksipirkka, vieraana Juho Rikala | 0x34, 25/09/2024, Kuunneltavissa: https://podcasts.apple.com/fi/podcast/herrasmieshakkerit/id1479000931, Kuunneltu 01/04/2025


Hackthebox,  NMAP all ports are in ignored state, Comment by user "adamkirito" 2/2023, Luettavissa:  https://forum.hackthebox.com/t/nmap-all-ports-are-in-ignored-state/272778, Luettu 01/04/2025

Metasploitable 2, RAPID7, Luettavissa: https://docs.rapid7.com/metasploit/metasploitable-2/

https://medium.com/@dangerhulk26022022/installing-metasploitable-2-in-virtual-box-for-windows-host-bf0a5b4f8375
