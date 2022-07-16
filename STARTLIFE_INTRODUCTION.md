[![Version](https://img.shields.io/badge/Version-1.5-yellow.svg)](https://conventionalcommits.org)

_Having problems? Join us on the [start-life support Slack](https://start-life.slack.com/signin#/signin)_.

# StartLife

**"Dynamic Insurance Revolution"** (Novi koncept osiguranja i Lojaliti programa) koji u bitnom podrazumeva zaključenje polisa životnog 
osiguranja sa dinamičnim (promenljivim) osiguranim sumama i dinamičnom (promenljivom) premijom osiguranja a čiji je esencijalni deo 
učešće partnera u okviru Lojaliti programa u smislu da partneri koji su trgovci i odobravaju osiguranicima/ugovaračima osiguranja 
benefite u vidu "cash back-a" i sistema "zaokruži", a da svi ti benefiti predstavljaju premiju životnog osiguranja koja se uplaćuje na 
polisu osiguranika, koju je prethodno zaključio sa osiguravačem.

## Poslovna logika

Komunikacija sa **StartLife** informacionim sistemom slanjem zahteva (request-a); sadrži dve komande LYT_GETPOINTS i LYT_SETPOINTS. 
Komandom LYT_GETPOINTS vrši se provera statusa polise kupca, da li je u aktivnom statusu (parametar **active**) i da li je moguće dodati 
iznos za štednju komandom LYT_SETPOINTS, podrazumeva se da je moguće dodati iznos samo na aktivnu polisu, odnosno aktivnu karticu. 
Ukoliko kasa ipak pošalje zahtev za dodavanje benefita (LYT_SETPOINTS), bez obzira što je prethodno dobila upozorenje da je kartica neaktivna, 
sistem će odbiti zahtev uz odgovarajuću poruku.<br/>
U odgovru na LYT_GETPOINTS komandu zahteva parametar **amountmax** prikazuje informaciju koliko je još moguće uplatiti na polisu do godišnjeg limita, 
polisu je moguće dopunjavati sve dok je vrednost amountmax veća od nule, godišnji limit iznosi 96.000 dinara što je i inicijalna vrednost amountmax.
Iznos koji se dodaje na polisu kupca se računa u odnosu na konfiguraciju trgovinskog lanca, sistem obračunava iznos u odnosu na vrednost 
izraženu u procentima koji se obračunava u odnosu na vrednost koju kasa prosleđuje za obračun 
(vrednost/iznos za obračun se može razlikovati od ukupnog iznosa računa npr. izuzimaju se zaštićeni proizvodi), obično 10%, 15%, 20%.. 
zavisno od ugovorene vrednosti između trgovinskog lanca i Generali osiguranja.
Kasa nema informaciju koliko će biti obračunato za uplatu na polisu kupca i samim tim ne može da predviđa iznos u slučaju da je amountmax
veći od nule drugim rečima samo ako je vrednost nula kasa ne treba da šalje zahtev i korisnik treba da dobije informaciju da je iskoristio limit,
a za sve druge situacije ta odgovornost je na strani **StartLife** informacionim sistema, a obračunata vrednost će biti u odgovoru
zahteva u parametru **lifeamount** koju kasa treba da odštampa kupcu.

## LIFE WEB Api

Tehnička specifikacija opisuje protkol za razmenu podataka između klijenata i servera i tačan opis postupaka povezanih sa sistemom LIFE za POS. 
Dokument opisuje sve tehničke aspekte I smernice koji se moraju ispuniti u procesu integracije.
Neophodno je da POS podržava TCP/IP komunikaciju pomoću HTTPS protokola i može direktno da komunicira sa Life serverom. 
Produkcioni URL I testni URLLIFE servera dat je u dodatku A. Zahtev treba slati metodom POST,a u “body” – telu zahtevase prosleđuje xml ili json 
objekat. Odgovori servera se takođe interpretiraju u xml ili json formatu u odnosu na zahtev. 
Komunikacija sa Life Web Api-jem je sinhrona, svaki zahtev sa odgovarajućom komandom LYT_GETPOINTS (čitanje) ili LYT_SETPOINTS (transakcija/upis)
će vratiti odgovor u propisanom formatu.