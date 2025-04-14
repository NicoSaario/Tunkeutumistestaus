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
           
-  Hoikkala 2023: [ffuf README.md](https://github.com/ffuf/ffuf/blob/master/README.md), tai
           
- Hoikkala "joohoi" 2020: [Still Fuzzing Faster (U fool). In HelSec Virtual meetup #1.](https://www.youtube.com/watch?v=mbmsT3AhwWU) (Noin tunnin mittainen)



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

### Tavallaan ihan hauska, että pääsin tälläkin tyylillä vahingossa maaliin, mutta jäin ihmettelemään tota lopputulosta ja tajusin samalla, että se filtteri suodattaa pois halutut kohdat

- Eli olisin vaan voinu tehdä sen suodattamalla pois ne, joita en halua. No tulipahan harjoteltua
- Eli siis  ```./ffuf -w /home/nico/Downloads/common.txt -u http://127.0.0.2:8000/FUZZ -fs 154```, joka suodattaa pois ne 154 - koon rivit, joita ei haluta nähdä.

![image](https://github.com/user-attachments/assets/792781bb-71cf-4933-bec8-df6d831fb301)





