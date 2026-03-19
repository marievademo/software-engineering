# Software Requirements Specification (SRS)
## Restaurant Orderingsysteem
### Versie 1.0
#### Opgesteld door: Lars Vanden Broeck, Tibo Van Geertsom, Marie Van de Moortel
#### Datum: 26-02-2026

---

## Revision History

| Version | Date       | Description             | Author       |
|---------|------------|-------------------------|--------------|
| 0.1     | 10-02-2026 | Initieel concept        | Projectteam  |
| 1.0     | 26-02-2026 | Finale versie           | Projectteam  |

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Overall Description](#2-overall-description)
3. [Specific Requirements](#3-specific-requirements)
4. [Appendices](#4-appendices)
5. [Approval](#5-approval)

---

## 1. Introduction

### 1.1 Purpose

Dit document specificeert de functionele en niet-functionele vereisten van het **Restaurant Orderingsysteem**. Het is bestemd voor ontwikkelaars, testers en docenten.

### 1.2 Scope

Het systeem laat klanten toe digitaal bestellingen te plaatsen via een mobiele of web-applicatie. Restaurantpersoneel (admin en keuken/bar) beheert menu's, bestellingen en betalingen via hetzelfde platform.

### 1.3 Definitions & Abbreviations

| Term | Definitie |
|------|-----------|
| **MVP** | Minimum Viable Product |
| **FR** | Functional Requirement |
| **NFR** | Non-Functional Requirement |
| **MRC** | Monthly Recurring Charge |
| **OTC** | One Time Cost |
| **GDPR** | General Data Protection Regulation |
| **Admin** | Restaurantbeheerder |
| **Keuken/Bar** | Personeel dat bestellingen verwerkt |

### 1.4 References

- IEEE Std 830-1998 – *IEEE Recommended Practice for Software Requirements Specifications*
- ISO/IEC/IEEE 29148:2018 – *Requirements Engineering*
- User Story Map – Restaurant Orderingsysteem (Mural, februari 2026)
- Business Case – Financieel overzicht Restaurant Orderingsysteem, 2026

---

## 2. Overall Description

### 2.1 Product Perspective

Cloudgebaseerde mobiele en web-applicatie met real-time communicatie tussen klanten, admin en keuken/bar. Data wordt centraal opgeslagen en het systeem is schaalbaar naar meerdere locaties.

### 2.2 User Characteristics

| Rol | Kenmerken |
|-----|-----------|
| **Klant** | Basiskennis smartphone/browser; geen training vereist |
| **Admin** | Restaurantbeheerder met basiskennis digitale tools |
| **Keuken/Bar** | Werkt met een eenvoudig statusoverzicht op scherm |

### 2.3 Constraints

- Beschikbaar op iOS, Android en moderne webbrowsers.
- Betalingen via gecertificeerde externe provider.
- GDPR-conform voor opslag van klantgegevens.

### 2.4 Assumptions & Dependencies

- Stabiele internetverbinding aanwezig per locatie.
- Betalingsprovider biedt stabiele API-integratie.
- Personeel ontvangt onboarding van maximaal een halve dag.

---

## 3. Specific Requirements

### 3.1 Functional Requirements

#### MVP

| ID | Requirement | Verificatie |
|----|-------------|-------------|
| **FR-1** | Gebruiker kan navigeren via een navigatiemenu. | Test op alle schermen. |
| **FR-2** | Klant kan het restaurantmenu bekijken. | Menuweergave testen. |
| **FR-3** | Klant kan items toevoegen aan een winkelmandje. | Winkelmandje controleren. |
| **FR-4** | Klant kan bestelling controleren en betalen. | Betalingsflow testen. |
| **FR-5** | Admin kan menu aanpassen (toevoegen, wijzigen, verwijderen). | CRUD-operaties testen. |
| **FR-6** | Keuken/bar kan voorraad registreren. | Voorraadstatus controleren op klantpagina. |
| **FR-7** | Admin ziet een orderoverzicht per tafel. | Test met meerdere actieve bestellingen. |
| **FR-8** | Admin ontvangt en registreert betalingen. | Betaling controleren in overzicht. |
| **FR-9** | Admin kan openingsuren aanpassen. | Openingsuren controleren op klantpagina. |

#### Must Have

| ID | Requirement | Verificatie |
|----|-------------|-------------|
| **FR-10** | Klant kan registreren en inloggen. | Registratie- en inlogflow testen. |
| **FR-11** | Klant kan allergieën opslaan in profiel. | Allergieën zichtbaar bij bestelling. |
| **FR-12** | Admin kan klantaccounts beheren. | Accounts raadplegen en beheren. |
| **FR-13** | Klant kan menu filteren op categorie (bv. pasta, pizza). | Filteren testen met meerdere categorieën. |
| **FR-14** | Klant kan aanpassingen per product opgeven (bv. geen look). | Aanpassing controleren in bestelling. |
| **FR-15** | Klant ontvangt rekening via e-mail. | E-mail ontvangst verifiëren. |
| **FR-16** | Klant kan de status van zijn bestelling volgen. | Statuswijziging zichtbaar op klantscherm. |
| **FR-17** | Keuken/bar kan orderstatus aanpassen. | Synchronisatie met klantscherm testen. |
| **FR-18** | Admin en keuken/bar zien alle lopende bestellingen. | Test met meerdere simultane bestellingen. |
| **FR-19** | Klanten en admin kunnen recensies lezen en schrijven. | Recensie plaatsen en weergeven. |
| **FR-20** | Gebruiker kan de taal van de applicatie kiezen. | Taal wisselen op alle schermen. |
| **FR-21** | Admin kan financiële rapporten genereren. | Rapport controleren op correcte totalen. |

#### Nice to Have

| ID | Requirement | Verificatie |
|----|-------------|-------------|
| **FR-22** | Klant kan spaarpunten verzamelen via loyaliteitsprogramma. | Punten toekennen en saldo controleren. |
| **FR-23** | Klant kan een favorietenlijst bijhouden. | Favoriet toevoegen en terugvinden. |
| **FR-24** | Klant kan wachtwoord resetten. | Reset-e-mail ontvangen en nieuw wachtwoord instellen. |
| **FR-25** | Admin kan speciale aanbiedingen instellen. | Korting zichtbaar in menu. |
| **FR-26** | Klant kan bestelhistorie raadplegen. | Historieoverzicht per klantaccount. |
| **FR-27** | Klant kan rekening opsplitsen met anderen. | Elk deel apart betalen. |
| **FR-28** | Uitgebreid voorraadbeheer systeem. | Voorraad automatisch bijhouden na bestelling. |
| **FR-29** | Keuken/bar kan bereidingstijd per gerecht opgeven. | Bereidingstijd zichtbaar bij bestelling. |

---

### 3.2 Non-Functional Requirements

| ID | Requirement | Type | Verificatie |
|----|-------------|------|-------------|
| **NFR-1** | Pagina laadt binnen **2 seconden** bij normale verbinding. | Performance | Load test. |
| **NFR-2** | **99,5% uptime** tijdens restaurantoperaties. | Beschikbaarheid | Monitoring over 30 dagen. |
| **NFR-3** | Bruikbaar zonder training voor nieuwe klanten. | Usability | Gebruikerstest met 10 deelnemers. |
| **NFR-4** | Betalingsdata versleuteld conform **PCI DSS**. | Beveiliging | Security audit en penetratietest. |
| **NFR-5** | Voldoet aan **GDPR** voor klantgegevens. | Compliance | Juridische review. |
| **NFR-6** | Werkt op **iOS, Android, Chrome, Firefox, Safari**. | Compatibiliteit | Cross-platform test op 5 apparaten. |
| **NFR-7** | Schaalbaar naar **50+ restaurantlocaties**. | Schaalbaarheid | Load test met gesimuleerde multi-locatie. |

---

### 3.3 System Features

#### Feature: Menu & Bestelling
Klant bekijkt menu, filtert op categorie, voegt items toe en betaalt.
**FR:** FR-2, FR-3, FR-4, FR-13, FR-14 | **Prioriteit:** Hoog

#### Feature: Keuken/Bar Orderoverzicht
Personeel ontvangt real-time bestellingen en past orderstatus aan.
**FR:** FR-6, FR-7, FR-17, FR-18 | **Prioriteit:** Hoog

#### Feature: Beheerdersdashboard
Admin beheert menu, openingsuren, klantaccounts en rapporten.
**FR:** FR-5, FR-9, FR-12, FR-21 | **Prioriteit:** Hoog

#### Feature: Klantprofiel & Recensies
Klant registreert zich, beheert allergieën en schrijft recensies.
**FR:** FR-10, FR-11, FR-19, FR-20 | **Prioriteit:** Gemiddeld

---

## 4. Appendices

### Appendix A – Glossary

| Term | Definitie |
|------|-----------|
| **Winkelmandje** | Digitale verzamelplaats van geselecteerde gerechten vóór betaling. |
| **Orderlijst** | Overzicht van actieve bestellingen voor admin en keuken/bar. |
| **Status** | Toestand van een bestelling (geplaatst, in bereiding, klaar, geleverd). |
| **Backbone** | Rij van hoofdfeatures bovenaan de user story map. |

---

## 5. Approval

| Naam | Rol | Handtekening | Datum |
|------|-----|--------------|-------|
| Lars Vanden Broeck | Ontwikkelaar / Projectlid | | |
| Tibo Van Geertsom | Ontwikkelaar / Projectlid | | |
| Marie Van de Moortel | Ontwikkelaar / Projectlid | | |
| | Docent / Beoordelaar | | |