# Gebruiksvriendelijkheidsspecificatie — Restaurant Bestelapplicatie
> Web App | Groep van 3 | Versie 1.0  
> Gebaseerd op: Richtlijnen & Principes v1.0 + Wireframes (Home, Menu, Winkelmandje)

---

## Doel

Dit document definieert **testbare criteria** om te verifiëren dat de geïmplementeerde web app voldoet aan de richtlijnen en wireframe-ontwerpen. Elk vereiste heeft een duidelijke slaag/zak-conditie zodat het tijdens het testen gecontroleerd kan worden.

---

## 1. Consistentievereisten

| ID | Vereiste | Hoe te testen | Slaagcriterium |
|----|----------|---------------|----------------|
| C-01 | Elk scherm heeft dezelfde header (logo + tafelnummer) | Open alle 3 schermen en vergelijk de header visueel | Header is consistent over alle schermen |
| C-02 | De onderste navigatiebalk is identiek op alle schermen | Vergelijk de navbar op Home, Menu, Mandje | Dezelfde 4 iconen (Home, Menu, Mandje, Profiel), zelfde volgorde, zelfde stijl |
| C-03 | De actieve pagina is visueel gemarkeerd in de navbar | Navigeer naar elke pagina | Actief tabblad is duidelijk te onderscheiden van inactieve tabbladen |
| C-04 | Alle knoppen gebruiken dezelfde stijl (afgerond, gevuld/omlijnd) | Controleer de + knop, Bestel en Bekijk mandje knoppen | Geen enkele knop wijkt af van de vastgestelde stijl |
| C-05 | Er wordt slechts één icoonsbibliotheek gebruikt in de hele app | Controleer alle iconen op alle schermen | Alle iconen komen uit dezelfde bibliotheek (bijv. allemaal Font Awesome) |
| C-06 | Het lettertype is consistent op alle pagina's | Controleer koppen en bodytekst op alle schermen | Maximaal 2 lettertypen, consequent gebruikt |

---

## 2. Layoutvereisten

| ID | Vereiste | Hoe te testen | Slaagcriterium |
|----|----------|---------------|----------------|
| L-01 | Het startscherm heeft een volledige breedte gecentreerde lay-out met hero banner | Open het startscherm | Hero banner beslaat de volledige breedte, inhoud is gecentreerd |
| L-02 | Het menuscherm heeft categorie-pills/tabs bovenaan | Open het menuscherm | Categorie-pills (Alle, Pizza, Pasta, Salade, etc.) zijn zichtbaar en functioneel |
| L-03 | Het menuscherm toont een productgrid met + knoppen | Open het menuscherm | Elk item heeft een zichtbare + knop om aan het mandje toe te voegen |
| L-04 | Het winkelmandje toont tafelnummer, itemlijst, hoeveelheidsbediening en bestelknop | Open het mandjescherm | Alle 4 elementen aanwezig en zichtbaar zonder scrollen (op een standaard tablet) |
| L-05 | De footer is aanwezig en identiek op alle schermen | Scroll naar de onderkant van elk scherm | Footer bevat restaurantnaam, adres en disclaimer op elke pagina |
| L-06 | De "Bekijk mandje" call-to-action is zichtbaar op het menuscherm | Open het menuscherm | Mandjebalk/knop is zichtbaar zonder te scrollen |

---

## 3. Navigatievereisten

| ID | Vereiste | Hoe te testen | Slaagcriterium |
|----|----------|---------------|----------------|
| N-01 | De klant kan in één tik van Home naar Menu gaan | Tik op "Bekijk menu →" op het startscherm | Menuscherm opent onmiddellijk |
| N-02 | De klant kan het Mandje bereiken vanuit het Menu in één tik | Tik op "Bekijk mandje →" op het menuscherm | Mandjescherm opent onmiddellijk |
| N-03 | De klant kan navigeren tussen alle hoofdschermen via de onderste navbar | Tik op elk icoon in de navbar | Het juiste scherm opent voor elk icoon |
| N-04 | De klant kan terugkeren naar een vorig scherm zonder de mandje-inhoud te verliezen | Navigeer weg van het mandje en keer terug | Mandje-items zijn nog aanwezig bij terugkeer |
| N-05 | Het tafelnummer kan worden gewijzigd vanuit het mandjescherm | Tik op "Wijzigen" naast het tafelnummer (T4) | Tafelnummer kan worden bijgewerkt |

---

## 4. Gebruiksvriendelijkheid & Duidelijkheidsvereisten

| ID | Vereiste | Hoe te testen | Slaagcriterium |
|----|----------|---------------|----------------|
| U-01 | Alle labels zijn zelfverklarend zonder voorafgaande uitleg | Vraag een testgebruiker die de app niet kent om een bestelling te plaatsen | Gebruiker voltooit bestelling zonder om hulp te vragen |
| U-02 | Een leeg mandje toont een nuttige melding, geen leeg scherm | Open het mandje zonder items toegevoegd | Een melding zoals "Je mandje is leeg" wordt weergegeven |
| U-03 | Een item toevoegen geeft visuele feedback | Tik op + bij een menu-item | Mandje-badge of teller wordt onmiddellijk bijgewerkt |
| U-04 | Het plaatsen van een bestelling toont een bevestiging | Tik op "Bestelling plaatsen" in het mandje | Een bevestigingsscherm of melding wordt getoond |
| U-05 | Foutmeldingen zijn begrijpelijk voor de gebruiker | Veroorzaak een fout (bijv. bestelling met 0 items) | Fout is geschreven in gewone taal, geen technische codes |
| U-06 | Prijzen worden altijd getoond met €-symbool en 2 decimalen | Controleer alle menu-items en mandjetotalen | Geen enkele prijs zonder € of met onjuiste decimalen |

---

## 5. Mobiel / Aanraakvereisten

| ID | Vereiste | Hoe te testen | Slaagcriterium |
|----|----------|---------------|----------------|
| M-01 | Alle tikdoelen (knoppen, iconen, + bediening) zijn minimaal 44px hoog | Controleer elementh hoogte in browser DevTools | Geen enkel interactief element kleiner dan 44px |
| M-02 | Geen enkele interactie vereist een hover-staat | Test op een aanraakschermapparaat | Alle functionaliteit werkt zonder hover |
| M-03 | Bodytekst is minimaal 16px | Controleer lettergroottes in DevTools | Geen bodytekst kleiner dan 16px |
| M-04 | Hoeveelheidsbediening (– 1 +) is gemakkelijk aan te tikken zonder per ongeluk tikken | Test op een fysiek apparaat | Gebruiker kan hoeveelheid aanpassen zonder onbedoelde tikken |

---

## 6. Prestatiedoelen

Dit zijn de **kwantitatieve prestatiecriteria** voor de gebruikersinterface:

| ID | Maatstaf | Doelwaarde | Meetmethode |
|----|----------|------------|-------------|
| P-01 | **Laadtijd pagina** (startscherm) | ≤ 2 seconden op standaard wifi | Browser DevTools → Netwerktabblad → DOMContentLoaded |
| P-02 | **Laadtijd pagina** (menuscherm, inclusief afbeeldingen) | ≤ 3 seconden op standaard wifi | Browser DevTools → Load event |
| P-03 | **Tijd tot interactief** (gebruiker kan op een menu-item tikken) | ≤ 3 seconden na het laden van de pagina | Lighthouse → Time to Interactive maatstaf |
| P-04 | **Reactietijd mandje-update** (na tikken op +) | ≤ 200ms visuele feedback | Handmatige stopwatch of browser performance profiler |
| P-05 | **Reactie na het plaatsen van bestelling** (na tikken op "Bestelling plaatsen") | ≤ 1 seconde naar bevestigingsscherm | Handmatige meting van tik tot bevestiging zichtbaar |
| P-06 | **Largest Contentful Paint (LCP)** | ≤ 2,5 seconden | Lighthouse audit |
| P-07 | **Cumulative Layout Shift (CLS)** | Score ≤ 0,1 (geen springende elementen) | Lighthouse audit |
| P-08 | **App werkt op schermbreedtes van 360px tot 1200px** | Geen layoutproblemen | Verklein browservenster en test op 360px, 768px, 1200px |

---

## 7. Inhoudsvereisten

| ID | Vereiste | Hoe te testen | Slaagcriterium |
|----|----------|---------------|----------------|
| CT-01 | Alle menu-items hebben: naam, korte beschrijving (max 2 regels), prijs, afbeelding | Controleer elk item op het menuscherm | Geen enkel item mist een van de 4 elementen |
| CT-02 | Geen plaatsaanduidingstekst ("Lorem ipsum") verschijnt ergens | Lees alle tekst op alle schermen | Nul gevallen van plaatsaanduidingstekst |
| CT-03 | Allergenenmelding is aanwezig | Controleer footer of menuscherm | Tekst zoals "Allergieën? Vraag ons personeel." is zichtbaar |
| CT-04 | Consistente taal wordt overal gebruikt (NL of EN, niet gemengd) | Lees alle tekst op alle schermen | Geen taalmenging gevonden |

---

## 8. Testsamenvattingschecklist

Gebruik deze checklist tijdens een definitieve reviewsessie vóór inlevering:

- [ ] Alle C-0x consistentiecontroles geslaagd
- [ ] Alle L-0x layoutcontroles geslaagd
- [ ] Alle N-0x navigatiecontroles geslaagd
- [ ] Alle U-0x gebruiksvriendelijkheidscontroles geslaagd
- [ ] Alle M-0x mobiel/aanraakcontroles geslaagd
- [ ] Alle P-0x prestatiedoelen gehaald (voer Lighthouse audit uit)
- [ ] Alle CT-0x inhoudscontroles geslaagd
- [ ] Getest op minimaal één echt mobiel apparaat of tablet
- [ ] Alle 3 wireframeschermen geïmplementeerd en komen overeen met wireframe-lay-out
- [ ] Feedback van alle groepsleden verwerkt

---

*Laatst bijgewerkt: februari 2026 | Restaurant Bestelapplicatie Project*
