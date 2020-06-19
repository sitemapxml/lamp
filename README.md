# Skripta za konfigurisanje Ubuntu servera
Skripta za konfigurisanje servera

# Instalacija

```
git clone https://github.com/sitemapxml/lamp.git
cd lamp
chmod +x lamp
./lamp
```
Ukoliko želite da sačuvate izlaz sa ekrana u fajl to možete ostvariti jednostavnim korišćenjem `tee` komande:
```
./lamp | tee log.txt
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

# Nakon instalacije

Nakon instalacije potrebno je nadograditi zastarele programe i restartovati server.

```
apt update && apt upgrade && reboot
```
Ukoliko se pojavi kutija za dijalog kao u primeru ispod:

```
A new version of /boot/grub/menu.lst is available, but the version installed currently has been locally modified.

What would you like to do about menu.lst?
```
Odaberite opciju `keep the local version currently installed` i odaberite `<Ok>` ili pritisnite `[Enter]`

# Ulazni parametri
Na početku instalacije postoji šest upita na koje morate da odgovorite kako bi skripta bila pohranjena potrebmin informacijama i kako bi konfigurisanje moglo automatski da se izvrši.

 - **Naziv domena (hostname)** - koristi se ekstenzivno pri instalaciji. Prema unetom nazivu hosta kreira se `web root` i `apache virtual host` fajl. Ukoliko odaberete instalaciju Wordpress-a naziv baze podataka će biti dodeljen prema nazivu hosta sa donjom crtom umesto tačke. (npr. `example_com`)
 - **Lozinka root korisnika** - root lozinka servera
 - **Korisničko ime UNIX korisnika** - ovom korisniku će biti data prava root-a
 - **Lozinka UNIX korisnika**
 - **MYSQL root lozinka**
 - **Email adresa administratora** - koristi se pri instalaciji SSL sertifikata


Za slučaj da je javi potreba za prilagođavanjem skripte, u tabeli ispod data je lista ulaznih parametara sa njihovim shell varijablama:

| FUNKCIJA                      | VARIJABLA  |
|:------------------------------|:-----------|
| Naziv domena                  | hostname   |
| Lozinka root korisnika        | rootpass   |
| Korisničko ime UNIX korisnika | unixuser   |
| Lozinka UNIX korisnika        | unixpass   |
| MYSQL root lozinka            | mysqlrpass |
| Email adresa administratora   | email      |

Podrazumevana vrednost svih promenljivih (ne računajući promenljivu `email`) je `default`
Podrazumevana vrednost promenljive `email` je `webmaster@example.com`

# Struktura projekta

```
lamp/
├── files/
│   ├── 6g.conf
│   ├── index.html
│   ├── jcameron-key.asc
│   └── vhost.conf
├── lamp
├── mksite
├── uninstall
├── LICENSE
└── README.md
```

# Čuvanje podataka
Pri konfigurisanju servera skripta će napraviti direktorijum `.podaci` u `root` direktorijumu servera (ukoliko tako odaberete u poslednjem koraku).

Unutar foldera `.podaci` biće napravljeni sledeći fajlovi:
- podaci.txt
- ssl-info.txt

Fajl `podaci.txt` sadrži sve podatke koje ste uneli na početku instalacije.

Fajl `ssl-info.txt` sadrži podatke o instaliranom SSL sertifikatu (naziv i putanje na serveru)

Licenca: [MIT](LICENSE)
