# Kotitehtävät

Kotitehtävät ovat kurssilta "Tunkeutumistestaus - Penetration Testing course" ja löytyvät osoitteesta https://terokarvinen.com/tunkeutumistestaus/.

> h6 Maalisuoralla

> Olet kohta läpäissyt yhden koulumme vaikeimmista kursseista. Tsemppiä loppumetreille!

> Rosvot menevät ali siitä, missä aita on matalin. Työkalupakista löytyy social engineering, fyysinen tunkeutuminen, jopa tiirikointi. Siksi meidänkin pitää tuntea nämä tekniikat.

    x) Lue/katso ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva kustakin artikkelista. Kannattaa lisätä myös jokin oma ajatus, idea, huomio tai kysymys.)
        Deviant Ollam. Vapaavalintainen esitys tai esitelmä esiintyjältä Deviant Ollam.
    a) Lippuvalmistelu. Valmistele kone ensi viikon lipunryöstöön. Tästä kohdasta ei tarvita kattavaa raporttia, riittää pelkkä luettelo siitä, miten ratkaisit allaolevat kysymykset. Jos sinulla on esimerkiksi valmis, toimiva Kali VM tavallisella PC:llä, tässä ei tarvitse tehdä juuri mitään.
        amd64. Haasteet voivat sisältää amd64-binäärejä. Nämä toimivat itsestään perus-PC:ssä, joita lähes kaikkien koneet ovat. Macintosheissa (M1, M2, M3...) on haasteita, jotka kannattaa ratkoa ennen lipunryöstöä.
        Netti. Koneesta pitää päästä nettiin, jotta voit palauttaa liput. Voi olla kätevää, että saat nettiyhteyden katkaistuksi, jotta porttiskannailu ym. on huoletonta.
        Too many secrets. Opettaja voi tarkastaa lipunryöstön koneesi ja tutkia kaikkia siellä olevia tiedostoja. Jos käytät pelkästään virtuaalikonetta, tarkastaminen koskee vain sitä. Katso, ettei tällä koneella ole luottamuksellisia tietoja.
        Koneella saa olla haluamasi ohjelmat ja työkalut. Työkaluista ja ohjelmista pitää antaa opettajalle kopio, jos ne eivät ole yleisesti käytettyjä, tunnettuja ja saatavilla olevia vapaita ohjelmia.
        Jos lipunryöstön koneella on muistiinpanoja, niistä on annettava kopio opettajalle sähköpostilla. Omia muistiinpanoja saa olla, mutta ei ole pakko. Lipunryöstössä saa lukea julkisia lähteitä, kuten opettajan kotisivuja sekä omia ja toisten julkisia raportteja.
        Paikallinen tekoäly. Omaa tekoälyä ei tarvita lipunryöstössä, eikä siitä todennäköisesti ole juuri hyötyä. Mutta siltä varalta, että joku miettii tätä: Paikallisen tekoälyn käyttö lipunryöstössä on sallittu, jos antaa opettajalle sähköpostitse täydellisen kopion keskustelusta tekoälyn kanssa ja kaikki paramaterit, sekä kopion tekoälyn koodista ja painoista tai linkin niihin. Ulkopuolisen tekoälyn käyttö verkon yli on lipunryöstössä kielletty.
    b) Oma korkki. Demonstroi tunkeutumista itse valitsemallasi luvallisella maalilla.


## x) Deviant Ollam

- Sen verran täytyy heti myöntää, että katsoin ensimmäisenä [tämän](https://www.youtube.com/watch?v=mvvVSTlbqEI) videon suojatiepainikkeen "hakkeroimisesta" ja se oli jokseenkin niin viihdyttävä, että jäin tutkimaan muita videoita vähän liiankin pitkäksi aikaa.

- Kanava on hyvin pitkälti painottunut lukkojen toimintaan, niiden murtamiseen ja murtamisen estämiseen, mutta törmäsin videoon joka liittyy niin jokapäiväiseen arkeen kun myös itse hakkerointiin.

- [Videossa](https://www.youtube.com/watch?v=k86UdUN6YlA) painotetaan yksinkertaisuuden merkitystä ja sitä, että yleensä ajattelemme asiat liian monimutkaisesti. Joskus ajaudumme urille, joissa uppoamme yhä syvemmälle ja syvemmälle kuiluun etsiessämme mitä monimutkaisempaa ratkaisua yksinkertaiseen asiaan. 

- Lähes kaikkea voi tehdä monimutkaisesti ja yksinkertaisen fiksusti. Jos käytetään tässä juuri nimenomaan hakkerin näkökulmasta, jotkut simppelit ratkaisut ovat jopa usein tehokkaampia, kuin monimutkaiset tavat hoitaa asioita. Kuvitellaan esimerkiksi vanha kunnon ```strings``` työkalu, jota käytin aikaisemmalla kurssilla enemmän; Voihan tiedoston viedä debuggeriin, lukea rivi riviltä ja yrittää selvittää haluttua asiaa, mutta saman voi usein ratkaista yksinkertaisesti ja nopeasti vain muutaman sekunnin työllä.

- Videossa siis käytetään esimerkkinä sitä, kun Deviant uppoutui yksinkertaiseen ongelmaan, jossa haluttiin helpottaa johtojen viemistä maan pintaa vasten eräänlaisilla "puutarhaniiteillä". Ensin hän mietti ratkaisua pitkään, mietti 3D - printtausta, mekanismien rakentamista ja hyvin monimutkaista rakennelmaa -> Lopulta hän keksi yksinkertaisen ratkaisun ja teki harjan varresta sekä "binder clips", eli paperiklipsistä yksinkertaisen viritelmän, jolla ongelma ratkesi kahdessa minuutissa ja kustannustehokkaasti.

## b) Lippuvalmistelu

- Jotta kaikki menee mahdollisimman sujuvasti, asensin Rufus - työkalua käyttäen USBitikulle Kali - Linuxin, jotta voin tarvittaessa hyödyntää Livetikkua ongelmien sattuessa. On kuitenkin kahden kurssin lipunryöstöt putkeen, niin ongelmia voi syntyä hyvinkin nopeasti.
- Asentelin uuden Kalin Oracle VM ja annoin sille 50gb muistia, Base memory 4700MB ja 6 Prossua. Siihen on toistaiseksi ainoastaan asennettu ```kali-tools-wireless``` 

- Tein vielä lopuksi pienen cheat - sheetin, kun kertailin kurssilla tehtyjä tehtäviä

https://github.com/NicoSaario/Tunkeutumistestaus/blob/main/Oma_Cheat_Sheet.md

## b) Oma korkki. Demonstroi tunkeutumista itse valitsemallasi luvallisella maalilla.
- Olisin halunnut tähän demota jotain oikeata, fyysistä laitetta, mutta sen kanssa leikin enemmän sitten omalla ajalla. Haluan kuitenkin varmistua siitä, että kaikki menee eettisesti, joten se vaatii paljon enemmän vielä taustatyötä.

- Valitsin siis maaliksi jälleen Metasploitable 2, jota aikaisemmissakin harjoituksissa on käytetty. Tällä kertaa ei kuitenkaan ole minkäänlaista kohdehaavoittuvuutta, joten seulon, tutkin ja etsin sellaisen itse.

- Nappasin koneet verkosta pois

![image](https://github.com/user-attachments/assets/4439a857-2e26-40b8-8577-1a67206b7795)

- Pitää testata niiden välinen yhteys

- Lähdin siis selvittämään sen Metasploitablen IP - osoitetta. Haluan tehdä tämän niin, kuin pääsyä koko laitteelle ei olisi.
- Haen ensin ```ip a``` - komennolla kaikkien interfacejen IP - osoitteet

![image](https://github.com/user-attachments/assets/277b3c7d-abdb-4979-81be-bda8ca36c086)

- Löydetään eth1, joka on aktiivisena ja sen IP
- Seuraavaksi skannaillaan ja yritetään selvittää Metasploitable2 ip -> ```nmap -sn 192.168.118.5/24```
- Paljastuu kolme laitetta -> Unknown oletettavasti on se Gateway, jota pitkin tässä mennään, sillä .5 - loppuinen (3) on host eli Kali ja .4 (2) on se metasploitablen IP.

![image](https://github.com/user-attachments/assets/6ed149e5-104f-40a2-8528-0a59062802a0)

- Se siis löytyi!

- Käydään nyt vielä varuiksi varmistamassa se Metasploitablella, koska ollaan kuitenkin jo maaliin päästy -> ```ifconfig``` ![image](https://github.com/user-attachments/assets/5cc52202-03c7-4beb-b077-e46562aab179)

- Käytän sitä metasploitablen ip:tä ja teen porttiskannauksen ```nmap -A -T4 -sN ip```
- Paljon löytyy haavoittuvuuksia, jota hyödyntää. Pähkäilen tässä jotain mielenkiintoista, mihin tarttua kiinni
- Hetken selailtua, päädyin lopulta tähän

![image](https://github.com/user-attachments/assets/c771ea9d-463d-400d-86f2-b328a3f0ba57)

- Nyt pitää hetki surffailla, miten siihen pääsee kiinni. Olettaisin, että ihan vain tcp tai käyttämällä vaikkapa Ncattia, joka on siis intergoitu nmappiin. 
- Ajattelin kokeilla kepillä jäätä ja Ncatin manuaalista lueskeltuani löin vaan suoraan ```nc ip portti```

![image](https://github.com/user-attachments/assets/91663d98-712f-41c7-b1ff-a8a10d42dea3)

- Ollaan siis heti roottina siellä kiinni. Tutkin vähän kansioita tässä välissä.

- Löysin tiedoston passwd-, jossa kätevästi näkyi jokainen käyttäjä ja salasanan sijainti

![image](https://github.com/user-attachments/assets/e901ce56-41e1-4c88-aa8e-fe4358d78034)

- Päätin tehdä seuraavaa -> Vien tiedostot passwd ja shadow Kalille. Aloitan JohnTheRipperillä availemaan niitä ja jatkan Metasploitilla tutkimista. Mietin jonkun kivan haittaohjelman tekemistä sinne.

Käytännössähän en tarvitse edes kauheasti noita salasanoja, sillä on jo muutenkin roottikäyttäjän oikeudet ja voin tehdä mitä vain järjestelmässä, mutta halusin harjoituksen muodossa niitä availla. Onhan siitä se hyöty, että jos tämä ovi sulkeutuu, jää muita auki. 

- Säästääkseni vähän aikaa, raportoin tässä kohtaa vähän jäljessä. Eli loin Python HTTP - palvelimen, jolla hain Kalilla tiedostot. Ensin tietysti ```cp molemmat tiedostot haluttuun kansioon```

- Kalilla navigoin selaimella Metasploitablen osoitteeseen ja latasin tiedostot

- Tein tiedostoista yhtenäisen komennolla ```unshadow passwd shadow-file > john.input.txt```ja aloitin skannauksen ```john --wordlist=/usr/share/wordlists/rockyou.txt```

- Koska uusi Kali, rockyou.txt piti ```gunzip rockyou.txt```


Tiedostot selaimesta
![image](https://github.com/user-attachments/assets/84b6da26-de4d-4e4b-abf2-121a361bc829)


Metasploitable - näkymä
![image](https://github.com/user-attachments/assets/e9cc6309-ac98-4e26-ad73-133c26229ff8)


Pari sieltä jo availtu
![image](https://github.com/user-attachments/assets/23abbc2e-0fe2-4042-97b6-f717ae452cce)

Siinä itseasiassa näkyy sudo-oikeuksien omaava käyttäjä, jonka testimielessä loin hetki sitten eli tuo "hiding", jolla salasana myös "hiding".

![image](https://github.com/user-attachments/assets/c658f9fe-5ed6-4c34-a769-9a274472cdbc)


Tulos ei paljoa muuttunut, eli neljä käyttäjää salasanoineen sieltä löytyi

----------------------------------------------------------------------------------------------------------------
Tässä kohtaa tein kotitehtäväpalautuksen, homma jatkuu yhä
----------------------------------------------------------------------------------------------------------------

- Eli siis ideana tässä itselläni oli se, että sikälimikäli tuo bind shell backdoor suljetaan, pystyn käyttämään vaihtoehtoisia tapoja sisälle pääsyyn. Avasin ftp:n servicellä. Se toimii hyvin. Tämän jälkeen testasin sitä hiding - käyttäjää, jonka loin ja sekin toimi.

![image](https://github.com/user-attachments/assets/e60898bb-bca6-415c-86fc-6505b312f27a)

![image](https://github.com/user-attachments/assets/e4869c89-92b5-44c9-b062-e632bc3bb952)

![image](https://github.com/user-attachments/assets/d0f634aa-5b2d-4840-8805-accb1fdc3918)

- Joudun itseasiassa toistaiseksi lopettamaan tähän ja valmistautumaan noihin lipunryöstöihin. Mukava kurssi ja antaa kyllä eväitä harjoitella näitä jatkossa itsenäisesti lisää!

## Lähteet

Kotitehtävät - Tunkeutumistestaus, Tero Karvinen, https://terokarvinen.com/tunkeutumistestaus/

Hacker Fun with Traffic Controls and Crosswalk Buttons, DeviantOllam, 2024, Katsottavissa: https://www.youtube.com/watch?v=mvvVSTlbqEI, Katsottu 14/05/2025

The Importance of Solving Problems in Smart But Also Dumb Ways, 2023, DeviantOllam, Katsottavissa: https://www.youtube.com/watch?v=k86UdUN6YlA, Katsottu 14/05/2025


