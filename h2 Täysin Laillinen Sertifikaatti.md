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

```
$POST /product/stock HTTP/1.0
$Content-Type: application/x-www-form-urlencoded
$Content-Length: 118
$stockApi=http://stock.weliketoshop.net:8080/product/stock/check%3FproductId%3D6%26storeId%3D1
```

-> Muokataan pyyntöä
```
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118
stockApi=http://localhost/admin
```
Mitä tutkia? 
- Osittaisia URLeja pyynnöissä (suhteelliset polut)
- URL - osoitteet dataformaatissa
- SSRF Refer - otsikon kautta

[Cross-site scripting](https://portswigger.net/web-security/cross-site-scripting)

- Mikä? Mahdollistaa käyttäjien vuorovaikutuksen haavoittuvan sovelluksen kanssa. Voi ohittaa "same origin policy" - käytännön, joka eristää ne yleensä toisistaan
- Tekeytyy uhriksi
- Manipuloi ja palauttaa haitallista JavaScriptia, joka vaarantaa sovelluksen kokonaan
- ```alert() ja print()``` - funktiot
- Esimerkki:
```
$https://insecure-website.com/status?message=All+is+well.
<p>Status: All is well.</p>
```
Sovellus ei tee mitään muuta prosessointia, joten siihen voi liittää pahoja juttuja

```
$https://insecure-website.com/status?message=<script>/*+Pahoja+juttuja+tänne...+*/</script>
<p>Status: <script>/* Pahoja juttuja tänne ...*/</script></p>
```
Stored XSS
- Data vastaanottaa epäluotettavasta lähteestä -> Sisällyttää myöhemiin HTTP - vastauksiin
- Viestisovellus antaa käyttäjien lähettää viestejä ja ne näkyy muille -> $<p>Hei, tämä on minun viestini!</p>

- Sitten lähetetään viesti, joka hyökkää muita käyttäjiä vastaan
```
$<p><script>/* Tähän jotain pahaa... */</script></p>
```
DOM XSS-> Javascriptiä epäluotettavasta lähteestö -> Data takaisin DOMiin -> Voi luoda haitallisen arvon, joka suorittaa scriptin

[Vapaaehtoinen: Server-side template injection](https://portswigger.net/web-security/server-side-template-injection)

Mikä? Hyökkääjä pystyy käyttämään mallipohjan omaa syntaksia haitallisen payloadin injektoimiseen, joka suoritetaan palvelimella.
- Käyttäjän syöre yhdistetään suoraan mallipohjaan eikä välitetä datana -> Hyökkääjät injektoi ohjeita, mikä antaa täyden hallinnan palvelimeen


## a) Totally Legit Sertificate. Asenna OWASP ZAP, generoi CA-sertifikaatti ja asenna se selaimeesi. Laita ZAP proxyksi selaimeesi. Laita ZAP sieppaamaan myös kuvat, niitä tarvitaan tämän kerran kotitehtävissä. Osoita, että hakupyynnöt ilmestyvät ZAP:n käyttöliittymään. (Ei toimine localhost:lla ilman Foxyproxya)

Yhdistän nämä kaksi tehtävää

## b) Kettumaista. Asenna "FoxyProxy Standard" Firefox Addon, ja lisää ZAP proxyksi siihen. Käytä FoxyProxyn "Patterns" -toimintoa, niin että vain valitsemasi weppisivut ohjataan Proxyyn. (Läksyssä ohjataan varmaankin PortSwigger Labs ja localhost.)


Asennellaan ensin se [ZAP](https://www.zaproxy.org/download/)
Ja sieltä Teron ohjeiden mukaan Cross Platform, koska se on paras. Itse asennan nyt Kalissa suoraa ```sudo apt-get install zaproxy```
Asentelu vei 35 sekuntia
Tämän jälkeen ```whereis zaproxy```

![image](https://github.com/user-attachments/assets/ffe04d4d-c25e-49c2-be8a-38cf280c953a)

Siitä ```cd /usr/bin``` ja ```./zaproxy```

Ajelin siihen kaikki nuo päivitykset, joita se tarjos

![image](https://github.com/user-attachments/assets/9f684c55-d971-433c-bf79-df148a4c7c41)


Laitellaan tässä samalla tuo FireFoxin FoxyProxy Addon - kuntoon. Eli päästään tuolta oikealta ylhäältä lisäilemään addoneja, haetaan foxyproxy

![image](https://github.com/user-attachments/assets/010fc62f-2ade-4bd2-8e43-66096dccf6df)

Valitaan se

![image](https://github.com/user-attachments/assets/689a446b-247f-4cba-ba6f-1caf8486ce2b)

![image](https://github.com/user-attachments/assets/44844868-965d-4409-9856-627d90165649)

Annetaan kivoja lupia

![image](https://github.com/user-attachments/assets/b0ef2f89-9c49-41e3-81fe-806eb512d033)

![image](https://github.com/user-attachments/assets/5223685a-0aea-4492-910c-6718d97d18bc)


Koska miksi ei

Ja nyt se löytyy tuolta, kun painaa "Extensions" 

![image](https://github.com/user-attachments/assets/fa81cef5-2995-4112-8f3e-782fc875fb83)


Takasin Zappiin ja sieltä Tools -> Options

![image](https://github.com/user-attachments/assets/85a24a3d-58c8-4b18-991a-15426f8d9fa8)


Voidaan manuaalisesti hakea tai sanoilla, mutta muistaakseni Certificates ei näy edes hakemalla, ennenkö on avannut "Network" - kohdan.
Seuraavaksi mennään Server Certificates

![image](https://github.com/user-attachments/assets/e8f89730-6d1c-4f3e-9e28-d9d97d6f873f)


![image](https://github.com/user-attachments/assets/c2fd5e7f-a194-42aa-be6a-ce153bc020be)


-> Generate

Nyt, kun Certi on tehty, mennään FireFoxiin settings -> Cerfiticates -> View Certificates

![image](https://github.com/user-attachments/assets/d221eec2-ad82-4dd2-add3-db9b188dbff7)

![image](https://github.com/user-attachments/assets/bb3b9273-d956-4d4b-beda-a1b864a46f89)

![image](https://github.com/user-attachments/assets/9a6cc706-b1c2-4ad3-9d08-c8ac322df683)


Ja tottakai luotetaan näin turvallisen oloiseen certiin.
![image](https://github.com/user-attachments/assets/10bd13e7-e9fe-4f70-b6c7-d6a5305ebe6e)

Nyt se näkyy listassa

Sitte lisätään se seuraavaks FoxyProxyyn -> Kävin samalla varmistamassa huvikseen, että localhost - osoite ei oo hypääny mihinkään taivaisiin

![image](https://github.com/user-attachments/assets/d6746285-180a-4d30-b47b-3aa3cf303fc9)

Zapin ohjeissa localhostin portti 8080, joten mennään sillä.

![image](https://github.com/user-attachments/assets/9b0de87b-f135-4c29-a1c4-fb16088e45cf)

Laitoin Proxy by Patterns ja siihen sen portswigger.net

![image](https://github.com/user-attachments/assets/34375f1c-bb2c-44ce-b370-1d98fdc30ccf)

Asetukset pitäis olla nyt kunnossa ja certit asennettu.



Ihmettelin hetken, kun mitään ei tapahdu ja sit unohdin et se varmaan liittyy Mozillan omiin nettiasetuksiin 
-> Settings -> General -> Ihan alas ja Network Settings

![image](https://github.com/user-attachments/assets/605e00bf-b282-4f52-97ea-ef543a788e0f)

Nyt tuli jopa valikko, jota ei aikaisemmin ollu:

![image](https://github.com/user-attachments/assets/f2fc8b04-d500-4ae2-a882-74dc66a10d09)

Nyt näyttää toimivan, sillä navigoimalla sivulle ```portswigger.net``` Zappi aktivoitui ja alkoi puuhastelemaan

Avasin FoxyProxyn, Zapproxyn ja portswiggerin nettisivut eikä mikään muuttunut noissa tuloksissa, joten uskon sen proxyn olevan ainoastaan päällä Portswiggerin sivuille. Varmistin tämän vielä surffailemalla lisää ties missä osoitteissa. Zapproxy ei reagoinut. Eteenpäin siis!

Ainiin! Samalla, kun ollaan Zapissa, käydään laittamassa ne kuvien sieppaus päälle, eli takaisin "Options" -> Display -> Process images in HTTP requests/responses

![image](https://github.com/user-attachments/assets/355913e3-eddf-4d1f-8b1b-ff4aaa4989d1)


### PortSwigger Labs. Ratkaise tehtävät. Selitä ratkaisusi: mitä palvelimella tapahtuu, mitä eri osat tekevät, miten hyökkäys löytyi, mistä vika johtuu. Kannattaa käyttää ZAPia, vaikka malliratkaisut käyttävät harjoitusten tekijän maksullista ohjelmaa. Monet tehtävät voi ratkaista myös pelkällä selaimella. Malliratkaisun kopioiminen ZAP:n tai selaimeen ei ole vastaus tehtävään, vaan ratkaisu ja haavoittuvuuden etsiminen on selitettävä ja perusteltava.

## Cross Site Scripting (XSS)

### c) https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded

- Kokeilin ensin lueskella vähän lisää siitä aiheesta ja ohjeessa luki "alert" - function, joten kokeilin käyttää sitä. Se ilmeisesti tykkää syötteistä, joten kokeillaan laittaa sinne vaikka HTML - syötettä

 ![image](https://github.com/user-attachments/assets/49bfa381-db00-4cfa-bb90-c4136271fb25)

Ja toimiihan se. 

Eli voinko lisätä vain <script>alert</script> siihen perään?
Kappas. En itse asiassa tajunnut, että sinne sisään pitää syöttää jotain joka sen tulostaa pihalle. Kävin siis googlettamassa nopeasti mikä tuo alert() on ja sinne sisään pitää tietysti laittaa jotain... 
Kokeilen siis ```<script>alert(123)</script>```

Ja se toimii!
![image](https://github.com/user-attachments/assets/ca5b5c92-c6df-4efc-9a45-bd88c30b28e1)

- Eli tässä ei siis ole rajoitettu lainkaan sitä, mitä käyttäjä saa syöttää kenttään, suorittaa sen heti ja se mahdollistaa hyökkäyksen

### d) [Stored XSS into HTML context with nothing encoded](https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded)

- Selailin ensin sivua läpi ja katsoin, mitä se sisältää. Hyvin saman tyylinen, kuin aikaisempi ilman suoraa kenttää syöttää mitään. 

- Jokaisen "View Post" jälkeen on kommenttikenttä, johon voi syöttää jotain ja se kiinnostaa

![image](https://github.com/user-attachments/assets/9bfa96fd-1fae-45c5-835f-5f7c5bd2dc14)

![image](https://github.com/user-attachments/assets/99ec8511-5149-485c-a1ab-63b0de788aff)

![image](https://github.com/user-attachments/assets/382928b9-3715-4642-8c49-8f77868f91db)

Kokeilen jälleen sitä <script>alert(123)</script> 

![image](https://github.com/user-attachments/assets/272c825a-c816-4c4b-b5a7-d962eff09e9d)

Eli kommenttikenttään syötetty scripti ajetaan käyttäjän selaimessa, kun hän menee sivulle, johon kommentti on syötetty. Palasin takasinpäin ja tuli kyseinen viesti: 

![image](https://github.com/user-attachments/assets/f4cb8a8b-0c10-4e60-a2b3-7ed88f7a2cbf)



- Tein ensin ihan tavallisen kommentin, jossa ei oikeastaan ollut mitään ihmeellistä. Se oli suht tarkkka s-postin ja websiten kanssa
- Tässäkin oli ainoastaan rajoitettu Website sekä E-mail - kohdat. Ei mitenkään kommenttikenttiin muita syötteitä.
- Kommenttikentässä se näkyy tyhjänä

![image](https://github.com/user-attachments/assets/e1243107-1202-457e-9d41-e39a0d70912c)

## Path traversal

### e) [File path traversal, simple case](https://portswigger.net/web-security/file-path-traversal/lab-simple) Laita tarvittaessa Zapissa kuvien sieppaus päälle

- Sivustolla ei näy mitään ihmeellistä. View details kiinnostaa

![image](https://github.com/user-attachments/assets/cdce5c7a-6ff0-4a80-8ed6-553d29998269)

- Osoiterivillä ei mitään mielenkiinnosta
- Tässä hetken aikaa tutkinu koko sivustoa ja päätin klikata kuvan auki

![image](https://github.com/user-attachments/assets/95befbb8-be6e-4752-9f83-0552230ba517)

- Aikasemmin, kun käytiin noita tiivistelmiä läpi, jäi mieleen se "filename" ja siihen syötteiden lisääminen
- Heitin suoraa syvään päätyyn ja laitoin filename=/etc/passwd

![image](https://github.com/user-attachments/assets/9b1598ca-6b2f-4608-800a-549be93d9efd)

Käsittääkseni ihan kiva ilmoitus. Se on siis mahdollista! 

Laitoin seuraavaksi .../.../.../etc/passwd
Sekään ei tuottanut tulosta - takaisin lukemaan
Tulikin vähän liikaa pisteitä, eli ../../../etc/passwd

![image](https://github.com/user-attachments/assets/6968883a-c1d0-4ae2-831d-0429ce5fe7b2)

Hetken aikaa kokeilin vaikka ja mitä, mutta palasin takasin sivustolle ja lukee "Lab solved"

![image](https://github.com/user-attachments/assets/8679197f-bcce-485b-8e5c-4055811ab936)

- Eli koska pystyttiin tekemään suoraa "hakuja" hyppimällä päämäärätietoisesti kohti haluttua hakemistoa, haavoittuvuus löytyi siitä, ettei siinä ollut mitään suojaa sitä vastaan.

### [f) File path traversal, traversal sequences blocked with absolute path bypass](https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass)

- Sama homma tässä, ei mitään ihmeellistä ensinalkuun. Mennään View details

  ![image](https://github.com/user-attachments/assets/2a0604b7-0c4b-49d1-911f-76c07535837e)

Täällä taas. Kokeilen samoja, kun aikaisemmin.

Ei toiminut itseasiassa mikään eikä myöskään ne, mitä portswiggerissä oli ohjeina

## Lähteet: 

- A01:2021 – Broken Access Control, OWASP Top 10:2021, Luettavissa: https://owasp.org/Top10/A01_2021-Broken_Access_Control/, Luettu: 08/04/2025

- A10:2021 – Server-Side Request Forgery (SSRF), OWASP Top 10:2021, Luettavissa: https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/, Luettu 08/04/2025

- Insecure direct object references (IDOR), PortSwigger, Luettavissa: https://portswigger.net/web-security/access-control/idor, Luettu: 08/04/2025

- Path traversal, PortSwigger, Luettavissa: https://portswigger.net/web-security/file-path-traversal, Luettu: 08/04/2025

- Server-side request forgery (SSRF), PortSwigger, Luettavissa: https://portswigger.net/web-security/ssrf, Luettu: 08/04/2025

- Cross-site scripting, PortSwigger, Luettavissa: https://portswigger.net/web-security/cross-site-scripting, Luettu: 08/04/2025

- Server-side template injection, PortSwigger, Luettavissa: https://portswigger.net/web-security/server-side-template-injection, Luettu 08/04/2025

- Configuring Proxies, ZAP by Checkmarx, Luettavissa: https://www.zaproxy.org/docs/desktop/start/proxies/, Luettu: 09/04/2025

- URL Patterns FoxyProxy, Luettavissa: https://help.getfoxyproxy.org/index.php/knowledge-base/url-patterns/, Luettu: 09/04/2025
