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
| 1.1     | 05-03-2026 | Security requirements uitgebreid | Projectteam  |

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

#### Security (Functioneel)

| ID | Requirement | Verificatie |
|----|-------------|-------------|
| **FR-S1** | Wachtwoorden worden opgeslagen als gehashte waarden (bcrypt of Argon2, minimaal 12 ronden). | Code review + penetratietest. |
| **FR-S2** | Wachtwoorden vereisen minimaal 8 tekens, 1 hoofdletter, 1 cijfer en 1 speciaal teken. | Validatietest bij registratie. |
| **FR-S3** | Account wordt tijdelijk geblokkeerd na 5 opeenvolgende mislukte inlogpogingen (rate limiting). | Bruteforce simulatietest. |
| **FR-S4** | Authenticatie verloopt via JWT-tokens met een maximale geldigheid van 60 minuten; refresh tokens hebben een maximale geldigheid van 7 dagen. | Token expiry test. |
| **FR-S5** | Uitloggen invalideert het actieve token onmiddellijk (server-side token blacklist of token versioning). | Test: token gebruiken na uitloggen → HTTP 401. |
| **FR-S6** | Toegang tot admin- en keuken/bar-functies is strikt beperkt op basis van gebruikersrol (RBAC). Ongeautoriseerde toegang geeft HTTP 403. | Roletest per endpoint. |
| **FR-S7** | Alle API-inputs worden gevalideerd en gesanitized om SQL-injectie en XSS te voorkomen. | Penetratietest (OWASP Top 10). |
| **FR-S8** | Betalingstransacties verlopen uitsluitend via een gecertificeerde externe betalingsprovider (bv. Stripe, Mollie). Kaartgegevens worden nooit opgeslagen op eigen servers. | PCI DSS audit. |
| **FR-S9** | Sessies verlopen automatisch na 30 minuten inactiviteit. | Inactiviteitstest. |
| **FR-S10** | Wachtwoordreset verloopt via een tijdgebonden eenmalige link (geldig maximaal 15 minuten), verstuurd naar het geregistreerde e-mailadres. | Reset-flow testen incl. verlopen link. |
| **FR-S11** | Klantgegevens zijn enkel toegankelijk voor de eigenaar van het account en bevoegde admins (data isolation). | Test: klant A kan gegevens van klant B niet raadplegen. |
| **FR-S12** | Alle communicatie tussen client en server verloopt uitsluitend via HTTPS (TLS 1.2 of hoger). HTTP-verzoeken worden automatisch omgeleid naar HTTPS. | SSL Labs test + redirect verificatie. |

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
| **NFR-8** | Alle data-at-rest (database, backups) is versleuteld met minimaal **AES-256**. | Beveiliging | Infrastructuur audit. |
| **NFR-9** | Alle data-in-transit is versleuteld via **TLS 1.2 of hoger**; oudere protocollen (SSL, TLS 1.0/1.1) zijn uitgeschakeld. | Beveiliging | SSL Labs scan (score A of hoger). |
| **NFR-10** | Beveiligingslogs (inlogpogingen, rolwijzigingen, betalingen) worden minimaal **12 maanden** bewaard en zijn auditeerbaar. | Logging & Audit | Log review + retentietest. |
| **NFR-11** | Het systeem detecteert en logt verdachte activiteit (bv. brute force, ongebruikelijke API-calls) en genereert alerts. | Beveiliging | Penetratietest + alerttest. |
| **NFR-12** | Persoonlijke klantgegevens worden geanonimiseerd of verwijderd op verzoek conform **GDPR Art. 17** (recht op vergetelheid), binnen 30 dagen. | Compliance | Verwijderprocedure testen. |
| **NFR-13** | Softwareafhankelijkheden worden maandelijks gescand op bekende kwetsbaarheden (CVEs) via geautomatiseerde tooling (bv. Dependabot, Snyk). | Beveiliging | Dependency scan rapportage. |
| **NFR-14** | API-endpoints zijn beveiligd tegen **rate limiting**: maximaal 100 verzoeken per minuut per IP-adres voor publieke endpoints. | Beveiliging | Load- en abustest. |
| **NFR-15** | De applicatie implementeert correcte **HTTP security headers**: `Content-Security-Policy`, `X-Frame-Options`, `X-Content-Type-Options`, `Strict-Transport-Security`. | Beveiliging | Security headers scan (bv. securityheaders.com). |

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

#### Feature: Authenticatie & Autorisatie
Veilig inloggen, rolgebaseerde toegangscontrole en sessiebeheer voor alle gebruikerstypen.
**FR:** FR-S1, FR-S2, FR-S3, FR-S4, FR-S5, FR-S6, FR-S9, FR-S10 | **Prioriteit:** Hoog

#### Feature: Databescherming & Compliance
Versleuteling van gegevens, GDPR-naleving en veilige betalingsverwerking.
**FR:** FR-S8, FR-S11, FR-S12 | **NFR:** NFR-4, NFR-5, NFR-8, NFR-9, NFR-12 | **Prioriteit:** Hoog

#### Feature: Beveiligingsmonitoring & Hardening
Logging van beveiligingsgebeurtenissen, detectie van aanvallen en technische hardeningmaatregelen.
**NFR:** NFR-10, NFR-11, NFR-13, NFR-14, NFR-15 | **Prioriteit:** Gemiddeld

---

## 4. Appendices

### Appendix A – Glossary

| Term | Definitie |
|------|-----------|
| **Winkelmandje** | Digitale verzamelplaats van geselecteerde gerechten vóór betaling. |
| **Orderlijst** | Overzicht van actieve bestellingen voor admin en keuken/bar. |
| **Status** | Toestand van een bestelling (geplaatst, in bereiding, klaar, geleverd). |
| **Backbone** | Rij van hoofdfeatures bovenaan de user story map. |
| **JWT** | JSON Web Token – gestandaardiseerd token voor veilige authenticatie. |
| **RBAC** | Role-Based Access Control – toegangscontrole op basis van gebruikersrol. |
| **HTTPS/TLS** | Versleuteld communicatieprotocol voor data-in-transit. |
| **AES-256** | Advanced Encryption Standard met 256-bit sleutel voor data-at-rest. |
| **PCI DSS** | Payment Card Industry Data Security Standard – beveiligingsnorm voor betalingsdata. |
| **OWASP Top 10** | Lijst van de tien meest kritieke webapplicatie-beveiligingsrisico's (Open Web Application Security Project). |
| **Rate Limiting** | Beperking van het aantal verzoeken per tijdseenheid om misbruik te voorkomen. |
| **CVE** | Common Vulnerabilities and Exposures – standaard identificatiesysteem voor bekende kwetsbaarheden. |

---

## 5. Approval

| Naam | Rol | Handtekening | Datum |
|------|-----|--------------|-------|
| Lars Vanden Broeck | Ontwikkelaar / Projectlid | | |
| Tibo Van Geertsom | Ontwikkelaar / Projectlid | | |
| Marie Van de Moortel | Ontwikkelaar / Projectlid | | |
| | Docent / Beoordelaar | | |