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
- Tunkeutumisen pääydin on se, että hyökkääjän on kehitettävä hyötykuorma, jotta voi murtautua rajojen sisäpuolelle, vakiinnutettava läsnäolonsa ja suoritettava tavoitteitaan

### Lähteet

Herrasmieshakkerit, Tietoturvan Niksipirkka, vieraana Juho Rikala | 0x34, 25/09/2024, Kuunneltavissa: https://podcasts.apple.com/fi/podcast/herrasmieshakkerit/id1479000931, Kuunneltu 01/04/2025
