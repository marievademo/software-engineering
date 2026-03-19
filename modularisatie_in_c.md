# Modularisatie in C met Functionele Ontwikkelmethodologie

## Inleiding

Bij het ontwikkelen van een programma in C is het belangrijk om de code op een gestructureerde manier op te bouwen. Een van deze technieken hiervoor is **modularisatie**: Hierbij deel je een groot programma op in kleinere delen zodat deze beter kan beheerd worden. Als basis hiervoor wordt de **functionele ontwikkelmethodologie** gehanteerd, waarbij het programma wordt opgesplitst in functies die het moet kunnen uitvoeren.

---

## Wat is Modularisatie?

Modularisatie is het proces waarbij een programma wordt opgedeeld in afzonderlijke eenheden die elk een specifieke taak hebben/vervullen. Elke module is onafhankelijk van de andere module en communiceert via gedefinieerde interfaces.

In de programmeertaal C wordt dit concreet gedaan door:

- Het gebruik van **aparte bronbestanden** (`.c`-bestanden) per module
- Het gebruik van **header-bestanden** (`.h`-bestanden) die de publieke interface van elke module beschrijven
- Een **hoofdbestand** (`main.c`) dat de modules samenbrengt en de algemene programmaflow aanstuurt

---

## De Functionele Ontwikkelmethodologie

Bij deze methode begin je met wat het programma moet doen. Je gaat niet meteen programmeren, maar je bedenkt eerst welke taken er zijn en welke logisch bij elkaar horen.

De aanpak verloopt als volgt:

1. **Probleemanalyse**: Begrijp wat het programma moet doen als geheel.
2. **Taakidentificatie**: Bepaal welke grote deeltaken het programma bevat.
3. **Functionele decompositie**: Splits elke grote taak op in kleinere, afzonderlijke functies.
4. **Groepering**: Bundel gerelateerde functies in dezelfde module.
5. **Interface-definitie**: Bepaal wat elke module naar buiten toe aanbiedt (via het `.h`-bestand).
6. **Implementatie**: Schrijf de effectieve code per module in het `.c`-bestand.

---

## Hoe een Programma Opdelen?

### Stap 1 – Identificeer de hoofdverantwoordelijkheden

De vraag die je bij stap 1 moet stellen is: *Welke grote taken moet mijn programma uitvoeren?*

Voorbeelden van verantwoordelijkheden zijn:
- Invoer/input verwerken (lezen van gebruikersinput of bestanden)
- Berekeningen uitvoeren
- Gegevens opslaan of beheren
- Uitvoer tonen aan de gebruiker
- Foutafhandeling

### Stap 2 – Groepeer gerelateerde functies

We groeperen modules aan de hand van hoe nauw ze met elkaar samenwerken en of ze dezelfde gegevens manipuleren.

### Stap 3 – Definieer de interface van elke module

De interface van een module is eigenlijk een soort van gebruikershandleiding voor de rest van het programma. In een header-bestand (.h) zet je alleen de functies en datatypes die andere bestanden (zoals main.c) mogen gebruiken.
Hoe de code werkt (met bepaalde functies) en de interne hulpfuncties horen bij een .c bestand (source bestand).

### Stap 4 – Implementeer elke module afzonderlijk

Elke module moet normaal zijn eigen `.c`-bestand krijgen waarin de functies moeten worden uitgewerkt. Deze module moet weinig tot niks weten van functionaliteiten van andere modules.

---

## Structuur van een Gemodulariseerd C-project

Een typische mappenstructuur ziet er als volgt uit:

```
project/
│
├── main.c              ← Startpunt van het programma
│
├── invoer.h            ← Interface van de invoermodule
├── invoer.c            ← Implementatie van de invoermodule
│
├── verwerking.h        ← Interface van de verwerkingsmodule
├── verwerking.c        ← Implementatie van de verwerkingsmodule
│
├── uitvoer.h           ← Interface van de uitvoermodule
└── uitvoer.c           ← Implementatie van de uitvoermodule
```

---

## Principes van een Goede Module

Een goede module bevat normaal de volgende zaken:

- **Hoge cohesie (samenhang)**: Alle functies binnen de module horen duidelijk bij elkaar.
- **Lage koppeling**: De module is zo weinig mogelijk afhankelijk van andere modules.
- **Enkelvoudige verantwoordelijkheid**: De module voorziet meestal 1 functionaliteit en is daarvoor verantwoordelijk.
- **Verborgen implementatie**: De interne werking is afgeschermd, de andere modules zien alleen wat deze nodig hebben.
- **Herbruikbaarheid**: Een goede module kan in andere projecten worden hergebruikt zonder al te veel aanpassingen te moeten doen.

---

## Voordelen van Modularisatie

Door het opsplitsen in modules van je hoofdprogramma kan dit veel voordelen bieden:

- **Overzichtelijkheid**: Kleinere bestanden of een module zijn gemakkelijker te lezen en te begrijpen (niet alleen voor de programmeur zelf maar ook voor degene die misschien in de toekomst er aan moet werken).
- **Onderhoudbaarheid**: Fouten vinden is veel gemakkelijker doordat de code compacter is.
- **Samenwerking**: Verschillende programmeurs kunnen tegelijkertijd aan verschillende modules werken.
- **Testbaarheid**: Elke module kan afzonderlijk getest worden, wat fouten sneller opspoort.
- **Hergebruik**: Modules kunnen zonder aanpassingen in andere projecten worden ingezet (herbruikbaarheid).

---

## Veelgemaakte Fouten bij Modularisatie

- **Te grote modules**: Een module met tientallen functies heeft waarschijnlijk meerdere verantwoordelijkheden en moet verder opgesplitst worden.
- **Te kleine modules**: Te kleine modules zijn ook niet goed want dan maak je het programmeren te complex omdat je onnodig te veel code genereert dat niet nodig is.
- **wederzijdse afhankelijkheid van elkaar**: Module A die module B nodig heeft, terwijl module B module A nodig heeft, moet vermeden worden.
- **Slechte naamgeving**: MModules moeten een gepaste naam krijgen die hun functionaliteit goed beschrijft.

---

## Voorbeeld: Prompt aan AI voor Modularisatie

Om hulp te vragen aan een AI-systeem bij het opdelen van een programma, kan een prompt er als volgt uitzien:

> *"Ik heb een C-programma dat gebruikersinvoer leest, berekeningen uitvoert op de ingevoerde gegevens en de resultaten op het scherm toont. Hoe kan ik dit programma het best opdelen in modules volgens de functionele ontwikkelmethodologie? Geef een overzicht van de modules, hun verantwoordelijkheden en welke functies in elke module thuishoren."*

Een goede prompt bevat dus:
- Een korte beschrijving van wat het programma doet
- De methodologie die gehanteerd moet worden
- Een concrete vraag naar de opsplitsing en de motivering ervan

---

## Besluit

Het opsplitsen van code in modules is bealngrijk voor het bouwen van kwalitatieve software. Door in het begin goed na te denken over de structuur en specifieke functionaliteit van de code, kan je ervoor zorgen dat de code achteraf beter kan gelezen worden, een betere structuur heeft voor de code te kunnen onderhouden en fouten vroegtijdig te kunnen opsporen. De tijd die je investeert in dit ontwerp, verdien je tijdens het ontwikkelproces meer dan voldoende terug.
