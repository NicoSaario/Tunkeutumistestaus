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

- Mitä? Kun sovellus käyttää käyttäjän antamaa syöttää suoraan kohteeseen (tietokantatietueet, tiedostot).
- Tässä tilanteessa nettisivu käyttää URLia, joka johtaa suoraan asiakkaan käyttäjäsivulle  ```https://insecure-website.com/customer_account?customer_number=132355```
  
    - Väärinkonfiguroituna mahdollistaa sen, että voidaan muokata pelkästään ```customer_number``` ja saada pääsy johonkin toiseen käyttäjään.

 - Toisessa esimerkissä nettisivu saattaa tallentaa chättiviestit levylle käyttäen kasvavia tiedostonimiä ja sallii käyttäjien hakea niitä. Pelkkä tiedostonimen muokkaaminen riittää. ```https://insecure-website.com/static/12144.txt```


[Path traversal](https://portswigger.net/web-security/file-path-traversal)

- Mikä? Itse kutsun "polkuhyppelyksi". Mahdollistaa hyökkääjän lukea mielivaltaisia tiedostoja palvelimelta, esim. Sovelluksen koodin ja datan, credientials tai arkaluontoisia käyttöjärjeselmätiedostoja
- Joskus voi jopa itse kirjoittaa, muokata ja ottaa koko hallinnan

Miten? ```<img src="/loadImage?filename=218.png">```

- LoadImage URL ottaa ```filename``` parametrin ja palauttaa sisällön siitä tiedostosta -> Ne on tallennettu /var/www/images ja sit se palauttaa hakemalla ton lopun, eli ```/var/www/images/218.png```
- Koska siinä ei oo mitään suojausta tätä vastaan, voidaan hyppiä ja hakea salasanakin sieltä ```https://insecure-website.com/loadImage?filename=../../../etc/passwd```, jollon se lukee ja palauttaa sen salasanan tuolta kansiopolusta
- ../ hyppii yhden ylöspäin, Winkkarilla toimii sekä ../ että ..\
- Kolme hyppyä rootti -> ../../../etc/passwd
- Paljon variaatioita: Absoluuttinen polku ```filename=/etc/passwd```, yhdistetty ```....//``` tai ```..../\```, Polunläpäisy-yritysten poisto syötteistä -> Burp automaatio tai URL - enkoodaus, Tietyn peruskansion vaatiminen, esim. ```/var/www/images```-> ```filename=/var/www/images/../../../etc/passwd```, tiedostopäätteen vaatiminen vaikka png -> %00 katkaisemaan tiedostopolku ```filename=../../../etc/passwd%00.png```
- 

Miten estää?
- Jos ei voi välttyä käyttäjäsyötteestä -> Kaksi kerrosta suojausta
- Validoi ennen käsittelyä -> Whitelist halutuista arvoista tai vain sallitut merkit


[Server-side request forgery (SSRF)](https://portswigger.net/web-security/ssrf)

- Mikä? Serveripuolen sovelluksen pyyntöjä ei haluttuun paikkaan. Hyökkääjä voi saada palvelimen muodostamaan yhteyden sisäisiin, sisäverkossa tooimiviin palveluihin tai pakottaa sen ottaa yhteyden ulkoisiin järjestelmiin -> Arkaluontoisten tietojen vuotaminen

- Esimerkki: Palvelimeen kohdistuvassa hyökkääjä saa sovelluksen tekemään HTTP - pyynnön Loopbackin kautta (127.0.0.1) ->

$POST /product/stock HTTP/1.0
$Content-Type: application/x-www-form-urlencoded
$Content-Length: 118
$stockApi=http://stock.weliketoshop.net:8080/product/stock/check%3FproductId%3D6%26storeId%3D1

-> Muokataan pyyntöä

$POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118
stockApi=http://localhost/admin

Mitä tutkia? 
- Osittaisia URLeja pyynnöissä (suhteelliset polut)
- URL - osoitteet dataformaatissa
- SSRF Refer - otsikon kautta

[Cross-site scripting](https://portswigger.net/web-security/cross-site-scripting)

- Mikä? Mahdollistaa käyttäjien vuorovaikutuksen haavoittuvan sovelluksen kanssa. Voi ohittaa "same origin policy" - käytännön, joka eristää ne yleensä toisistaan
- Tekeytyy uhriksi
- Manipuloi ja palauttaa haitallista JavaScriptia, joka vaarantaa sovelluksen kokonaan
- ```alert() ja print()``` - funktiot
- Esimerkki: $https://insecure-website.com/status?message=All+is+well.
<p>Status: All is well.</p>

Sovellus ei tee mitään muuta prosessointia, joten siihen voi liittää pahoja juttuja


$https://insecure-website.com/status?message=<script>/*+Pahoja+juttuja+tänne...+*/</script>
<p>Status: <script>/* Pahoja juttuja tänne ...*/</script></p>

Stored XSS
- Data vastaanottaa epäluotettavasta lähteestä -> Sisällyttää myöhemiin HTTP - vastauksiin
- Viestisovellus antaa käyttäjien lähettää viestejä ja ne näkyy muille -> $<p>Hei, tämä on minun viestini!</p>

- Sitten lähetetään viesti, joka hyökkää muita käyttäjiä vastaan
$<p><script>/* Tähän jotain pahaa... */</script></p>

DOM XSS-> Javascriptiä epäluotettavasta lähteestö -> Data takaisin DOMiin -> Voi luoda haitallisen arvon, joka suorittaa scriptin

[Vapaaehtoinen: Server-side template injection](https://portswigger.net/web-security/server-side-template-injection)

Mikä? Hyökkääjä pystyy käyttämään mallipohjan omaa syntaksia haitallisen payloadin injektoimiseen, joka suoritetaan palvelimella.
- Käyttäjän syöre yhdistetään suoraan mallipohjaan eikä välitetä datana -> Hyökkääjät injektoi ohjeita, mikä antaa täyden hallinnan palvelimeen


## a) Totally Legit Sertificate. Asenna OWASP ZAP, generoi CA-sertifikaatti ja asenna se selaimeesi. Laita ZAP proxyksi selaimeesi. Laita ZAP sieppaamaan myös kuvat, niitä tarvitaan tämän kerran kotitehtävissä. Osoita, että hakupyynnöt ilmestyvät ZAP:n käyttöliittymään. (Ei toimine localhost:lla ilman Foxyproxya)
















## Lähteet: 

- A01:2021 – Broken Access Control, OWASP Top 10:2021, Luettavissa: https://owasp.org/Top10/A01_2021-Broken_Access_Control/, Luettu: 08/04/2025

- A10:2021 – Server-Side Request Forgery (SSRF), OWASP Top 10:2021, Luettavissa: https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/, Luettu 08/04/2025

- Insecure direct object references (IDOR), PortSwigger, Luettavissa: https://portswigger.net/web-security/access-control/idor, Luettu: 08/04/2025

- Path traversal, PortSwigger, Luettavissa: https://portswigger.net/web-security/file-path-traversal, Luettu: 08/04/2025

- Server-side request forgery (SSRF), PortSwigger, Luettavissa: https://portswigger.net/web-security/ssrf, Luettu: 08/04/2025

- Cross-site scripting, PortSwigger, Luettavissa: https://portswigger.net/web-security/cross-site-scripting, Luettu: 08/04/2025

- Server-side template injection, PortSwigger, Luettavissa: https://portswigger.net/web-security/server-side-template-injection, Luettu 08/04/2025
