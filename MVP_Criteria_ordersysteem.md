# Uitwerking User Stories: MVP Scope
Deze stories vormen de "Walking Skeleton" van de applicatie: de minimale set functionaliteiten die nodig is om een volledige bestelling van klant naar keuken te laten stromen.

## Story 1: De basis (Menu bekijken)
**Rol:** Klant  
**Feature:** Het digitale menu
### User Story
> "Als **hongerige gast** wil ik **de menukaart op mijn telefoon zien**, zodat **ik kan beslissen wat ik wil eten zonder op een fysieke kaart te wachten.**"
### Acceptance Criteria
**Weergave & structuur**
- Gerechten worden getoond in een scrolbare lijst, gegroepeerd per categorie (Voorgerecht, Hoofdgerecht, Dranken). Elke categorie heeft een zichtbare koptitel. Categorieën staan in een vaste, door de admin instelbare volgorde.
- Elk menu-item toont verplicht: naam (max. 60 tekens), prijs (€ x,xx) en ingrediëntenlijst. Bij ontbrekende data verschijnt een placeholder-tekst, nooit een lege ruimte.
**Beschikbaarheid**
- Gerechten met status 'niet leverbaar' worden volledig verborgen uit de klantweergave. Ze zijn niet aanpasbaar, niet bestelbaar en niet zichtbaar.
- Als alle items in een categorie niet leverbaar zijn, wordt de gehele categoriekop ook verborgen.
**Performance**
- De menulijst laadt binnen 2 seconden op een 4G-verbinding (≥ 20 Mbps). Meting op basis van Google Lighthouse 'Time to Interactive'.
  - Bij een laadtijd > 2 seconden wordt een zichtbare laadindicator (spinner) getoond.
  - Bij een mislukte serververbinding toont de app de melding: *"Menu kon niet geladen worden. Controleer je verbinding en probeer opnieuw."* met een **Herlaad**-knop.
**MVP-beperkingen**
- Er zijn geen zoekbalk of filters aanwezig. Scrollen is de enige navigatie. Filters worden gebouwd in Release 2 (Must Have).
- Er is geen taalwisseling mogelijk in de MVP. De menukaart wordt in één taal aangeboden.

## Story 2: De controle (Bestelling controleren)
**Rol:** Klant  
**Feature:** Winkelmandje / Order review
### User Story
> "Als **gast** wil ik **mijn gekozen items controleren en het totaalbedrag zien**, zodat **ik zeker weet dat ik geen fouten heb gemaakt voordat ik betaal.**"
### Acceptance Criteria
**Overzicht mandje**
- De gebruiker ziet een lijst van geselecteerde items met: naam, aantal, stukprijs en regelprijs (aantal × stukprijs).
- Onderaan staat het totaalbedrag inclusief BTW (21%) duidelijk weergegeven, opgesplitst als: subtotaal, BTW-bedrag en totaal.
**Mandje aanpassen**
- Per item zijn knoppen **(+)** en **(–)** aanwezig. Het minimum aantal is 1; de **(–)** knop is uitgeschakeld (grijs) bij een aantal van 1 om per ongeluk verwijderen te voorkomen. Om een item volledig te verwijderen, gebruikt de gebruiker een expliciete **Verwijder**-knop (prullenbak-icoon) naast het item.
- Na een aanpassing van hoeveelheid of verwijdering wordt het totaalbedrag direct (< 0,5 seconde) herberekend zonder paginaherlaad.
**Leeg mandje**
- Als het mandje leeg is, verschijnt de melding: *"Je mandje is leeg. Ga terug naar het menu om gerechten toe te voegen."*
- De knop **Naar betalen** is niet zichtbaar en niet aanklikbaar zolang het mandje leeg is.
**Actie & navigatie**
- Er is een prominente **Naar betalen**-knop die voldoet aan WCAG 2.1 kleurcontrastratio ≥ 4.5:1. De knop staat gefixeerd onderaan het scherm zodat hij altijd zichtbaar is, ook bij lange mandjes.
- Er is een **Terug naar menu**-link bovenaan het scherm voor gebruikers die nog iets willen toevoegen.
**MVP-beperkingen**
- Er is geen optie om speciale verzoekjes (zoals 'geen ui') toe te voegen. Klanten doen dit mondeling of dit wordt gebouwd in Release 2 (Must Have).

## Story 3: De transactie (Bestelling betalen)
**Rol:** Klant  
**Feature:** Afrekenen
### User Story
> "Als **gast** wil ik **mijn bestelling direct digitaal kunnen afrekenen**, zodat **mijn order direct naar de keuken wordt gestuurd.**"
### Acceptance Criteria
**Betaalmethoden**
- De applicatie integreert met **Mollie** als betaalprovider. Ondersteunde methoden in de MVP: iDEAL en creditcard (Visa/Mastercard).
- De betaalflow opent in een door Mollie gehoste betaalpagina (redirect). Na betaling stuurt Mollie de gebruiker terug via de callback-URL.
**Succesflow**
- Na een succesvolle Mollie-callback (status: `paid`) ontvangt de gebruiker de melding: *"Bedankt! Je bestelling is ontvangen. De keuken gaat aan de slag."* met het bestelnummer zichtbaar.
- Pas ná bevestiging van status `paid` via de **Mollie webhook** wordt de order weggeschreven in de database en doorgestuurd naar het keukenscherm. De webhook is leidend, niet de frontend-redirect. Dit voorkomt dat orders naar de keuken gaan bij een mislukte betaling of een gebruiker die de browser sluit.
**Foutafhandeling**
- Als de betaling mislukt (status: `failed` of `canceled`), keert de gebruiker terug naar het winkelmandje met de items bewaard. De melding luidt: *"Betaling mislukt. Je items zijn bewaard. Probeer opnieuw of kies een andere betaalmethode."* De orderstatus in de database blijft `abandoned` — er wordt geen order aangemaakt in de keuken.
- Als de Mollie-webhook na 5 minuten uitblijft (timeout), wordt de order gemarkeerd als `pending`. Het restaurantpersoneel krijgt een notificatie om dit handmatig te controleren. Er wordt geen order naar de keuken gestuurd.
- Bij een dubbele webhook (herstuurde callback) is de verwerking **idempotent**: een reeds verwerkte betaling leidt niet tot een dubbele order.
**Veiligheid**
- De Mollie API-sleutel wordt nooit blootgesteld aan de frontend. Alle communicatie met Mollie verloopt via de server-side backend.

## Story 4: Het beheer (Aanpassen menu)
**Rol:** Admin (Beheerder)  
**Feature:** Menu management
### User Story
> "Als **restauranteigenaar** wil ik **gerechten aan of uit kunnen zetten**, zodat **klanten geen gerechten kunnen bestellen waarvan de ingrediënten op zijn.**"
### Acceptance Criteria
**Toegang & beveiliging**
- Het admin-paneel is bereikbaar via `/admin` en vereist authenticatie. Niet-ingelogde bezoekers worden direct doorgestuurd naar de loginpagina.
- Inloggen vereist een e-mailadres en wachtwoord. Na 5 mislukte pogingen wordt het account 15 minuten geblokkeerd (rate limiting). Wachtwoorden worden opgeslagen als een bcrypt-hash (minimaal 12 rounds).
- De admin-sessie verloopt na 8 uur inactiviteit. De admin wordt dan uitgelogd en teruggestuurd naar de loginpagina.
**Toggle-functionaliteit**
- Elk gerecht heeft een aan/uit-schakelaar met labels **Beschikbaar** (groen) en **Niet beschikbaar** (grijs). De huidige status is altijd zichtbaar vóór het omschakelen.
- Na het omschakelen verschijnt een bevestigingsmelding: *"[Naam gerecht] is nu [beschikbaar / niet beschikbaar]."* De wijziging is opgeslagen als deze melding zichtbaar is.
**Real-time & concurrency**
- Een wijziging is zichtbaar voor klanten die het menu herladen binnen 30 seconden. De cache-levensduur van de menu-API is maximaal 30 seconden.
- Als twee admins tegelijk dezelfde toggle bedienen, wint de laatste write (last-write-wins). Beide admins zien na herlaad de actuele status.
**Responsive design**
- Het admin-paneel is bruikbaar op desktop (≥ 1024px) en tablet (768px–1023px). Optimalisatie voor mobiel (< 768px) is een Must Have voor Release 2.
**MVP-beperkingen**
- Het toevoegen van nieuwe gerechten, het aanpassen van prijzen of ingrediënten kan in de MVP direct in de database. Alleen de beschikbaarheidstoggle moet via de UI beschikbaar zijn.

## Story 5: De productie (Overzicht per tafel)
**Rol:** Keuken / Bar  
**Feature:** Orderscherm (Kitchen Display System)
### User Story
> "Als **kok** wil ik **nieuwe bestellingen per tafel op een scherm zien binnenkomen**, zodat **ik weet wat ik moet bereiden en voor wie.**"
### Acceptance Criteria
**Weergave bon**
- Elke bon toont in grote, goed leesbare tekst (min. 24px): tafelnummer, tijdstip van bestelling en de volledige lijst van gerechten met aantallen.
- Bons worden gesorteerd op FIFO-volgorde (First In, First Out): de oudste bestelling staat altijd linksboven. Bij gelijke tijdstip wordt gesorteerd op bestelnummer (oplopend).
**Notificatie**
- Bij elke nieuwe inkomende bestelling klinkt **verplicht** een geluidssignaal (≥ 1 seconde). Het geluid is niet uitschakelbaar via de UI om gemiste orders in drukke keukens te voorkomen.
- Aanvullend knippert de nieuwe bon 3 keer (interval 500ms) bij binnenkomst als visuele aanvulling op het geluid.
**Status & archief**
- Per bon is een knop **Gereed** aanwezig. Na klikken verdwijnt de bon van het actieve scherm en krijgt de status `completed` in de database met een timestamp.
- Als een bon per ongeluk op **Gereed** wordt geklikt, is er binnen 10 seconden een zichtbare **Ongedaan maken**-knop. Na 10 seconden is de actie definitief.
**Offline & verbinding**
- Als de internetverbinding wegvalt, toont het scherm een rode banner: *"Verbinding verbroken – bestellingen worden mogelijk gemist. Herverbinden..."* De reeds zichtbare bons blijven in beeld.
- Bij herverbinding worden automatisch alle nieuwe bestellingen geladen die tijdens de offline-periode binnenkwamen, met geluid- en knippernotificatie per gemiste bestelling.