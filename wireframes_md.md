# Wireframes – Restaurant Orderingssysteem

## Overzicht

Dit document beschrijft de drie wireframes die ontworpen zijn voor de mobiele app van het restaurant orderingssysteem. De wireframes zijn low-fidelity en focussen op structuur en functionaliteit, niet op visueel design. De app is gericht op de **klant** die zelf zijn bestelling plaatst aan tafel.

---

## 01 – Home pagina

### Doel
De home pagina is het startpunt van de app. Het geeft de klant een snel overzicht van het restaurant en leidt hem naar het menu.

### Componenten

**Header**
De header toont de naam van het restaurant en de huidige tafel. Rechts staat een icoontje voor het winkelmandje met een badge die het aantal items aangeeft.

**Hero banner**
Een grote afbeelding van het restaurant of een featured gerecht, met daarop een call-to-action knop "Bekijk het menu →". Dit nodigt de klant direct uit om te beginnen met bestellen.

**Categorie-pills**
Een horizontale rij knoppen (Pizza, Pasta, Salade, Drank, ...) waarmee de klant snel naar een categorie kan navigeren. De actieve categorie is donker gemarkeerd.

**Productkaarten**
Een grid van 2 kolommen met productkaarten. Elke kaart bevat een afbeelding, productnaam, korte beschrijving, prijs en een + knop om het item direct toe te voegen aan het winkelmandje.

**Bottom navigatie**
Een vaste navigatiebalk onderaan met vier items: Home, Menu, Mandje en Profiel. Het mandje toont een badge met het aantal items.

### Navigatiemogelijkheden
- Klik op "Bekijk het menu →" → gaat naar het menu scherm
- Klik op een categorie-pill → filtert de kaarten op die categorie
- Klik op + knop → voegt item toe aan winkelmandje
- Klik op mandje icoon of tab → gaat naar winkelmandje

---

## 02 – Menu overzicht

### Doel
Het menuscherm geeft een volledig overzicht van alle beschikbare gerechten. De klant kan zoeken, filteren op categorie en items toevoegen aan zijn winkelmandje.

### Componenten

**Header**
Bevat een terugknop (links) en de paginatitel "Menu". Rechts staat een icoon voor extra opties.

**Zoekbalk**
Een afgeronde zoekbalk met een zoekicoontje en placeholder tekst. Naast de zoekbalk staat een filterknop waarmee de klant kan filteren op allergenen of andere criteria (complexe filters zoals pasta, pizza, ...).

**Categorie-tabs**
Een horizontale scrollbare rij met categorietabs: Alle, Pizza, Pasta, Salade, Drank. De actieve tab is donker gemarkeerd. Door te klikken op een tab worden enkel de items van die categorie getoond.

**Sectietitel**
Een kleine label boven de lijst die de actieve categorie aangeeft.

**Menulijst**
Een verticale lijst van menu-items. Elk item bestaat uit:
- Een afbeelding (links)
- Naam van het gerecht
- Korte beschrijving of allergenen-info
- Prijs
- Een ronde + knop om het item toe te voegen

**Mandje-balk (sticky bottom)**
Een vaste balk onderaan die het aantal geselecteerde items en de totaalprijs toont, met een knop "Bekijk mandje →" die naar het winkelmandjeScherm leidt.

### Navigatiemogelijkheden
- Zoekbalk gebruiken → filtert de lijst in real-time
- Filterknop → opent filterpaneel (allergenen, prijs, ...)
- Klik op categorie-tab → toont enkel items van die categorie
- Klik op + → voegt item toe, badge en totaalprijs updaten
- Klik op "Bekijk mandje →" → gaat naar winkelmandje

---

## 03 – Winkelmandje

### Doel
Het winkelmandjeScherm geeft de klant een overzicht van zijn bestelling voor hij deze definitief plaatst. Hij kan hoeveelheden aanpassen, een opmerking toevoegen en de totaalprijs controleren.

### Componenten

**Header**
Bevat een terugknop (links) en de paginatitel "Winkelmandje". Rechts staat een icoon om het mandje leeg te maken.

**Tafelindicator**
Een balk direct onder de header die de tafelnummer toont (bv. T4) samen met een label "Tafel 4". Rechts staat een "Wijzigen" knop waarmee de klant het tafelnummer kan aanpassen indien nodig.

**Lijst van items**
Een verticale lijst van toegevoegde items. Elk item bevat:
- Een kleine afbeelding
- Naam van het gerecht
- Prijs per stuk
- Hoeveelheid aanpassen via − en + knoppen

**Opmerkingenveld**
Een tekstinvoerveld waar de klant een opmerking kan achterlaten voor de keuken (bv. "zonder uien", "extra saus").

**Prijsoverzicht**
Een samenvattingssectie met:
- Subtotaal
- Servicekosten
- Een horizontale scheidingslijn
- **Totaal** (vet weergegeven)

**Bestelknop**
Een grote, prominente knop "Bestelling plaatsen" onderaan het scherm. Daaronder staat een kleine informatieve tekst (bv. over betaling).

### Navigatiemogelijkheden
- Klik op − / + → past hoeveelheid aan, prijsoverzicht update
- Klik op "Wijzigen" bij tafelindicator → laat tafelnummer aanpassen
- Tekst invoeren in opmerkingenveld → wordt meegestuurd met de bestelling
- Klik op "Bestelling plaatsen" → bevestigt de bestelling en gaat naar betalingsscherm

---

## Algemene opmerkingen

- Alle wireframes zijn ontworpen voor **mobiel** (375x812px, iPhone-formaat).
- De wireframes zijn **low-fidelity**: grijstinten, placeholder-afbeeldingen en blokken voor tekst.
- De klant bestelt **zelf via de app** aan tafel. Er is geen tussenkomst van de ober bij het bestellen.
- De tafelnummer is altijd zichtbaar zodat de ober en keuken weten voor welke tafel de bestelling is.
