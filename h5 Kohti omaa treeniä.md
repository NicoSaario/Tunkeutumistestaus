# Kotitehtävät

Kotitehtävät ovat kurssilta "Tunkeutumistestaus - Penetration Testing course" ja löytyvät osoitteesta https://terokarvinen.com/tunkeutumistestaus/.

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 04/05/2025 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+3) 04/05/2025 (pvm/kk/v)

Kaikissa testaukseen liittyvässä: Oracle VM VirtualBox ja Kali Linux Point release live image (2025.1a)

## x) Lue/katso ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva kustakin artikkelista. Kannattaa lisätä myös jokin oma ajatus, idea, huomio tai kysymys.)

    Karvinen 2025: Start Your Research with a Review Article
    Review. Etsi vapaavalintainen review eli katsausartikkeli, joka liittyy kurssin aiheisiin.
        Artikkelin pitää olla JUFO-arvioidusta julkaisusta eli jufo-taso 1, 2 tai 3.
        Mieluiten tuore, julkaisusta alle 2 vuotta.
        Artikkeleita löytyy scholar.google.com/ncr
        Jos artikkeli on pitkä, yli 4 sivua, voit perustaa tiivistelmäsi silmäilyyn, kunhan mainitset tästä tiivistelmässäsi.


## a) HTB Dancing. Ratkaise HackTheBox.com: Starting Point: Tier 0: Dancing.

- Tein sinne käyttäjän ja vinkkien mukaan yhdistän OpenVPN - kautta Kalista.
- Eli siis HackTheBoxin sivuille -> Haluttuun labraan, siitä valitaan "OpenVPN" ja mennään ohjeiden mukaan
- Serveriksi vaihdoin Eu

![image](https://github.com/user-attachments/assets/5a58d7f6-466b-4dc5-98b4-a669403258ed)

- Eli ladataan ovpn - tiedosto -> CLI ->  ```openvpn + tiedostopolku``` ja pari kertaa testattua, se heittää erroria, jos ei aja roottina, eli lisätään siihen vielä ```sudo openvpn + tiedostopolku/tiedosto```

- Piti päivittää sivu, jotta tulee vihreää valoa HTB - kautta, että yhteys on luotu
- Starting - Point - labroja saa vapaasti jakaa [sääntöjen](https://app.hackthebox.com/rules) mukaan, joten siitä ei pelkoa

- Teen tästä muut alta pois ennen tuota tehtävää, jotta saan vähän pohjaa näiden suorittamiseen.
- Ekassa piti vastailla kysymyksiin, skannata portit ja ottaa telnet - yhteys roottikäyttäjällä, jonka jälkeen sai lipun
- Tokassa käyttää aukinaisena olevaa ftp - porttia ja hakea sieltä flägitiedosto




