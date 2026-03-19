# Richtlijnen & Principes — Restaurant Bestelapplicatie
> Web App | Groep van 3 | Versie 1.0

---

## 1. Projectoverzicht

Deze web app is een **bestelapplicatie voor in het restaurant** waarmee klanten het menu kunnen bekijken, bestellingen plaatsen en hun bestelling volgen — rechtstreeks vanaf hun tafel, zonder voor elke stap een ober nodig te hebben.

**Hoofdschermen:**
- **Startscherm** — Landingspagina per tafel (welkom + tafelnummer + start met bestellen)
- **Menuscherm** — Blader door categorieën en gerechten, voeg toe aan mandje
- **Mandje / Bestelscherm** — Bekijk bestelling, bevestig en verstuur

---

## 2. Ontwerpprincipes

### 2.1 Consistentie Eerst
Elke pagina moet aanvoelen alsof hij bij dezelfde app hoort. Dit betekent:
- Dezelfde navbar op elk scherm
- Identieke knopstijlen, kleuren en lettergroottes door de hele app
- Iconen consistent gebruikt (hetzelfde icoon betekent altijd dezelfde actie)
- Tussenruimte en opvulling volgen een vast raster

### 2.2 Duidelijkheid Boven Slimheid
De app wordt gebruikt in een drukke restaurantomgeving, vaak door mensen die het systeem niet kennen.
- Labels moeten zelfverklarend zijn — geen vakjargon
- Belangrijke acties (Voeg toe aan mandje, Bevestig bestelling) moeten opvallend en makkelijk aan te tikken zijn
- Foutmeldingen moeten begrijpelijk zijn voor de gebruiker, niet technisch

### 2.3 Mobiel Eerst
De app wordt gebruikt op tablets of telefoons aan tafel.
- Ontwerp voor aanraking: knoppen minimaal 44px hoog
- Geen interacties die afhankelijk zijn van hover
- Leesbare lettergrootte: minimaal 16px voor bodytekst

### 2.4 Feedback & Bevestiging
Elke gebruikersactie moet een duidelijke reactie hebben:
- Item toevoegen → visuele bevestiging (badge op mandje-icoon wordt bijgewerkt)
- Bestelling plaatsen → bevestigingsscherm of melding
- Lege staten (leeg mandje, geen items in categorie) → nuttige melding, geen leeg scherm

---

## 3. Paginastructuur

Elke pagina volgt deze lay-outstructuur:

```
┌────────────────────────────────┐
│           HEADER               │  Logo + Tafelnummer
├────────────────────────────────┤
│           NAVBAR               │  Navigatielinks/tabs
├────────────────────────────────┤
│                                │
│            MAIN                │  2-kolommen lay-out (menu + mandje zijbalk)
│                                │  of volledige breedte (start/bevestiging)
│                                │
├────────────────────────────────┤
│           FOOTER               │  Contactinfo, disclaimer, restaurantnaam
└────────────────────────────────┘
```

### 3.1 Header
- Restaurantlogo (links)
- Appnaam of tagline (midden, optioneel)
- Tafelnummer duidelijk weergegeven (rechts) — bijv. "Tafel 7"
- Consistente hoogte op alle pagina's

### 3.2 Navigatiebalk
- Bevindt zich onder de header
- Links: **Menu** | **Mijn Bestelling** | **Help**
- Actieve pagina is visueel gemarkeerd (andere kleur of onderstreping)
- Mandje-icoon met badge voor aantal items, altijd zichtbaar
- Geen uitklapmenu's — houd het plat en eenvoudig

### 3.3 Hoofdinhoud
- **Startscherm:** Volledige breedte, gecentreerde inhoud (welkomstbericht, tafelbevestiging, CTA-knop)
- **Menuscherm:** 2-kolommen lay-out — menu-items links (breder), mandje-samenvatting rechts (smaller)
- **Bestel-/Mandjescherm:** Gecentreerde enkele kolom — itemlijst, hoeveelheden, totaal, bevestigingsknop
- **Bevestigingsscherm:** Volledige breedte, gecentreerd succesbericht

### 3.4 Footer
- Restaurantnaam en adres
- Korte disclaimer (bijv. "Prijzen zijn inclusief btw. Allergieën? Vraag ons personeel.")
- Optioneel: sociale media-iconen of websitelink
- Houd het minimaal — de footer mag niet concurreren met de inhoud

---

## 4. Visuele Stijlrichtlijnen

| Element | Richtlijn |
|---|---|
| **Primaire kleur** | Beslis als groep — kies er één en houd je eraan |
| **Lettertype** | Maximaal 2 lettertypen: één voor koppen, één voor bodytekst |
| **Knoppen** | Afgeronde hoeken, gevuld voor primaire acties, omlijnd voor secundaire acties |
| **Afbeeldingen** | Alle voedselafbeeldingen dezelfde beeldverhouding (bijv. 4:3), consistente kwaliteit |
| **Iconen** | Gebruik slechts één icoonsbibliotheek (bijv. Font Awesome of Material Icons) |
| **Tussenruimte** | Gebruik veelvouden van 8px voor alle marges en opvulling |

---

## 5. Inhoudsrichtlijnen

- **Menu-itemformaat:** Naam + korte beschrijving (max 2 regels) + prijs + afbeelding
- **Categorieën:** Gebruik duidelijke namen (Voorgerechten, Hoofdgerechten, Dranken, Desserts)
- **Prijzen:** Altijd weergeven met valutasymbool (€) en 2 decimalen
- **Allergeneninformatie:** Voeg een opmerking toe om het personeel te vragen — probeer niet alle allergenen op te sommen
- **Taal:** Nederlands of Engels — beslis als groep en gebruik slechts één taal

---

## 6. Wireframe Checklist

Beantwoord deze vragen als groep vóór het tekenen:

- [ ] Welk tafelnummer wordt weergegeven? Waar precies op het scherm?
- [ ] Waar is het mandje-icoon? Altijd zichtbaar?
- [ ] Hoe komt een klant van het startscherm naar het menu?
- [ ] Kunnen ze makkelijk teruggaan vanuit elk scherm?
- [ ] Wat gebeurt er nadat ze de bestelling bevestigen?
- [ ] Is de footer hetzelfde op alle 3 schermen?

---

## 7. Samenwerkingsregels

- **Bespreek vóór het tekenen** — kom de lay-out en structuur overeen voordat iemand Figma of papier opent
- **Vergelijk je pagina's** — leg alle wireframes naast elkaar en controleer op inconsistenties
- **Geef schriftelijke feedback** — gebruik opmerkingen, niet alleen mondelinge feedback
- **Één bron van waarheid** — één persoon beheert het definitieve wireframe-bestand; anderen dienen wijzigingen in via review
- **Geen last-minute solowijzigingen** — elke ontwerpwijziging moet door de groep worden goedgekeurd

---

## 8. Wat We NIET Doen

- Geen kopiëren van andere websites zonder aanpassing
- Geen inconsistente ontwerpen tussen pagina's
- Geen plaatsaanduidingstekst ("Lorem ipsum") in definitieve wireframes
- Geen ontwerpbeslissingen genomen door één persoon zonder groepsoverleg
- Geen feedbackronde overslaan vóór inlevering

---

*Laatst bijgewerkt: februari 2026 | Restaurant Bestelapplicatie Project*
