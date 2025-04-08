# Kotitehtävät

Kotitehtävät ovat kurssilta "Verkkoon Tunkeutuminen ja Tiedustelu - Network Attacks and Reconnaissance" ja löytyvät osoitteesta https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/#verkkojen-perusteet 

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 08/04/2025 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+3) 08/04/2025 (pvm/kk/v)

Kaikissa testaukseen liittyvässä: Oracle VM VirtualBox ja Kali Linux Point release live image (2025.1a)


# h2 Täysin Laillinen Sertifikaatti

## x) Lue/katso ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva kustakin artikkelista. Kannattaa lisätä myös jokin oma ajatus, idea, huomio tai kysymys.)
OWASP 2021: OWASP Top 10:2021

[A01:2021 – Broken Access Control (IDOR ja path traversal ovat osa tätä)](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)

- Pääsynvalvonta mahdollistaa sen, ettei käyttäjät voi toimia lupiensa ulkopuolella
- Pahat seuraamukset -> Arkaluontoisen tiedon leviäminen, muokkaaminen, tuhoaminen
- Haavoittuvuudet mm.
    - Vähimmäinen oikeus, oletusarvoinen esto rikottu -> Yleinen pääsy rajoitetun sijaan
    - Ohittaminen URLin muokkaamisella
    - Toisen käyttäjän näkeminen tai muokkaus
    - API - rajapintaan pääsy ilman pääsynvalvontaa (POST, PUT, DELETE)
    - Metadatan manipulointi (eväste, kenttä, pääsynvalvontatunniste JSON Web Token)
    - CORS konfiguroitu väärin -> API - pääsy luvattomista lähteistä
    - Pakotettu selaus tunnistautumista vaativiin sivuihin tunnistautumattomana käyttäjänä tai vaikka admin - sivut tavan käyttäjällä

Miten estetään?
  - Varmistetaan, ettei hyökkääjä voi muokata pääsynvalvontatarkistusta tai metadataa:
      - Ei julkiset resurssit ->> ESTO
      - Mekanismit kerran -> Uudelleen koko sovelluksessa
      - Valvominen sen sijaan, että annetaan käyttäjän lukea, luoda, päivittää tai poistaa tietueita
      - Mihin sovellusten tulisi päästä?
      - Verkkopalvelimien hakemistolistaus pois käytöstä -> Metadata, esim. .git ei juurihakemistossa
      - Epäonnistumiset pääsynvalvontan -> Lokit ja hälytykset!!!
      - API - ohjainpääsyn rajoittaminen
      - Tilalliset = Uloskirjautuminen -> Mitätöinti
      - Tilattomat JWT - tokenit -> Lyhyet!
      - Pitkäikäiset JWT - tokenit -> OAuth - standardeja
   
  Hyökkäykset melko simppelit, mutta tehokkaat:
    - Esim. 1: SQL ``` pstmt.setString(1, request.getParameter("acct"));
 ResultSet results = pstmt.executeQuery( );```
    - Jos löytää käyttäjät, voi päästä minne tahansa käyttäjälle: URL/account: ```https://example.com/app/accountInfo?acct=notmyacct```
    - Esim. 2: URL:  ```https://example.com/app/getappInfo
 https://example.com/app/admin_getappInfo``` Jos pääsee edes käymään tavallisella käyttäjällä, on kyseessä haavoittuvuus



[A10:2021 – Server-Side Request Forgery (SSRF)](https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/)

- Ilmenee, kun verkkosovellus hakee resursseja etänä ilman käyttäjän syöttämän URL tarkistusta/validointia. Mahdollistaa hyökkäjän pakottaa muokattu pyyntö kohteeseen palomuurin, VPN:n tai muun suojaamana.
- Yleinen käyttäjäystävällisyyden takia
- Pilvipalvelut ja arkkitehtuuri -> Vakavuus kasvussa

Miten ehkäistä? 
    - Verkkotasolla:
        - Etäresurssien segmentointi erillisiin verkkoihin
        - Käyttöön oletusarvoinen esto -> Vain välttämätön Intranet - liikenne
        - Kaikki hyväksytyt ja estetyt verkkoliikennevirrat palomuurille -> Kirjaa
    - Sovellustasolla:
        - Puhdista, validoi asiakkaan syöttämä data
        - Valvo URL, portit ja kohde sallittujen listalla
        - Ei raw vastauksia asiakkaalle
        - URL - osoitteiden johdonmukaisuus
        - HTTP - uudelleenohjaus -> Pois
        - Deny list tai säännölliset lausekkeet ei auta
    - Verkon salaus!
    - Paikallinen liikenne localhostina

Hyökkäysesimerkit:
    - 1. Porttiskannaus sisäisille servereille
        - Segmentoimaton arkkitehtuuri -> Sisäiset verkot -> Portit auki vai kiinni? = Liikaa tietoa
    - 2. Arkaluontoisen tiedon vaarantuminen
        - Hyökkääjät pääsee paikallisiin tiedostoihin tai sisäisiin palveluihin, vaikka ```file:///etc/passwd?``` ```http://localhost:28017/```
    - 3. Metadataan tai pilvipalveluihin pääsy
        - Useimmilla pilvipalveluntarjoajilla on metadata "varasto", esimerkiksi osoitteessa ```http://169.254.169.254/```
    - 4. Sisäisten palveluiden vaarantuminen
        - Voi käyttää niitä hyväkseen ja tehdä enemmän vahinkoa (Remote Code Execution), DOS
    


## PortSwigget Academy:
[Insecure direct object references (IDOR)](https://portswigger.net/web-security/access-control/idor)


[Path traversal](https://portswigger.net/web-security/file-path-traversal)


[Server-side request forgery (SSRF)](https://portswigger.net/web-security/ssrf)


[Cross-site scripting](https://portswigger.net/web-security/cross-site-scripting)


[Vapaaehtoinen: Server-side template injection](https://portswigger.net/web-security/server-side-template-injection)


















## Lähteet: 

- A01:2021 – Broken Access Control, OWASP Top 10:2021, Luettavissa: https://owasp.org/Top10/A01_2021-Broken_Access_Control/, Luettu: 08/04/2025

- A10:2021 – Server-Side Request Forgery (SSRF), OWASP Top 10:2021, Luettavissa: https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/, Luettu 08/04/2025

- Insecure direct object references (IDOR), PortSwigger, Luettavissa: https://portswigger.net/web-security/access-control/idor, Luettu: 08/04/2025

- Path traversal, PortSwigger, Luettavissa: https://portswigger.net/web-security/file-path-traversal, Luettu: 08/04/2025

- Server-side request forgery (SSRF), PortSwigger, Luettavissa: https://portswigger.net/web-security/ssrf, Luettu: 08/04/2025

- Cross-site scripting, PortSwigger, Luettavissa: https://portswigger.net/web-security/cross-site-scripting, Luettu: 08/04/2025

- Server-side template injection, PortSwigger, Luettavissa: https://portswigger.net/web-security/server-side-template-injection, Luettu 08/04/2025
