# Kotitehtävät

Kotitehtävät ovat kurssilta "Tunkeutumistestaus -Penetration Testing course - 2024 late " ja löytyvät osoitteesta https://terokarvinen.com/tunkeutumistestaus/

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 14/04/2025 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+3) 14/04/2025 (pvm/kk/v)

Kaikissa testaukseen liittyvässä: Oracle VM VirtualBox ja Kali Linux Point release live image (2025.1a)



# h3 Fuzzy

Opit käyttämään maailman johtavaa weppifuzzeria: ffuf. Sen opettaa meille tunnilla itse työkalun tekijä, joohoi.

## x) Lue/katso/kuuntele ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva. Lisää mukaan jokin oma idea, huomio, kysymys tai kommentti.)

### Karvinen 2023: [Find Hidden Web Directories - Fuzz URLs with ffuf](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/)

- Äärimmäisen kätevä webbifuzzeri - kehittänyt joohoi, jolla löytyy piilotettuja hakemistoja ja käytännössä mitä vain POSTeista parametreihin.

- Löytyy nykyään uusimmista package managereista

```
sudo apt-get update
sudo apt-get install fuff
```

Voi myös hakea [täältä](https://github.com/ffuf/ffuf)

Sanalistoja [täältä](https://github.com/danielmiessler/SecLists) jos ei halua itse tehdä

- Esimerkiksi common.txt kokeilemisen saa näin

 ```
  ./fuff -w common.txt -u http://127.0.0.3:8000/FUZZ
 ```

Se siis käytännössä vaihtaa joka rivin tuolta common(.)txt riviltä FUZZ - tilalla

```
wget https://raw.githubusercontent.com/danielmiessler/SecLists/master/Discovery/Web-Content/common.txt
```

- Ei tosiaan kannata netissä pitää, jos ei tiedä, mitä tekee. Fuffilla voi tehdä pahaa jälkeä, jos ei käytä esim. rate - limittiä ja rajoita pyyntöjä.
- Filtteröintiin: Status (-fc), Size in bytes (-fs), Words (-fw), Lines (-fl), Duration in ms. (-ft), eli hyvin simppelit ja helppo miettiä. Manuaalista voi tarkistaa, jos tarvitsee.
- Sivulta saa myös äärimmäisen hyvän targetin, johon voi harjoitella paikallisesti


### Jompi kumpi, Hoikkalan video tai teksti:
           
Hoikkala 2023: [ffuf README.md](https://github.com/ffuf/ffuf/blob/master/README.md), tai

 - FUZZ voi käyttää missä tahansa kohdassa, URL (-u), headers (-H) tai POST data (-d)
 - Interaktiivinen moodi, eli pauselle ja voi tehdä muutoksia [ENTER]
 - Jotta ffuf ei aja itseään loputtomiin, voi käyttää ```-maxtime```, joka lopettaa prosessin halutussa ajassa (s)
 - Pidän edelleen siitä rakenteesta, että komennot pystyy suurilta osin jopa päättelemään.
           
Hoikkala "joohoi" 2020: [Still Fuzzing Faster (U fool). In HelSec Virtual meetup #1.](https://www.youtube.com/watch?v=mbmsT3AhwWU) (Noin tunnin mittainen)

- Äärimmäisen mielenkiintoista seurattavaa
- Hyvin selitetty toiminnot yksityiskohtaisemmin



## a) Fuzzzz. Ratkaise dirfuz-1 artikkelista Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf.

Ensin tietysti

```
sudo apt-get update
sudo apt-get install ffuf
```

- Kali on kyllä siitä kätevä, että suurin osa on jo valmiiksi asennettuna

![image](https://github.com/user-attachments/assets/1c93e5b5-0433-4cdf-8050-ba9ab98ecb4c)

Eli lähdetään hakemalla tiedosto [täältä](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/) tai vaihtoehtoisesti aikaisemman ohjeen mukaan:

```
$ wget https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/dirfuzt-1
$ chmod u+x dirfuzt-1
$ ./dirfuzt-1
```

Sitten käydään hakemassa SecLististä sanakirja

```
$ wget https://raw.githubusercontent.com/danielmiessler/SecLists/master/Discovery/Web-Content/common.txt
```

![image](https://github.com/user-attachments/assets/23484949-b5ba-40ad-9dba-c4aa798dcbc1)

Sille tui annettua noi oikeudet ja ajettua käyntiin

Selaimella navigoimalla tohon ```127.0.0.2:8000``` tulee tehtävä näkyviin

![image](https://github.com/user-attachments/assets/3e555159-6f62-49f2-9586-26316d71ec74)

Eli maali on päällä

Napataan netti pois päältä

![image](https://github.com/user-attachments/assets/a1926e18-8eac-467c-bc02-539b521c1ea6)

Sitten kokeillaan tota esimerkkiä, eli sanalista ja targetin url

```
./ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ
```

Mutta! Koska ffuf olettaa, että tiedosto on samassa kansiossa, tehdään sille polku -> ```./ffuf -w /home/nico/Downloads/common.txt -u http://127.0.0.2:8000/FUZZ```

Siinä lopputulema

![image](https://github.com/user-attachments/assets/83123ce7-6931-49c4-bd97-6cf08aa8e923)


![image](https://github.com/user-attachments/assets/a07d3a28-a2fe-4ca9-a0c8-a786542667f2)

Tutkailin sitä hetken lisäämällä | & less - komennon loppuun, jonka jälkeen on helpompi tarkastella.
Suurin osa oli status 200, Size 154, Words 9, Lines 10, mutta kaksi erittottui joukosta

```
Status 301, Size 41, Words 3, Lines 3
and
Status 200, Size 178, Words 6, Lines 11
```

Päätin kokeilla ensimmäistä ja laitoin

```
./ffuf -w /home/nico/Downloads/common.txt -u http://127.0.0.2:8000/FUZZ -fc 301 -fw 9 -fl 10
```

Olin nähtävästi laittanut vahingossa -fw 9, vaikka sen piti olla 3 ja -fl 10, vaikka sen piti olla 3. No vähän ajatus meni ristiin, mutta eipä sillä tässä kohtaa niin suurta merkitystä. 

Tuosta näkee vielä nuo filtterit

![image](https://github.com/user-attachments/assets/cad8458e-5441-4ed7-83c7-61d1a726418f)

Ja lopputulos oli tämä:

![image](https://github.com/user-attachments/assets/65986717-8705-450a-8f93-e08700250ae5)

Sieltä löytyi siis wp-admin

Muokkaamalla luvut Words 6, Lines 11, Status 200  ```./ffuf -w /home/nico/Downloads/common.txt -u http://127.0.0.2:8000/FUZZ -fc 200 -fw 6 -fl 11```

Saadaan tämän näköinen lopputulos:

![image](https://github.com/user-attachments/assets/059a8e76-cb43-4fc3-b864-68e931ed8182)

### Tavallaan ihan hauska, että pääsin tälläkin tyylillä vahingossa maaliin, mutta jäin ihmettelemään tota lopputulosta ja tajusin samalla, että se filtteri suodattaa pois halutut kohdat eikä toisinpäin

- Eli olisin vaan voinu tehdä sen suodattamalla pois ne, joita en halua. No tulipahan harjoteltua
- Eli siis  ```./ffuf -w /home/nico/Downloads/common.txt -u http://127.0.0.2:8000/FUZZ -fs 154```, joka suodattaa pois ne 154 - koon rivit, joita ei haluta nähdä.

![image](https://github.com/user-attachments/assets/792781bb-71cf-4933-bec8-df6d831fb301)

Ja tuosta, kun syöttää ```/wp-admin``` osoiteriville, jotain pitäisi tapahtua:


![image](https://github.com/user-attachments/assets/3c2be0be-5a46-448b-9e74-ab5ef777f8b2)

Lippu löytyi: FLAG{tero-wpadmin-3364c855a2ac87341fc7bcbda955b580}


## b) Fuff me. Asenna FuffMe-harjoitusmaali. Karvinen 2023: [Fuffme - Install Web Fuzzing Target on Debian](https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/)

Mennään täysin tässä Teron ohjeiden mukaan noilta sivuilta, mutta ennen sitä pitää lopettaa fuffailut ja laittaa netti takasin päälle

![image](https://github.com/user-attachments/assets/6d9600c1-6ebb-411a-880e-7643b621e6a8)

Jälleen ollaan maailmalla. Meiltä löytyy siis jo ffuf, joten asennellaan docker.io ja git


```
sudo apt-get install docker.io git
```

Sitten rakennetaan harjoitusmaali Dockerkontille

```
git clone https://github.com/adamtlangley/ffufme
cd ffufme/
sudo docker build -t ffufme .
```

Ajetaan se

```
sudo docker run -d -p 80:80 ffufme
$ curl localhost
```

Testataan, että toimii

```
curl -si localhost|grep title
<title>FFUF.me</title>
```

![image](https://github.com/user-attachments/assets/b6d5c2fe-67e2-4d66-968e-492d2e00e4bb)

Tähän virheilmoitukseen olikin Terolla jo vastaus, eli Apache2 pois päältä.
Käytimme sitä toisessa kotitehtävässä, joten se siellä varmasti kummitteli


![image](https://github.com/user-attachments/assets/be60c24f-c4a3-4a7c-9a1e-b3ba42b9d721)

Ajetaan uudelleen ja tehdään ```curl localhost```

![image](https://github.com/user-attachments/assets/64ebcbd4-d0d6-4e62-a073-8ad68eeaeb33)

![image](https://github.com/user-attachments/assets/c31faf57-4034-4bff-ac65-2dcdc9532a12)


Näyttää siis toimivan

Tältä näyttää vielä selaimesta:


![image](https://github.com/user-attachments/assets/f0bcf4a9-a7c2-49c1-b5e2-06f02624173a)


Asennetaan sanalistat

```
mkdir $HOME/wordlists
cd $HOME/wordlists
wget http://ffuf.me/wordlist/common.txt
wget http://ffuf.me/wordlist/parameters.txt 
wget http://ffuf.me/wordlist/subdomains.txt
cd -
```

Ja tän jälkeen netti taas pois päältä


![image](https://github.com/user-attachments/assets/dc191cb3-13c3-4969-9efa-fd95b6709755)

Täältä löytyy se oikea komentopolku, jota suoritetaan


![image](https://github.com/user-attachments/assets/590f1152-2746-43db-ae8f-91e1fcf17d0e)


![image](https://github.com/user-attachments/assets/92908a05-aadf-48eb-8248-4a961b0a129d)

Eli: ```ffuf -w ~/wordlists/common.txt -u http://localhost/cd/basic/FUZZ```



![image](https://github.com/user-attachments/assets/8b5d0599-900c-4f28-a976-e514c62d936d)

Ja sieltä löytyi class ja development.log. Homma toimii! 


## Ratkaise ffufme harjoitukset - kaikki paitsi ei "Content Discovery - Pipes".

### c) Basic Content Discovery

Tämän tein jo aikaisemmassa tehtävässä, kun testasin sen toimintaa

### d) Content Discovery With Recursion


```ffuf -w ~/wordlists/common.txt -recursion -u http://localhost/cd/recursion/FUZZ```


![image](https://github.com/user-attachments/assets/70f45ce6-959d-4f9c-8bac-a47a93fe63f6)

Tässä siis kerrotaan -r avulla ffuffille, että jos se löytää hakemiston, se aloittaa sen hakemiston sisällön skannaamisen, joka jatkuu niin pitkään, että jokainen hakemisto on skannattu ja kaikki löydetty.

Ajaa sen sitten tuon /wordlist/common.txt kautta tai oman tallennetun SecListin, lopputulos on sama. Hetken kyllä mietin, onko tässä joku kompa:

![image](https://github.com/user-attachments/assets/5ecf4b04-e1ab-443c-af8c-1326bbd96efb)

Eli polun pitäisi olla siis ```http://localhost/cd/recursion/admin/users/96```

![image](https://github.com/user-attachments/assets/d82aaa49-7d40-42a6-817c-b7d1075cb286)




### e) Content Discovery With File Extensions

Tässä seuraavassa on /logs - hakemisto löydetty, mutta sen sisältöä ei nähdä. Oletettavasti puhutaan kuitenkin .log - päätteitä.
-e spesifioi sen tyypin, joka laitetaan jokaisen sanan perään vaikkapa kotitehtavat.log ja hakee ne

![image](https://github.com/user-attachments/assets/a6ec1a7e-c3a5-4017-ad8f-16dfce96969b)

```ffuf -w ~/wordlists/common.txt -e .log -u http://localhost/cd/ext/logs/FUZZ```

Sieltähän se users.log löytyi

![image](https://github.com/user-attachments/assets/76dfe6c4-bb24-43c1-9a6d-06fc37dc245c)

![image](https://github.com/user-attachments/assets/c8e5c082-3af2-4412-8804-abf3200a1ab0)



### f) No 404 Status

Joskus on hyvä etsiä ei niin onnekkaita tuloksia, eli virheilmoituksia. Tässä tuttu 404 paljastaa salaisuuksia

-fs filtteröi siis ulos kaikki tulokset, jotka ovat määritellyn pituisia

![image](https://github.com/user-attachments/assets/ae3b416d-e9df-487f-aae8-5819457a84f6)

Ajetaan ```ffuf -w ~/wordliste/common tyt -ll httn.//localhosticd/na404/FUZZ```


![image](https://github.com/user-attachments/assets/b3107e68-6f02-49ba-a33b-7db052d28e66)

Palauttaa liudan 669 - tavun mittaisia tiedostoja.

Ajetaan siis -fs komennon kanssa -> ```ffuf -w ~/wordlists/common.txt -u http://localhost/cd/no404/FUZZ -fs 669```

Löydetään secret

![image](https://github.com/user-attachments/assets/0811b920-caea-4010-bffc-68566807c1df)

```https://localhost/cd/no404/secret```

Ja se palauttaa "Controller does not exist"

![image](https://github.com/user-attachments/assets/be19dc30-f842-435d-a4e6-9af36c4f8651)

Mietin taas, oliko tässä se kuuluisa kompa taustalla, mutta onhan tuo periaatteessa jo salaisuus.



### g) Param Mining

Tässä haetaan parametrejä, koska 400 status koodilla tuli Bad Request ja "Required Parameter Missing". Eli käytännössä wordlistiä jälleen hyödyntämällä päästään perille

![image](https://github.com/user-attachments/assets/ab5705f1-be49-4e8a-bdda-b559583b40dd)


Ajetaan siis ```ffuf -w ~/wordlists/parameters.txt -u http://localhost/cd/param/data?FUZZ=1```

![image](https://github.com/user-attachments/assets/dba3753e-553f-437c-879c-d733bc3064f8)

debug sieltä löytyi!

Kokeillaan jälleen, eli jos FUZZ=1 oli aikaisemmin, vaihdetaan se debug=1:

![image](https://github.com/user-attachments/assets/bf729c54-9e8b-42ee-bbaa-27324bb5a076)

Parametri löytyi siis!


### h) Rate Limited

Tässä harjoituksessa rajoitetaan pyyntöjä, joka jo normaalistikin on järkevää tietyissä tilanteissa.
Monet palvelut eivät anna tehdä montaa pyyntöä kerrallaan ja saattaa tulla vastaan 429 HTTP - ilmoitus.

```
-mc
```
Näyttää vain annetut http statukset

![image](https://github.com/user-attachments/assets/39c13506-d307-426a-97cd-69e41c58db32)

Ajetaan: ```ffuf -w ~/wordlists/common.txt -u http://ffuf.test/cd/rate/FUZZ -mc 200,429```

![image](https://github.com/user-attachments/assets/610a5cc8-fe2e-4cb8-a2eb-f85d3a36f04b)


Lisätään tuo -p arvo, joka pausettaa 0.1 sekunniksi joka pyynnön jälkeen ja -t, joka tekee 5 eri versiota fuffista. Eli tekee yhteensä 50 pyyntöä sekunnissa. Se on pidempi prosessi, mutta usein kannattava. Tämä kesti normaalilla haulla sen 5 - sekuntia, nyt noin 2 - minuuttia.

Ajetaan ```ffuf -w ~/wordlists/common.txt -t 5 -p 0.1 -u http://ffuf.test/cd/rate/FUZZ -mc 200,429```

Pariin kertaan ajelin tuota ja ihmettelin, kun mitään ei tapahtu ja ilmoitukset on pelkkää erroria. Ajelin kolmatta kertaa ja huomasin, ettei osoite johda oikeaan paikkaan.

Sen pitäisi olla siis ```ffuf -w ~/wordlists/common.txt -t 5 -p 0.1 -u http://localhost/cd/rate/FUZZ -mc 200,429```
Ylemmässäkin komennossa pitää siis muuttaa tuo ffuf.test localhostiksi


![image](https://github.com/user-attachments/assets/370e339d-221e-4b28-851c-8e43e516fa47)


Ja vihreetä valoo! 

![image](https://github.com/user-attachments/assets/0a0f0917-88a6-4c0c-b6e1-8f5ef0ff833f)

![image](https://github.com/user-attachments/assets/990c796f-2029-4d35-a927-7ccb1197b5c4)



### i) Subdomains - Virtual Host Enumeration


Tässä etsitään subdomaineja käyttämällä virtuaalihostia ja vaihtamalla host headeria.

Ajetaan: ```ffuf -w ~/wordlists/subdomains.txt -H "Host: FUZZ.ffuf.me" -u http://localhost```


Jälleen palautuu tietty kuvio; 1495 tavua iso tiedosto. 

![image](https://github.com/user-attachments/assets/9bc4f822-e16f-4f71-be5a-09bf8a72e376)



Käytetään -fs, jotta saadaan ne ulos listalta

Ajetaan: ```ffuf -w ~/wordlists/subdomains.txt -H "Host: FUZZ.ffuf.me" -u http://localhost -fs 1495```

Sieltähän paljastui redhat

![image](https://github.com/user-attachments/assets/92749b89-4207-4e7b-999c-53fb942dfaf8)

Luulisin siis, että pystyisin käyttämään esimerkiksi nyt redhat.ffuf.me subdomainia jos verkko olisi päällä. Tässä harjoituksessa sillä ei siis tee mitään.



Lähteet:

Tunkeutumistestaus, Tero Karvinen, Luettavissa: https://terokarvinen.com/tunkeutumistestaus/, Luettu 14/04/2025

Find Hidden Web Directories - Fuzz URLs with ffuf, Karvinen 2023, Luettavissa: https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/, Luettu 14/04/2025

Fuffme - Install Web Fuzzing Target on Debian, Karvinen 2023, Luettavissa: https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/, Luettu 14/04/2025

ffuf - Fuzz Faster U Fool, Hoikkala "joohoi" 2023, Luettavissa: https://github.com/ffuf/ffuf/blob/master/README.md, Luettu 14/04/2025

0x03 Still Fuzzing Faster (U Fool) - joohoi - HelSec Virtual meetup #1, HelSec 2021, Katsottavissa: https://www.youtube.com/watch?v=mbmsT3AhwWU, Katsottu 14/04/2025


SecLists, danielmiessler, https://github.com/danielmiessler/SecLists







