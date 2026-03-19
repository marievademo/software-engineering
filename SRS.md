***

# Software Requirements Specification (SRS)

## Project: Restaurant Orderingssysteem
### Versie 1.0
**Datum:** 26-02-2026
**Status:** Concept

---

## Revisiegeschiedenis

| Versie | Datum | Omschrijving | Auteur |
| :--- | :--- | :--- | :--- |
| 0.1 | 26-02-2026 | Eerste concept o.b.v. Mural brainstorm | Lars Vanden Broeck, Tibo Van Geertsom en Marie Van de moortel |

---

## Inhoudsopgave
1. [Inleiding](#1-inleiding)
2. [Algemene Beschrijving](#2-algemene-beschrijving)
3. [Functionele Eisen (Functional Requirements)](#3-functionele-eisen)
4. [Niet-Functionele Eisen (Non-Functional Requirements)](#4-niet-functionele-eisen)
5. [Systeemkenmerken per Rol](#5-systeemkenmerken-per-rol)

---

## 1. Inleiding

### 1.1 Doel
Het doel van dit document is het definiëren van de software-eisen voor het **Restaurant Orderingssysteem**. Dit systeem stelt klanten in staat om digitaal bestellingen te plaatsen en te betalen, biedt het keukenpersoneel inzicht in orders per tafel, en geeft beheerders tools om het menu en de financiën te beheren.

### 1.2 Scope
Het systeem bestaat uit drie hoofdinterfaces:
1.  **Klantinterface:** Voor het bekijken van het menu, samenstellen van een bestelling en betaling.
2.  **Admininterface:** Voor beheer van het menu, openingstijden en rapportages.
3.  **Keuken/Bar-interface:** Voor het ontvangen en monitoren van bestellingen per tafel.

De focus voor de eerste release ligt op de **MVP (Minimum Viable Product)** functionaliteiten, gevolgd door de *Must Have* en *Nice to Have* fases.

### 1.3 Definities en Afkortingen
| Term | Definitie |
| :--- | :--- |
| **MVP** | Minimum Viable Product (de minimale set functies om live te gaan) |
| **FR** | Functional Requirement (wat het systeem moet doen) |
| **NFR** | Non-Functional Requirement (hoe het systeem zich moet gedragen) |
| **CRUD** | Create, Read, Update, Delete |

---

## 2. Algemene Beschrijving

### 2.1 Productperspectief
Het systeem functioneert als een web-based of native applicatie die gebruikt wordt binnen het restaurant (aan tafel) en door het personeel. Het systeem vereist een internetverbinding voor betalingsverwerking en synchronisatie tussen de zaal en de keuken.

### 2.2 Gebruikerskenmerken
*   **Klant:** Geen training vereist. Moet intuïtief door het menu kunnen navigeren.
*   **Admin (Eigenaar/Manager):** Basiskennis van computergebruik voor back-office taken.
*   **Keuken/Bar Personeel:** Moet snel en efficiënt statussen kunnen wijzigen op een touchscreen in een hectische omgeving.

---

## 3. Functionele Eisen

De eisen zijn gecategoriseerd op basis van de prioriteiten uit de Mural: **MVP**, **Must Have**, en **Nice to Have**.

### 3.1 MVP (Minimum Viable Product)
*Deze functies zijn essentieel voor de eerste lancering.*

| ID | Rol | Omschrijving | Rationale |
| :--- | :--- | :--- | :--- |
| **FR-MVP-01** | Klant | Het systeem toont een navigatiemenu en laat de klant het menu bekijken. | Basisinteractie. |
| **FR-MVP-02** | Klant | De klant kan producten toevoegen aan het winkelmandje ("Winkelmandje vullen"). | Nodig om een order te starten. |
| **FR-MVP-03** | Klant | De klant kan de bestelling controleren en bevestigen. | Fouten voorkomen voor verzending. |
| **FR-MVP-04** | Klant | De klant kan de bestelling direct betalen via het systeem. | Afronding transactie. |
| **FR-MVP-05** | Admin | De beheerder kan het menu aanpassen (producten toevoegen/wijzigen). | Menu up-to-date houden. |
| **FR-MVP-06** | Admin | De beheerder kan betalingen ontvangen en verifiëren. | Financiële controle. |
| **FR-MVP-07** | Admin | De beheerder kan de openingsuren aanpassen. | Klantinformatie. |
| **FR-MVP-08** | Keuken | Het systeem toont een overzicht van bestellingen gegroepeerd per tafel. | Efficiënte bereiding en uitservering. |

### 3.2 Must Have (Release 2.0)
*Deze functies zijn noodzakelijk voor een complete gebruikservaring.*

| ID | Rol | Omschrijving | Rationale |
| :--- | :--- | :--- | :--- |
| **FR-MH-01** | Klant | Klanten kunnen zich registreren en inloggen. | Klantbinding en dataopslag. |
| **FR-MH-02** | Klant | Het menu ondersteunt complexe filters (bijv. filteren op 'pasta', 'pizza'). | Vindbaarheid producten verhogen. |
| **FR-MH-03** | Klant | Het systeem toont allergeneninformatie bij producten. | Wettelijke verplichting & gezondheid. |
| **FR-MH-04** | Klant | Klanten kunnen producten personaliseren (bijv. "geen look"). | Servicegerichtheid. |
| **FR-MH-05** | Klant | De klant kan de statuswijziging van hun bestelling zien (bijv. "In de maak"). | Verwachtingsmanagement. |
| **FR-MH-06** | Klant | De klant kan de rekening per e-mail ontvangen. | Administratie voor klant. |
| **FR-MH-07** | Klant | De klant kan een taal kiezen voor de interface. | Toerisme/toegankelijkheid. |
| **FR-MH-08** | Klant | Klanten kunnen recensies lezen en schrijven. | Social proof en feedback. |
| **FR-MH-09** | Admin | De beheerder kan klantaccounts beheren. | CRM en support. |
| **FR-MH-10** | Admin | De beheerder kan financiële rapporten genereren. | Inzicht in omzet. |
| **FR-MH-11** | Keuken | Het systeem biedt functionaliteit om voorraad aan te geven (bijv. "op"). | Teleurstelling bij bestellen voorkomen. |
| **FR-MH-12** | Keuken | Keukenpersoneel kan alle orders monitoren en de status wijzigen. | Workflow management. |

### 3.3 Nice to Have (Toekomstige Releases)
*Wenselijke functies die extra waarde toevoegen maar niet kritiek zijn.*

| ID | Rol | Omschrijving | Rationale |
| :--- | :--- | :--- | :--- |
| **FR-NTH-01** | Klant | Inloggen om spaarpunten te verzamelen (Loyalty systeem). | Klantenbinding. |
| **FR-NTH-02** | Klant | De klant kan zijn bestelhistorie inzien. | Gemak bij herhaalbestellingen. |
| **FR-NTH-03** | Klant | De klant kan een favorietenlijst aanmaken. | Snel bestellen. |
| **FR-NTH-04** | Klant | De klant kan de rekening opsplitsen (split bill). | Groepsgemak. |
| **FR-NTH-05** | Klant | De klant kan zijn wachtwoord resetten. | Self-service accountbeheer. |
| **FR-NTH-06** | Admin | De beheerder kan speciale aanbiedingen instellen. | Marketing/Upselling. |
| **FR-NTH-07** | Admin | Historie inzien van verkopen/activiteit. | Trendanalyse. |
| **FR-NTH-08** | Keuken | Een uitgebreid voorraadsysteem (automatische aftrek). | Automatisering inkoop. |
| **FR-NTH-09** | Keuken | Weergave van bereidingstijd per gerecht/order. | Planning optimaliseren. |

---

## 4. Niet-Functionele Eisen

*Deze eisen beschrijven de kwaliteitscriteria waaraan het systeem moet voldoen.*

| ID | Categorie | Omschrijving | Verificatie |
| :--- | :--- | :--- | :--- |
| **NFR-01** | **Performance** | Het menu en de afbeeldingen moeten binnen **2 seconden** geladen zijn op een mobiel 4G-netwerk. | Load test. |
| **NFR-02** | **Usability** | Een nieuwe klant moet een bestelling kunnen plaatsen in maximaal **5 klikken/stappen** na het openen van het menu. | User testing. |
| **NFR-03** | **Betrouwbaarheid** | Het ordermanagement (Keuken scherm) moet real-time synchroniseren met een vertraging van maximaal **1 seconde**. | Netwerk test. |
| **NFR-04** | **Security** | Betalingstransacties moeten voldoen aan de PCI-DSS standaarden. Geen creditcardgegevens worden lokaal opgeslagen. | Security audit. |
| **NFR-05** | **Privacy** | Klantgegevens (account, e-mail) moeten worden opgeslagen conform de GDPR/AVG wetgeving. | Compliance check. |
| **NFR-06** | **Beschikbaarheid** | Het systeem moet een uptime garanderen van **99.9%** tijdens openingstijden van het restaurant. | Monitoring logs. |
| **NFR-07** | **Interface** | De interface voor de keuken moet leesbaar zijn vanaf een afstand van 1 meter (groot contrast/lettertype). | Design review. |

---

## 5. Systeemkenmerken per Rol

### Feature: Orderproces (Klant)
**Beschrijving:** De kern van het systeem. De klant kiest items, personaliseert deze (geen look), plaatst ze in het mandje en rekent af.
*   **Betrokken FR's:** FR-MVP-01 t/m 04, FR-MH-02, FR-MH-04.

### Feature: Order Management (Keuken/Bar)
**Beschrijving:** Het ontvangen van orders op een digitaal scherm, gegroepeerd per tafel, en het updaten van de status (van "Besteld" naar "Bereid" naar "Uitgeserveerd").
*   **Betrokken FR's:** FR-MVP-08, FR-MH-12.

### Feature: Beheer & Rapportage (Admin)
**Beschrijving:** De "achterkant" van het restaurant. Hier wordt bepaald wat er verkocht wordt, wat de prijzen zijn en worden de dagtotalen ingezien.
*   **Betrokken FR's:** FR-MVP-05, FR-MVP-06, FR-MH-10.

---
*Einde van document*