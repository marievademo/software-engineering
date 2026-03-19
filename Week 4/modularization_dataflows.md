# Modularization – Dataflows tussen modules (2p)
Voor elke dataflow wordt aangegeven welke module de data verstuurt (output) en welke module ze ontvangt (input), samen met de concrete data die wordt doorgegeven.

## Overzicht dataflows
| #  | Van       | Naar     | Data (output → input) |
|--- |-----      |-----     |-----------------------|
| 1  | `main`    | `auth`   | Ingevoerde gebruikersnaam + wachtwoord |
| 2  | `auth`    | `main`   | Gebruikersrol + gebruikers-ID |
| 3  | `main`    | `menu`   | Verzoek om menulijst op te halen |
| 4  | `main`    | `kitchen`| Verzoek om orderoverzicht te tonen |
| 5  | `main`    | `admin`  | Verzoek om beheerderspaneel te openen |
| 6  | `user`    | `menu`   | Allergieënlijst voor filtering |
| 7  | `menu`    | `order`  | Geselecteerd gerecht (ID, naam, prijs, hoeveelheid) |
| 8  | `order`   | `kitchen`| Nieuwe bestelling (items, tafel-ID, tijdstip) |
| 9  | `kitchen` | `order`  | Statusupdate (bijv. "in bereiding", "klaar") |
| 10 | `order`   | `payment`| Bestellings-ID + totaalbedrag |
| 11 | `payment` | `utils`  | E-mailadres + rekeningdata voor verzending |
| 12 | `payment` | `user`   | Bevestiging betaling voor spaarpunten |
| 13 | `order`   | `user`   | Afgeronde bestelling voor historieoverzicht |
| 14 | `admin`   | `menu`   | CRUD-operaties (nieuw gerecht, aanpassing, verwijdering) |
| 15 | `admin`   | `user`   | Beheersactie op klantaccount (blokkeren, verwijderen) |
| 16 | `admin`   | `order`  | Verzoek om orderoverzicht per tafel |
| 17 | `review`  | `user`   | Recensie koppelen aan klantprofiel |
| 18 | `auth`    | `utils`  | Ongeldige inlogpoging voor foutlogging |
| 19 | `utils`   | alle modules | Foutmelding of validatieresultaat |

## Gedetailleerde beschrijving per flow

### Flow 1–2: Inloggen (`main` ↔ `auth`)
`main` geeft de ingevoerde gebruikersnaam en het wachtwoord door aan `auth`. De `auth`-module valideert deze gegevens en stuurt de bijbehorende gebruikersrol (`KLANT`, `ADMIN`, `KEUKEN`) en het gebruikers-ID terug naar `main`.

### Flow 6: Allergieënfiltering (`user` → `menu`)
Wanneer een ingelogde klant het menu opvraagt, haalt `menu` de allergieënlijst op uit `user` en filtert gerechten die de allergenen bevatten automatisch weg of markeert ze.

### Flow 7–8: Bestelling plaatsen (`menu` → `order` → `kitchen`)
De klant selecteert items uit `menu`. Deze worden met prijs en ID doorgegeven aan `order`, die een bestelling aanmaakt en deze met tafel-ID en tijdstip doorstuurt naar `kitchen`.

### Flow 9: Statusupdate (`kitchen` → `order`)
Wanneer keuken- of barpersoneel de status van een bestelling aanpast, stuurt `kitchen` de nieuwe status door naar `order`. `order` slaat de update op en maakt ze zichtbaar voor de klant.

### Flow 10–11: Betaling (`order` → `payment` → `utils`)
Na het bevestigen van een bestelling geeft `order` het bestellings-ID en totaalbedrag door aan `payment`. Na succesvolle betaling roept `payment` de `utils`-module aan om de rekening per e-mail te sturen.

### Flow 14: Menubeheer (`admin` → `menu`)
De admin kan via het beheerderspaneel gerechten toevoegen, wijzigen of verwijderen. `admin` stuurt de gewijzigde menudata door naar `menu`, die de database bijwerkt.

### Flow 19: Foutafhandeling (`utils` → alle modules)
Alle modules kunnen `utils` aanroepen voor invoervalidatie. `utils` geeft een validatieresultaat of foutmelding terug die de aanroepende module kan verwerken en tonen aan de gebruiker.

## Visueel overzicht
                        ┌─────────┐
                        │  main   │
                        └────┬────┘
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
           ┌──────┐     ┌────────┐     ┌────────┐
           │ auth │     │  menu  │◄────│  user  │
           └──────┘     └───┬────┘     └────┬───┘
                            │               ▲
                            ▼               │
                        ┌───────┐           │
                        │ order ├───────────┘
                        └───┬───┘
               ┌────────────┼────────────┐
               ▼            ▼            ▼
          ┌─────────┐  ┌─────────┐  ┌─────────┐
          │ kitchen │  │ payment │  │  admin  │
          └─────────┘  └────┬────┘  └─────────┘
                            │
                            ▼
                        ┌───────┐
                        │ utils │◄──── alle modules
                        └───────┘
```
