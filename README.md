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

Ukoliko ste odabrali instalaciju Wordpress-a, nakon uspešnog konfigurisanja servera potrebno je dovršiti instalaciju otvaranjem odgovarajuće adrese u vašem pretraživaču i unošenjem podataka za povezivanje sa bazom podataka sačuvanih u fajlu `db-info.txt`

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
│   │ └── resources/
│   │      ├── 6g.conf
│   │      ├── index.html
│   │      ├── jcameron-key.asc
│   │      └── vhost.conf
│   ├── mksite
│   └── uninstall
├── lamp
├── LICENSE
└── README.md
```
Repozitorija se sastoji od tri skripte za konfigurisanje i foldera `files` unutar koga se nalaze fajlovi koji se po potrebi kopiraju u toku instalacije.

Namena skripti je sledeća:
- `lamp` služi za kompletno konfigurisanje servera i instalaciju svih neophodnih programa za rad LAMP steka.
- `mksite` služi za automatizovano dodavanje novog veb sajta, što obuhvata kreiranje virtual-hostova, instalaciju SSL sertifikata, dodavanje novog korisnika, kreiranje novog webroot-a i po potrebi, instalaciju Wordpress-a.
- `uninstall` služi za de-instalaciju svih instaliranih komponenti. Ova skripta se koristi samo u svrhe razvoja u slučajevima kada se javlja potreba za ponovnim konfigurisanjem servera.

Namena fajlova unutar foldera `files` je sledeća:
- `6g.conf` jeste niz pravila apache servera koji se po potrebi može dodati u konfiguracioni fajl bilo kog virtualnog hosta na serveru. Namena ovog fajla jeste blokiranje neželjenih URL zahteva. 6G vatreni zid je pažljivo razvijan dugi niz godina od strane iskusnig php developera i stručnjaka za bezbednost pod imenom `Jeff Star` i veoma dobro testiran i uglavnom radi bez problema kada je u pitanju Apache2/PHP/mysql set tehnologija. Više informacija o 6G vatrenom zidu možete da pročitate na veb sajtu njegovog autora na adresi: [https://perishablepress.com/6g/](https://perishablepress.com/6g/)
- `index.html` u slučaju da ne želite da instalirate Wordpress `index.html` se pri instalaciji kopira u `webroot` i sadrži linkove do kontrolne table i do `php.info` fajla, koji se koristi za testiranje funkcionalnosti php-a, kao i jednu jednostravnu igricu u css-u, zahvaljujući autoru `Nathan Taylor` iz Tokia - izvor: [https://codepen.io/nathantaylor/pen/KaLvXw](https://codepen.io/nathantaylor/pen/KaLvXw)

# Čuvanje podataka
Pri konfigurisanju servera skripta će napraviti direktorijum `.podaci` u `root` direktorijumu servera (ukoliko tako odaberete u poslednjem koraku).

Unutar foldera `.podaci` biće napravljeni sledeći fajlovi:
- podaci.txt
- ssl-info.txt

Fajl `podaci.txt` sadrži sve podatke koje ste uneli na početku instalacije.

Fajl `ssl-info.txt` sadrži podatke o instaliranom SSL sertifikatu (naziv i putanje na serveru)

Licenca: [MIT](LICENSE)
