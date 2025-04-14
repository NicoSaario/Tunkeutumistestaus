# Kotitehtävät

Kotitehtävät ovat kurssilta "Tunkeutumistestaus -Penetration Testing course - 2024 late " ja löytyvät osoitteesta https://terokarvinen.com/tunkeutumistestaus/

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 14/04/2025 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+3) 14/04/2025 (pvm/kk/v)

Kaikissa testaukseen liittyvässä: Oracle VM VirtualBox ja Kali Linux Point release live image (2025.1a)



# h3 Fuzzy

Opit käyttämään maailman johtavaa weppifuzzeria: ffuf. Sen opettaa meille tunnilla itse työkalun tekijä, joohoi.

## x) Lue/katso/kuuntele ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva. Lisää mukaan jokin oma idea, huomio, kysymys tai kommentti.)

Karvinen 2023: [Find Hidden Web Directories - Fuzz URLs with ffuf](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/)

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


Jompi kumpi, Hoikkalan video tai teksti:
           
-  Hoikkala 2023: [ffuf README.md](https://github.com/ffuf/ffuf/blob/master/README.md), tai
           
- Hoikkala "joohoi" 2020: [Still Fuzzing Faster (U fool). In HelSec Virtual meetup #1.](https://www.youtube.com/watch?v=mbmsT3AhwWU) (Noin tunnin mittainen)
