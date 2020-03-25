# Skripta za konfigurisanje Ubuntu servera
Skripta za konfigurisanje servera

# Instalacija

```
git clone https://github.com/sitemapxml/lamp.git
cd lamp
chmod +x lamp
./lamp
```
Pre instalacije treba se uveriti da dns serveri pokazuju na odgovarajuću IP adresu. Jedan od načina je korišćenjem host komande:
```
host example.com
```
Što će kao rezultat vratiti:
```
example.com has address 93.184.216.34
```
Ukoliko IP adresa nije prikazana na ekranu ili dobijete odgovor kao u primeru ispod:
```
Host example.com not found: 3(NXDOMAIN)
```
To znači da DNS propagacija još uvek nije izvršena i da je potrebno da sačekate dok se u gore navedenoj proveri ne pojavi IP adresa. Konfigurisanje servera je moguće i samo sa IP adresom ali u tom slučaju nije moguća instalacija SSL sertifikata.

Licenca: [MIT](LICENSE)
