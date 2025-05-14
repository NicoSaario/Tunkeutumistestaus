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



## b) Lippuvalmistelu

- Jotta kaikki menee mahdollisimman sujuvasti, asensin Rufus - työkalua käyttäen USBitikulle Kali - Linuxin, jotta voin tarvittaessa hyödyntää Livetikkua ongelmien sattuessa. On kuitenkin kahden kurssin lipunryöstöt putkeen, niin ongelmia voi syntyä hyvinkin nopeasti.
- Asentelin uuden Kalin Oracle VM ja annoin sille 50gb muistia, 6 Prossua ja 4mb Ramia. Siihen on toistaiseksi ainoastaan asennettu ```kali-tools-wireless``` 
- 
