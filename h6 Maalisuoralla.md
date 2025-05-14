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






## Lähteet

Hacker Fun with Traffic Controls and Crosswalk Buttons, DeviantOllam, 2024, Katsottavissa: https://www.youtube.com/watch?v=mvvVSTlbqEI, Katsottu 14/05/2025

The Importance of Solving Problems in Smart But Also Dumb Ways, 2023, DeviantOllam, Katsottavissa: https://www.youtube.com/watch?v=k86UdUN6YlA, Katsottu 14/05/2025


