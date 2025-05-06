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





