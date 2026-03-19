# Blackbox Tests – User Module
Gebaseerd op het activiteitendiagram van de User-module. De module bevat drie submodules: **Allergieën beheren**, **Taalvoorkeur instellen** en **Bestelhistorie bekijken**.

## Module 1: Allergieën beheren
Deze module laat de klant toe allergieën toe te voegen aan of te verwijderen uit het profiel. Na een wijziging wordt de bijgewerkte allergieënlijst doorgestuurd naar `menu.c` zodat het menu gefilterd wordt bij een volgend bezoek.
| Test ID   | Beschrijving                                                          | Input | Verwachte output |
|---------  |-------------                                                          |-------|-----------------|
| A-01      | Allergie toevoegen – happy path                                       | Klant opent profiel, kiest "Allergieën beheren", kiest "Toevoegen", selecteert een allergie uit de lijst | Allergie wordt opgeslagen in `user.c`, bevestiging wordt getoond, allergieënlijst wordt doorgestuurd naar `menu.c` |
| A-02      | Allergie verwijderen – happy path                                     | Klant kiest "Verwijderen", selecteert een bestaande allergie | Allergie wordt verwijderd uit `user.c`, bevestiging wordt getoond, bijgewerkte lijst wordt doorgestuurd naar `menu.c` |
| A-03      | Toevoegen zonder selectie                                             | Klant kiest "Toevoegen" maar selecteert geen allergie | Geen opslag; systeem toont foutmelding of blijft op de selectiepagina |
| A-04      | Verwijderen zonder selectie                                           | Klant kiest "Verwijderen" maar selecteert geen allergie | Geen wijziging in profiel; systeem toont foutmelding of blijft op de selectiepagina |
| A-05      | Duplicaat toevoegen                                                   | Klant voegt een allergie toe die al in het profiel staat | Geen duplicaat opgeslagen; gepaste melding wordt getoond |
| A-06      | Profiel tonen bij openen | Klant navigeert naar "Allergieën beheren"  | Huidig allergieënprofiel wordt correct weergegeven |
| A-07      | Doorsturen naar menu.c na wijziging                                   | Na toevoegen of verwijderen van een allergie | Menu wordt gefilterd op basis van de bijgewerkte lijst bij het volgende bezoek |

## Module 2: Taalvoorkeur instellen
Deze module laat de klant een taal kiezen (bv. NL, FR, EN, …). De voorkeur wordt opgeslagen en de app wordt herladen in de gekozen taal.
| Test ID   | Beschrijving                      | Input | Verwachte output |
|---------  |-------------                      |-------|-----------------|
| T-01      | Taal instellen – happy path       | Klant kiest "Taalvoorkeur instellen", selecteert een taal (bv. FR) | Taalvoorkeur wordt opgeslagen, app herlaadt volledig in FR |
| T-02      | Dezelfde taal opnieuw kiezen      | Klant selecteert de taal die al actief is | Taalvoorkeur wordt opgeslagen zonder fout; app herlaadt |
| T-03      | Alle beschikbare talen testen     | Klant selecteert achtereenvolgens elke beschikbare taal (NL, FR, EN, …) | App herlaadt telkens correct in de geselecteerde taal; alle UI-elementen zijn vertaald |
| T-04      | Annuleren zonder taal te kiezen   | Klant opent de submodule maar verlaat zonder te kiezen | Huidige taalvoorkeur blijft ongewijzigd; app herlaadt niet |
| T-05      | Persistentie van taalvoorkeur     | Klant stelt taal in, sluit de app en heropent | App start op in de eerder gekozen taal |

## Module 3: Bestelhistorie bekijken
Deze module toont een overzicht van vroegere bestellingen. De klant kan een bestelling opnieuw plaatsen, waarbij een allergieëncheck wordt uitgevoerd.
| Test ID   | Beschrijving | Input | Verwachte output |
|---------  |-------------|-------|-----------------|
| B-01      | Historieoverzicht ophalen – happy path | Klant opent "Bestelhistorie bekijken" | Lijst van vroegere bestellingen wordt correct weergegeven |
| B-02      | Geen bestelhistorie beschikbaar | Klant heeft nog nooit een bestelling geplaatst | Lege lijst of melding "geen bestellingen gevonden" |
| B-03      | Opnieuw plaatsen – geen conflict | Klant kiest een bestelling zonder conflicterende allergenen | Allergieëncheck slaagt, bestelling bevestigd, klant gaat naar betalingsflow |
| B-04      | Opnieuw plaatsen – conflict, klant gaat toch door | Bestelling bevat conflicterend allergeen; klant kiest "Toch doorgaan" | Waarschuwing wordt getoond, klant bevestigt, bestelling wordt doorgestuurd naar betalingsflow |
| B-05      | Opnieuw plaatsen – conflict, klant annuleert | Bestelling bevat conflicterend allergeen; klant kiest niet door te gaan | Waarschuwing wordt getoond, bestelling niet aangemaakt, klant keert terug naar overzicht |
| B-06      | Bestelling niet opnieuw plaatsen | Klant bekijkt historieoverzicht maar plaatst niets opnieuw | Klant keert terug naar profieloverzicht; geen nieuwe bestelling aangemaakt |
| B-07      | Allergieëncheck zonder allergieënprofiel | Klant heeft geen allergieën ingesteld; plaatst bestelling opnieuw | Geen conflict gedetecteerd; bestelling wordt normaal bevestigd |
| B-08      | Correctheid van historiedata | Klant bekijkt historieoverzicht | Getoonde bestellingen (items, datum, bedrag) komen overeen met de werkelijk geplaatste bestellingen |

## Algemene / Cross-module Tests
| Test ID   | Beschrijving | Input | Verwachte output |
|---------  |-------------|-------|-----------------|
| G-01      | Terugkeer naar profieloverzicht | Klant sluit een submodule af (na of zonder actie) | Profieloverzicht wordt opnieuw correct getoond |
| G-02      | Profieloverzicht laden bij openen | Klant opent het profiel | Profieloverzicht wordt onmiddellijk getoond met actuele gegevens |
| G-03      | Navigatie tussen submodules | Klant opent achtereenvolgens alle drie de submodules | Elke submodule laadt correct; geen data van een vorige submodule lekt door |