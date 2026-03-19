# 🍽️ Restaurant Orderingsapp — UI/UX Guidelines & Principles
## 1. Doel van dit document
Dit document beschrijft de richtlijnen en principes voor het ontwerpen en bouwen van de Restaurant Orderingsapp.
Het zorgt voor consistentie in design, navigatie en gebruikerservaring over alle pagina's heen.
Elk teamlid volgt deze afspraken zodat de app als één geheel aanvoelt.

## 2. Doelgroep
- **Klant**: restaurantbezoeker die rustig en eenvoudig wil bestellen (zie persona Sophie)
- **Admin**: medewerker die orders, menu en instellingen beheert
- **Keuken/Bar**: medewerker die bestellingen verwerkt en statussen bijwerkt

## 3. Algemene Ontwerpprincipes
| Principe | Uitleg |
|---|---|
| **Eenvoud** | Zo weinig mogelijk stappen om een bestelling te plaatsen |
| **Consistentie** | Zelfde kleuren, knoppen en navigatie op elke pagina |
| **Feedback** | Altijd een bevestiging tonen na een actie (bestelling, betaling, fout) |
| **Toegankelijkheid** | Grote tekst, duidelijke knoppen, voldoende contrast |
| **Foutafhandeling** | Duidelijke foutmeldingen bij mislukte acties, nooit een blanco scherm |

## 4. Kleurenpalet & Typografie
### Kleuren
| Gebruik | Kleur | HEX |
|---|---|---|
| Primaire actie (knoppen) | Donkergroen | `#2E7D32` |
| Achtergrond | Lichtgrijs | `#F5F5F5` |
| Navigatiebalk | Wit | `#FFFFFF` |
| Foutmelding | Rood | `#C62828` |
| Bevestiging / succes | Groen | `#43A047` |
| Tekst (hoofd) | Bijna zwart | `#212121` |
| Tekst (secundair) | Grijs | `#757575` |
### Typografie
- **Font**: Inter of Roboto (sans-serif, goed leesbaar op scherm)
- **Minimale tekstgrootte**: 16px voor bodytekst, 14px voor labels
- **Titels (H1)**: 28px bold
- **Subtitels (H2)**: 22px semi-bold
- **Knoppen**: 16px bold, minimum hoogte 48px (touch-vriendelijk)

## 5. Pagina-opbouw (Structuur)
Elke pagina volgt dezelfde basisstructuur:
```
┌──────────────────────────────────────┐
│              NAVIGATIEBALK           │
├──────────────────────────────────────┤
│                HEADER                │
├────────────────────┬─────────────────┤
│                    │                 │
│     HOOFDINHOUD    │    ZIJPANEEL    │
│    (2 of 3 kol.)   │   (optioneel)   │
│                    │                 │
├──────────────────────────────────────┤
│                FOOTER                │
└──────────────────────────────────────┘
```
### 5.1 Navigatiebalk
- **Positie**: vast bovenaan (sticky), zichtbaar op elke pagina
- **Bevat**:
  - Logo / naam van het restaurant (links)
  - Hoofdmenu-links: Home, Menu, Mijn Bestelling (midden of rechts)
  - Winkelmandje-icoon met teller (rechts)
  - Inloggen / account-icoon (rechts)
- **Mobiel**: hamburger-menu dat uitklapt
### 5.2 Header
- Paginatitel of welkomstboodschap
- Optioneel: een hero-afbeelding op de homepagina
- Op interne pagina's: breadcrumb-navigatie (`Home > Menu > Pasta`)
### 5.3 Hoofdinhoud
| Pagina | Kolom-indeling |
|---|---|
| Homepagina | 1 kolom (hero + uitgelichte gerechten) |
| Menuoverzicht | 3 kolommen (gerechtenkaarten) |
| Bestelling plaatsen | 2 kolommen (menu links, winkelmandje rechts) |
| Admin dashboard | 2 kolommen (menu links, data rechts) |
### 5.4 Footer
- **Bevat**:
  - Naam restaurant + copyright
  - Openingsuren
  - Contactgegevens
  - Links: Privacybeleid, Algemene Voorwaarden
  - Taalkeuzeschakelaar (NL / EN / FR)
- **Stijl**: donkere achtergrond, lichte tekst

## 6. Navigatiestructuur
```
Homepagina
├── Menu bekijken
│   ├── Gerecht detail
│   └── Filters (allergieën, categorie)
├── Bestelling plaatsen
│   ├── Winkelmandje
│   ├── Bestelling controleren
│   └── Betaling + Bevestiging
└── Inloggen / Account
    ├── Registreren
    └── Wachtwoord resetten
Admin (aparte login)
├── Dashboard
├── Orderlijst
├── Menu beheren
└── Instellingen
```

## 7. Componenten & Hergebruik
Maak voor elk herbruikbaar element **één component** aan dat je overal gebruikt:
| Component | Beschrijving |
|---|---|
| `NavBar` | Navigatiebalk (identiek op elke pagina) |
| `Footer` | Voettekst (identiek op elke pagina) |
| `GerechtenKaart` | Kaart met foto, naam, prijs en "toevoegen"-knop |
| `WinkelmandjeItem` | Rij in het winkelmandje |
| `BevestigingsBanner` | Groene banner na succesvolle actie |
| `FoutMelding` | Rode banner bij fout of mislukte actie |
| `FilterBalk` | Filterknoppen boven het menu |

## 8. Feedback & Bevestigingen (verplicht)
Elke actie van de gebruiker **moet** een reactie tonen:
| Actie | Feedback |
|---|---|
| Gerecht toegevoegd aan mandje | Korte groene toast: "Toegevoegd aan je bestelling" |
| Bestelling geplaatst | Volledige bevestigingspagina met ordernummer |
| Betaling geslaagd | Bevestigingsscherm + optie om te printen/mailen |
| Betaling mislukt | Duidelijke foutmelding + instructie om opnieuw te proberen |
| Formulier onvolledig | Inline foutmelding per veld, niet na verzending pas |

## 9. Toegankelijkheid (Accessibility)

- Minimale klikgebieden: **48x48px**
- Kleurcontrast: minimaal **4.5:1** (WCAG AA)
- Alle afbeeldingen hebben een `alt`-tekst
- Formuliervelden hebben altijd een zichtbaar label (geen placeholder als enige aanduiding)
- Navigeerbaar met toetsenbord

## 10. Wireframe-afspraken
Bespreek onderstaande punten **voor** je begint met tekenen:
- [ ] Zijn de kleuren en fonts afgesproken?
- [ ] Is de navigatiebalk identiek op alle wireframes?
- [ ] Staat het winkelmandje-icoon altijd op dezelfde positie?
- [ ] Zijn de kolomindeling per pagina afgesproken?
- [ ] Is de footer inhoud bepaald?
- [ ] Zijn bevestigings- en foutschermen ook opgenomen?
### Te maken wireframes
1. **Home Screen** — hero, uitgelichte gerechten, navigatie
2. **Hoofd Screen (Menu)** — filteropties, gerechtenkaarten, winkelmandje zijpaneel
3. *(Groep van 3)* **Bestelling & Bevestiging** — overzicht, betaalstap, bevestigingspagina

## 11. Versie & Samenwerking
- Werk met **Git** en maak voor elke feature een aparte branch
- Mapstructuur:
  /src        → broncode
  /docs       → dit document + wireframes
  /tests      → testbestanden
  /assets     → afbeeldingen, fonts, iconen
  ```
- Geef elkaar **feedback op wireframes** voor je begint te bouwen
- Gebruik **pull requests** voor code review