# Whitebox Tests – Gebruikersprofiel Module (`user.c`)

## Korte beschrijving van de module (metacode)

```
MODULE user.c

  FUNCTIE toonProfiel(klant_id):
    haal profielgegevens op (naam, taalvoorkeur, allergieënlijst)
    RETURN profieloverzicht

  FUNCTIE slaAllergieOp(klant_id, allergie):
    ALS allergie REEDS BESTAAT IN profiel:
      RETURN foutmelding "Allergie al aanwezig"
    voeg allergie toe aan profiel van klant
    ALS databank schrijffout:
      RETURN foutmelding "Opslaan mislukt"
    stuurAllergieënlijstNaar(menu.c)
    RETURN bevestiging

  FUNCTIE verwijderAllergie(klant_id, allergie):
    ALS allergie NIET BESTAAT IN profiel:
      RETURN foutmelding "Allergie niet gevonden"
    verwijder allergie uit profiel
    ALS databank schrijffout:
      RETURN foutmelding "Verwijderen mislukt"
    stuurAllergieënlijstNaar(menu.c)
    RETURN bevestiging

  FUNCTIE slaaTaalvoorkeurOp(klant_id, taal):
    ALS taal NIET IN (NL, FR, EN, ...):
      RETURN foutmelding "Ongeldige taalcode"
    sla taalvoorkeur op in profiel
    herlaadApp(taal)
    RETURN succes

  FUNCTIE haalBestellingshistorieOp(klant_id):
    RETURN lijst van vroegere bestellingen (kan leeg zijn)

  FUNCTIE plaatsBestellingOpnieuw(klant_id, bestelling_id):
    maak nieuwe bestelling aan op basis van oude bestelling
    voerAllergieëncheckUit(bestelling, profiel):
      ALS conflict GEVONDEN:
        toonWaarschuwing aan klant
        ALS klant kiest NIET door te gaan:
          RETURN annuleer (terug naar historieoverzicht)
    bevestigBestelling()
    RETURN doorsturen naar betalingsflow

END MODULE
```

---

## Legenda – Knooppuntlabels

> Deze labels worden gebruikt in de **Pad**-verwijzingen bij elke test.

| Label | Type | Beschrijving |
|-------|------|--------------|
| A | Start | Start: Klant opent profiel |
| B | Stap | Profieloverzicht tonen |
| C | Beslissing | Actie kiezen (Allergieën beheren / Taalvoorkeur instellen / Bestelhistorie bekijken) |
| D | Stap | Huidig allergieënprofiel tonen |
| E | Beslissing | Allergie toevoegen of verwijderen? |
| F | Stap | Allergie selecteren uit lijst (toevoegen) |
| G | Stap | user.c: Allergie opslaan in profiel |
| H | Stap | Bevestiging tonen |
| I | Stap | Allergieënlijst doorsturen naar menu.c |
| J | Stap | Menu filteren op allergieën bij volgend bezoek |
| K | Stap | Allergie selecteren om te verwijderen |
| L | Stap | user.c: Allergie verwijderen uit profiel |
| M | Stap | Taal kiezen: NL, FR, EN, … |
| N | Stap | Taalvoorkeur opslaan |
| O | Stap | App herladen in gekozen taal |
| P | Stap | Historieoverzicht ophalen |
| Q | Stap | Lijst van vroegere bestellingen tonen |
| R | Beslissing | Bestelling opnieuw plaatsen? (Ja / Nee) |
| S | Stap | Bestelling opnieuw aanmaken |
| T | Beslissing | Allergieëncheck: conflict met profiel? (Conflict / Geen conflict) |
| U | Stap | Waarschuwing tonen aan klant |
| V | Beslissing | Toch doorgaan? (Ja / Nee) |
| W | Stap | Bestelling bevestigen |
| X | Einde | Einde: naar betalingsflow |

---

## Whitebox Tests

### WT-01 – Allergie toevoegen: normaal pad

**Functie:** `slaAllergieOp()`  
**Doel:** Verifieer dat een nieuwe allergie correct wordt opgeslagen en de allergieënlijst naar `menu.c` wordt doorgestuurd.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Kies actie "Allergieën beheren" | Huidig allergieënprofiel getoond |
| 2 | Kies "Toevoegen", selecteer allergie uit lijst (bv. "gluten") | Allergie geselecteerd |
| 3 | Controleer databank | Allergie opgeslagen in profiel van klant |
| 4 | Controleer bevestiging | Bevestigingsmelding getoond |
| 5 | Controleer oproep `menu.c` | Allergieënlijst doorgestuurd naar `menu.c` |
| 6 | Controleer terugkeer | Profieloverzicht opnieuw getoond |

**Pad:** C -- Allergieën beheren → D → E -- Toevoegen → F → G → H → I → J → B  
**Testtype:** Normaal pad (happy path)

---

### WT-02 – Allergie toevoegen: reeds bestaande allergie

**Functie:** `slaAllergieOp()`  
**Doel:** Verifieer dat een duplicaat-allergie niet dubbel wordt opgeslagen.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Voeg allergie toe die al in het profiel staat | Systeem detecteert duplicaat |
| 2 | Controleer databank | Allergie slechts 1× aanwezig in profiel |
| 3 | Controleer feedback | Foutmelding "Allergie al aanwezig" getoond |
| 4 | Controleer oproep `menu.c` | Geen nieuwe doorstuur uitgevoerd |

**Pad:** G (duplicaatcontrole mislukt) → foutpad  
**Testtype:** Grensgeval (duplicaat)

---

### WT-03 – Allergie verwijderen: normaal pad

**Functie:** `verwijderAllergie()`  
**Doel:** Verifieer dat een bestaande allergie correct uit het profiel wordt verwijderd.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Kies "Verwijderen", selecteer bestaande allergie | Allergie geselecteerd |
| 2 | Controleer databank | Allergie verwijderd uit profiel |
| 3 | Controleer bevestiging | Bevestigingsmelding getoond |
| 4 | Controleer oproep `menu.c` | Bijgewerkte allergieënlijst doorgestuurd |

**Pad:** C -- Allergieën beheren → D → E -- Verwijderen → K → L → H → I → J → B  
**Testtype:** Normaal pad

---

### WT-04 – Allergie verwijderen: allergie niet aanwezig in profiel

**Functie:** `verwijderAllergie()`  
**Doel:** Verifieer foutafhandeling bij poging om een niet-bestaande allergie te verwijderen.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Probeer allergie te verwijderen die niet in profiel staat | Foutmelding "Allergie niet gevonden" getoond |
| 2 | Controleer databank | Geen wijziging in profiel |
| 3 | Controleer oproep `menu.c` | Geen doorstuur uitgevoerd |

**Pad:** L (allergie niet gevonden) → foutpad  
**Testtype:** Foutpad

---

### WT-05 – Taalvoorkeur instellen: geldige taal

**Functie:** `slaaTaalvoorkeurOp()`  
**Doel:** Verifieer dat een geldige taalvoorkeur correct wordt opgeslagen en de app herlaadt in de gekozen taal.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Kies actie "Taalvoorkeur instellen", selecteer "FR" | Taal geselecteerd |
| 2 | Controleer databank | Taalvoorkeur `FR` opgeslagen in profiel |
| 3 | Controleer app-gedrag | App herladen in het Frans |
| 4 | Controleer terugkeer | Profieloverzicht getoond in de nieuwe taal |

**Pad:** C -- Taalvoorkeur instellen → M → N → O → B  
**Testtype:** Normaal pad; herhalen voor NL en EN

---

### WT-06 – Taalvoorkeur instellen: ongeldige taalcode

**Functie:** `slaaTaalvoorkeurOp()`  
**Doel:** Verifieer dat een niet-ondersteunde taalcode wordt geweigerd zonder de huidige voorkeur te wijzigen.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Geef ongeldige taalcode (bv. `XX`) | Foutmelding "Ongeldige taalcode" getoond |
| 2 | Controleer databank | Taalvoorkeur niet gewijzigd |
| 3 | Controleer app | App herlaadt niet |

**Pad:** M → N (validatie mislukt) → foutpad  
**Testtype:** Foutpad / grensgeval

---

### WT-07 – Bestelhistorie bekijken: geen herbestelling

**Functie:** `haalBestellingshistorieOp()`  
**Doel:** Verifieer dat de bestelhistorie correct wordt opgehaald en getoond, en dat de gebruiker kan terugkeren zonder herbestelling.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Kies actie "Bestelhistorie bekijken" | Historieoverzicht opgehaald |
| 2 | Controleer weergave | Lijst van vroegere bestellingen getoond |
| 3 | Kies "Niet opnieuw plaatsen" | Terugkeer naar profieloverzicht |

**Pad:** C -- Bestelhistorie bekijken → P → Q → R -- Nee → B  
**Testtype:** Normaal pad

---

### WT-08 – Bestelling opnieuw plaatsen: geen allergieënconflict

**Functie:** `plaatsBestellingOpnieuw()`  
**Doel:** Verifieer dat een herbestelling zonder allergieconflict direct wordt bevestigd en doorgestuurd.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Kies een vroegere bestelling en kies "Opnieuw plaatsen" | Nieuwe bestelling aangemaakt |
| 2 | Allergieëncheck uitgevoerd | Geen conflict met huidig profiel |
| 3 | Controleer doorstuur | Bestelling bevestigd, doorgestuurd naar betalingsflow |

**Pad:** R -- Ja → S → T -- Geen conflict → W → X  
**Testtype:** Normaal pad (happy path)

---

### WT-09 – Bestelling opnieuw plaatsen: allergieënconflict, klant gaat toch door

**Functie:** `plaatsBestellingOpnieuw()`  
**Doel:** Verifieer dat bij een allergieconflict een waarschuwing wordt getoond en de klant kan kiezen toch door te gaan.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Kies herbestelling met ingredient dat conflicteert met profiel | Allergieëncheck detecteert conflict |
| 2 | Controleer waarschuwing | Waarschuwingsmelding getoond aan klant |
| 3 | Klant kiest "Toch doorgaan" | Bestelling bevestigd |
| 4 | Controleer doorstuur | Doorgestuurd naar betalingsflow |

**Pad:** T -- Conflict → U → V -- Ja → W → X  
**Testtype:** Normaal pad na waarschuwing

---

### WT-10 – Bestelling opnieuw plaatsen: allergieënconflict, klant annuleert

**Functie:** `plaatsBestellingOpnieuw()`  
**Doel:** Verifieer dat de klant na een conflictwaarschuwing kan annuleren en veilig terugkeert naar de historieweergave.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Allergieëncheck detecteert conflict, waarschuwing getoond | Waarschuwing zichtbaar |
| 2 | Klant kiest "Niet doorgaan" | Bestelling geannuleerd |
| 3 | Controleer navigatie | Terugkeer naar lijst van vroegere bestellingen |

**Pad:** T -- Conflict → U → V -- Nee → Q  
**Testtype:** Foutpad met veilige terugkeer

---

### WT-11 – Bestelhistorie bekijken: lege historiek

**Functie:** `haalBestellingshistorieOp()`  
**Doel:** Verifieer dat een klant zonder bestellingshistorie een gepaste melding krijgt en geen crash optreedt.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Open bestelhistorie voor nieuwe klant zonder bestellingen | Lege lijst opgehaald (geen fout) |
| 2 | Controleer weergave | Melding "Geen eerdere bestellingen" of lege staat getoond |
| 3 | Controleer opties | Optie "Opnieuw plaatsen" niet beschikbaar of uitgeschakeld |

**Pad:** P → Q (lege lijst) → R (geen opties)  
**Testtype:** Grensgeval (lege dataset)

---

### WT-12 – Allergie opslaan: databasefout

**Functie:** `slaAllergieOp()`  
**Doel:** Verifieer foutafhandeling wanneer de databank niet bereikbaar is tijdens het opslaan van een allergie.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Selecteer allergie om toe te voegen | Allergie geselecteerd |
| 2 | Simuleer een database schrijffout | Foutmelding "Opslaan mislukt" getoond |
| 3 | Controleer profiel in databank | Allergie **niet** opgeslagen |
| 4 | Controleer oproep `menu.c` | Niet uitgevoerd |

**Pad:** G (database schrijffout) → foutpad  
**Testtype:** Exception path

---

## Dekkingsmatrix

| Test | Functie | Pad/Branch | Conditie getest |
|------|---------|------------|-----------------|
| WT-01 | `slaAllergieOp` | C → D → E -- Toevoegen → F → G → H → I → J → B | Geldige allergie, normaal pad |
| WT-02 | `slaAllergieOp` | G (duplicaat) → foutpad | Reeds bestaande allergie |
| WT-03 | `verwijderAllergie` | E -- Verwijderen → K → L → H → I → J → B | Bestaande allergie verwijderd |
| WT-04 | `verwijderAllergie` | L (niet gevonden) → foutpad | Allergie niet aanwezig in profiel |
| WT-05 | `slaaTaalvoorkeurOp` | C → M → N → O → B | Geldige taalcode |
| WT-06 | `slaaTaalvoorkeurOp` | N (validatie mislukt) → foutpad | Ongeldige taalcode |
| WT-07 | `haalBestellingshistorieOp` | P → Q → R -- Nee → B | Historiek bekeken, geen herbestelling |
| WT-08 | `plaatsBestellingOpnieuw` | R -- Ja → S → T -- Geen conflict → W → X | Herbestelling zonder conflict |
| WT-09 | `plaatsBestellingOpnieuw` | T -- Conflict → U → V -- Ja → W → X | Conflict, klant gaat toch door |
| WT-10 | `plaatsBestellingOpnieuw` | T -- Conflict → U → V -- Nee → Q | Conflict, klant annuleert |
| WT-11 | `haalBestellingshistorieOp` | P → Q (leeg) | Lege historiek |
| WT-12 | `slaAllergieOp` | G (DB fout) → foutpad | Databasefout bij opslaan |
