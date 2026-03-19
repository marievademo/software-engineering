# Modularization – Restaurant Orderingsysteem
## Gebruikte methode: Functionele decompostie
Voor dit project gebruiken we **functionele decompositie** als moduleringstrategie. Dit wil zeggen dat het programma wordt opgesplitst op basis van wat elke functie *doet*. Elke module heeft één duidelijke verantwoordelijkheid en dus één duidelijk doel. We gaan dit uitwerken in C met aparte `.h` en `.c` bestanden per module.

### Prompt gebruikt voor AI-ondersteuning
> "In bijlage kan u mijn 

## Modulaire opdeling
### Module 1: `auth` — Authenticatie
Het doel van deze module is voor alles rond inloggen en registeren te verzamelen. Het controleert of de gebruiker bestaat, dan valideert de modeule het wachtwoord van de gebruiker en bepaalt dan de rol van de gebruiker. Dus of dat de gebruiker een klant, admin, of ober is.
**Bestanden:** `auth.h`, `auth.c`

### Module 2: `menu` — Menubeheer
Bij deze module wordt het menu beheerd. De admin kan gerechten toevoegen, aanpassen of verwijderen. Klanten kunnen het menu opvragen en eventueel filteren op categorie voor moesten ze dat willen. 
**Bestanden:** `menu.h`, `menu.c`

### Module 3: `order` — Bestelbeheer
Deze module is verantwoordelijk voor het bestellen. Het gaat de ingevoerde bestelling van de gebruiker verwerken, opslaan en opvolgen. Het bevat de lociga voor het winkelmandje, het plaatsen van een bestelling en het bijwerken van de orderstatus.
**Bestanden:** `order.h`, `order.c`

### Module 4: `payment` — Betalingsverwerking
Module 4 verwerkt de betaling van een bestelling. Het berekent het totaalbedrag van de klant, verstuurt de rekening per e-mail en registreert de betaling in het systeem voor de admin.
**Bestanden:** `payment.h`, `payment.c`

### Module 5: `user` — Gebruikersprofielbeheer
Module 5 is verantwoordelijk voor de klantprofielen: allergieën, taalvoorkeur, bestelhistorie en spaarpunten. De admin kan via deze module ook accounts beheren.
**Bestanden:** `user.h`, `user.c`

### Module 6: `kitchen` — Keuken/Bar interface
Deze module geeft de keuken- en barpersoneel een overzicht van de actieve bestellingen. Het laat ze toe om de status van een bestelling aan te passen (bijv. "in bereiding" → "klaar").
**Bestanden:** `kitchen.h`, `kitchen.c`

### Module 7: `admin` — Beheerderspaneel
Module 7 is specifiek voor de admin: openingsuren van het restaurant beheren, speciale aanbiedingen instellen op de menu pagina, financiële rapporten genereren en klantaccounts raadplegen.
**Bestanden:** `admin.h`, `admin.c`

### Module 8: `review` — Recensies
Deze module laat klanten toe om recensies te schrijven en te lezen. Het is veranwtoordelijk om beoordelingen op te slaan en ze te koppellen aan het juiste gerecht.
**Bestanden:** `review.h`, `review.c`

### Module 9: `utils` — Hulpfuncties
Module 9 bevat algemene hulpfuncties die door meerdere modules gebruikt worden: invoervalidatie, datumformattering, e-mailverzending en foutafhandeling.
**Bestanden:** `utils.h`, `utils.c`

### Module 10: `main` — Ingangspunt
Module 10 is eigenlijk het begin van heel het programma. Het start het programma op, initialiseert alle modules en stuurt de gebruiker naar het juiste scherm op basis van zijn rol.
**Bestanden:** `main.c`

## Dataflows tussen modules
| Van       | Naar         | Data |
|-----      |------        |------|
| `main`    | `auth`       | Inloggegevens (gebruikersnaam, wachtwoord) |
| `auth`    | `main`       | Gebruikersrol + gebruikers-ID |
| `main`    | `menu`       | Verzoek om menulijst op te halen |
| `menu`    | `order`      | Geselecteerde gerechten (ID, naam, prijs) |
| `order`   | `payment`    | Totaalbedrag + bestellings-ID |
| `order`   | `kitchen`    | Nieuwe bestelling (items, tafelID, tijdstip) |
| `kitchen` | `order`      | Statusupdate (bijv. "klaar") |
| `order`   | `user`       | Bestellingshistorie opslaan |
| `user`    | `menu`       | Allergieënlijst voor filtering |
| `payment` | `utils`      | E-mailadres + rekening voor verzending |
| `admin`   | `menu`       | CRUD-operaties op gerechten |
| `admin`   | `user`       | Klantaccountbeheer |
| `admin`   | `order`      | Orderoverzicht per tafel |
| `review`  | `user`       | Recensie koppelen aan klantprofiel |
| `utils`   | alle modules | Foutmeldingen, invoervalidatie |

## Bestandsstructuur
restaurant_system/
├── main.c
├── auth/
│   ├── auth.h
│   └── auth.c
├── menu/
│   ├── menu.h
│   └── menu.c
├── order/
│   ├── order.h
│   └── order.c
├── payment/
│   ├── payment.h
│   └── payment.c
├── user/
│   ├── user.h
│   └── user.c
├── kitchen/
│   ├── kitchen.h
│   └── kitchen.c
├── admin/
│   ├── admin.h
│   └── admin.c
├── review/
│   ├── review.h
│   └── review.c
└── utils/
    ├── utils.h
    └── utils.c