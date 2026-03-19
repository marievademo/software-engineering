# Whitebox Tests – Order Module (`order.c`)

## Korte beschrijving van de module (metacode)

```
MODULE order.c

  FUNCTIE slaBestellingOp(klant_id, gerechten[], aanpassingen[]):
    ALS gerechten[] LEEG:
      RETURN foutmelding "Geen gerechten geselecteerd"
    genereer uniek bestellings_id
    sla tijdstip op (timestamp)
    sla bestelling op in databank
    ALS databank schrijffout:
      RETURN foutmelding "Opslaan mislukt"
    ROEP kitchen.c::stuurBestellingNaarKeuken(bestelling)
      ALS kitchen.c niet bereikbaar: registreer retry, RETURN status "keuken_onbereikbaar"
    ROEP payment.c::berekenTotaalbedrag(bestelling)
      ALS payment.c fout: sla betaling op als "pending"
    RETURN bestellings_id

  FUNCTIE ontvangStatusUpdate(bestellings_id, nieuwe_status):
    ALS bestellings_id NIET BESTAAT:
      RETURN foutmelding "Onbekende bestelling"
    ALS status GELIJK AAN huidige_status:
      RETURN (geen actie, idempotent)
    werk status bij in systeem
    update klantscherm met nieuwe_status

  FUNCTIE stelStatusIn(bestellings_id, status):  // "Geleverd"
    valideer bestellings_id
    zet status → Geleverd
    ROEP slaBestellingshistorieOp(bestellings_id)

  FUNCTIE slaBestellingshistorieOp(bestellings_id):
    ROEP user.c::koppelBestellingAanGebruiker(bestellings_id)
    sla historierecord op in databank

END MODULE
```

---

## Legenda – Knooppuntlabels

> Deze labels worden gebruikt in de **Pad**-verwijzingen bij elke test.

| Label | Type | Beschrijving |
|-------|------|--------------|
| A | Start | Start: Klant bekijkt menu |
| B | Stap | Gerecht selecteren |
| C | Beslissing | Aanpassingen toevoegen? |
| D | Stap | Aanpassing invoeren (bv. geen look) |
| E | Stap | Gerecht toevoegen aan winkelmandje |
| F | Beslissing | Meer gerechten toevoegen? |
| G | Stap | Winkelmandje tonen |
| H | Beslissing | Bestelling bevestigen? (Aanpassen / Bevestigen) |
| I | Stap | Item verwijderen of wijzigen |
| J | Stap | order.c: Bestelling opslaan in systeem |
| K | Stap | order.c: Bestellings-ID + tijdstip aanmaken |
| L | Stap | kitchen.c: Bestelling doorsturen naar keuken *(parallel met M)* |
| M | Stap | payment.c: Totaalbedrag berekenen *(parallel met L)* |
| N | Stap | Keuken verwerkt bestelling |
| O | Stap | kitchen.c: Status → In bereiding |
| P | Stap | order.c: Statusupdate ontvangen |
| Q | Stap | Klantscherm toont: In bereiding |
| R | Stap | Keuken voltooit bereiding |
| S | Stap | kitchen.c: Status → Klaar |
| T | Stap | order.c: Statusupdate ontvangen |
| U | Stap | Klantscherm toont: Klaar |
| V | Stap | Levering aan tafel |
| W | Stap | order.c: Status → Geleverd |
| X | Stap | order.c: Bestellingshistorie opslaan via user.c |
| Y | Einde | Einde |

---

## Whitebox Tests

### WT-01 – Bestelling opslaan: normaal pad

**Functie:** `slaBestellingOp()`  
**Doel:** Verifieer dat een geldige bestelling correct wordt opgeslagen met een uniek ID en tijdstip.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Geef geldige `klant_id` en een niet-lege lijst van gerechten | Functie retourneert een niet-leeg `bestellings_id` |
| 2 | Controleer tijdstip | Tijdstip is ingevuld en ligt binnen het huidige tijdvenster |
| 3 | Controleer systeemopslag | Record bestaat in de databank |
| 4 | Controleer oproep `kitchen.c` | `stuurBestellingNaarKeuken()` werd exact 1× aangeroepen |
| 5 | Controleer oproep `payment.c` | `berekenTotaalbedrag()` werd exact 1× aangeroepen |

**Pad:** J → K → L & M  
**Testtype:** Normaal pad (happy path)

---

### WT-02 – Bestelling opslaan: lege gerechtenlijst

**Functie:** `slaBestellingOp()`  
**Doel:** Verifieer foutafhandeling bij een bestelling zonder gerechten.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Geef geldige `klant_id` maar lege `gerechten[]` | Functie retourneert foutmelding "Geen gerechten geselecteerd" |
| 2 | Controleer systeemopslag | Geen record aangemaakt in databank |
| 3 | Controleer externe oproepen | `kitchen.c` en `payment.c` worden **niet** aangeroepen |

**Pad:** J (invoervalidatie mislukt) → foutpad  
**Testtype:** Grensgeval (boundary test)

---

### WT-03 – Statusupdate ontvangen: geldig pad "In bereiding"

**Functie:** `ontvangStatusUpdate()`  
**Doel:** Verifieer dat de status correct wordt bijgewerkt naar "In bereiding" en het klantscherm wordt bijgewerkt.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Roep aan met geldig `bestellings_id` en status `IN_BEREIDING` | Status wordt bijgewerkt in systeem |
| 2 | Controleer klantscherm-output | Klantscherm toont "In bereiding" |

**Pad:** O → P → Q  
**Testtype:** Normaal pad

---

### WT-04 – Statusupdate ontvangen: geldig pad "Klaar"

**Functie:** `ontvangStatusUpdate()`  
**Doel:** Verifieer dat de status correct wordt bijgewerkt naar "Klaar" en het klantscherm wordt bijgewerkt.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Roep aan met geldig `bestellings_id` en status `KLAAR` | Status wordt bijgewerkt in systeem |
| 2 | Controleer klantscherm-output | Klantscherm toont "Klaar" |

**Pad:** S → T → U  
**Testtype:** Normaal pad

---

### WT-05 – Statusupdate ontvangen: ongeldig bestellings_id

**Functie:** `ontvangStatusUpdate()`  
**Doel:** Verifieer foutafhandeling bij een onbekend `bestellings_id`.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Geef niet-bestaand `bestellings_id` | Functie retourneert foutmelding "Onbekende bestelling" |
| 2 | Controleer systeem | Geen statuswijziging opgeslagen |
| 3 | Controleer klantscherm | Geen update verstuurd naar klant |

**Pad:** ontvangStatusUpdate (ID niet gevonden) → foutpad  
**Testtype:** Foutpad (exception path)

---

### WT-06 – Status instellen op "Geleverd" + historiekoppeling

**Functies:** `stelStatusIn()` → `slaBestellingshistorieOp()`  
**Doel:** Verifieer dat na levering de status correct wordt gezet en de historiekoppeling via `user.c` plaatsvindt.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Roep `stelStatusIn(bestellings_id, GELEVERD)` aan | Status wordt `GELEVERD` in systeem |
| 2 | Controleer oproep `user.c` | `koppelBestellingAanGebruiker()` werd exact 1× aangeroepen |
| 3 | Controleer historierecord | Record bestaat in databank met correct `bestellings_id` |

**Pad:** W → X → Y  
**Testtype:** Normaal pad

---

### WT-07 – Dubbele statusupdate (idempotentie)

**Functie:** `ontvangStatusUpdate()`  
**Doel:** Verifieer dat een identieke statusupdate geen fout of dubbel record veroorzaakt.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Stuur status `KLAAR` twee keer achter elkaar | Geen crash, geen dubbel record |
| 2 | Controleer systeemstatus | Status blijft `KLAAR`, slechts 1 record gewijzigd |

**Pad:** ontvangStatusUpdate (status = huidige status) → geen actie  
**Testtype:** Robuustheid (idempotentie)

---

### WT-08 – Parallelle oproep kitchen.c en payment.c

**Functie:** `slaBestellingOp()`  
**Doel:** Verifieer dat `kitchen.c` en `payment.c` beiden worden aangeroepen na het aanmaken van het bestellings-ID, onafhankelijk van elkaar.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Sla geldige bestelling op | Beide functies worden aangeroepen (volgorde onafhankelijk) |
| 2 | Simuleer vertraging in `payment.c` | `kitchen.c` blokkeert niet; bestelling gaat door naar keuken |

**Pad:** K → L & M (parallelle splitsing)  
**Testtype:** Concurrentie / parallelle verwerking

---

### WT-09 – Bestelling opslaan: databasefout

**Functie:** `slaBestellingOp()`  
**Doel:** Verifieer dat een databasefout tijdens het opslaan correct wordt afgehandeld.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Simuleer een database schrijffout | Functie retourneert foutmelding "Opslaan mislukt" |
| 2 | Controleer oproep `kitchen.c` | Niet aangeroepen |
| 3 | Controleer oproep `payment.c` | Niet aangeroepen |
| 4 | Controleer logging | Foutmelding intern geregistreerd |

**Pad:** J → K (database fout) → foutpad  
**Testtype:** Exception path

---

### WT-10 – Keukenmodule niet bereikbaar

**Functie:** `slaBestellingOp()`  
**Doel:** Verifieer systeemgedrag wanneer `kitchen.c` niet bereikbaar is na het opslaan van de bestelling.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Simuleer timeout/fout in `stuurBestellingNaarKeuken()` | Fout wordt gedetecteerd |
| 2 | Controleer orderstatus | Status blijft "ontvangen", niet "in bereiding" |
| 3 | Controleer retry-mechanisme | Systeem registreert poging voor herverbinding |
| 4 | Controleer logging | Fout geregistreerd in systeem |

**Pad:** K → L (kitchen.c fout) → retry/foutpad  
**Testtype:** Robuustheid / externe fout

---

### WT-11 – Betalingsmodule geeft fout terug

**Functie:** `slaBestellingOp()`  
**Doel:** Verifieer dat een fout in de betalingsberekening de bestelling niet blokkeert maar als "pending" wordt gemarkeerd.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Simuleer fout in `berekenTotaalbedrag()` | Exception opgevangen |
| 2 | Controleer bestelling in databank | Bestelling blijft opgeslagen |
| 3 | Controleer betalingsstatus | Betaling gemarkeerd als `pending` |
| 4 | Controleer logging | Fout geregistreerd voor opvolging |

**Pad:** K → M (payment.c fout) → bestelling behouden, betaling = pending  
**Testtype:** Exception path / gedeeltelijke fout

---

## Dekkingsmatrix

| Test | Functie | Pad/Branch | Conditie getest |
|------|---------|------------|-----------------|
| WT-01 | `slaBestellingOp` | J → K → L & M | Geldige bestelling, normaal pad |
| WT-02 | `slaBestellingOp` | Invoervalidatie → foutpad | Lege gerechtenlijst |
| WT-03 | `ontvangStatusUpdate` | O → P → Q | Status = In bereiding |
| WT-04 | `ontvangStatusUpdate` | S → T → U | Status = Klaar |
| WT-05 | `ontvangStatusUpdate` | ID niet gevonden → foutpad | Ongeldig bestellings_id |
| WT-06 | `stelStatusIn` + `slaBestellingshistorieOp` | W → X → Y | Status Geleverd + historiek |
| WT-07 | `ontvangStatusUpdate` | Status = huidige status → geen actie | Idempotentie |
| WT-08 | `slaBestellingOp` | K → L & M (parallel) | Concurrente verwerking |
| WT-09 | `slaBestellingOp` | K (DB fout) → foutpad | Databasefout |
| WT-10 | `slaBestellingOp` | L (kitchen.c fout) → retry | Keukenmodule onbereikbaar |
| WT-11 | `slaBestellingOp` | M (payment.c fout) → pending | Betalingsmodule geeft fout |
