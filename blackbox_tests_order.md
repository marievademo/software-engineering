# Blackbox Tests – Ordermodules
Gebaseerd op het activity diagram voor het bestelproces. De volgende modules worden onderscheiden: `order.c`, `kitchen.c`, `payment.c` en `user.c`.

## 1. `order.c` – Bestelling beheren
### Verantwoordelijkheden (spec)
- Bestelling opslaan in het systeem
- Bestellings-ID en tijdstip aanmaken
- Statusupdates ontvangen en doorsturen naar klantscherm
- Status bijwerken naar "Geleverd"
- Bestellingshistorie doorgeven aan `user.c`

### Blackbox Tests
| Test ID   | Invoer                                    | Verwachte uitvoer | Doel |
|---------  |--------                                   |-------------------|------|
| OC-01     | Bevestigde bestelling met ≥1 gerecht      | Bestelling aanwezig in systeem, uniek bestellings-ID teruggegeven | Opslaan werkt correct |
| OC-02     | Twee opeenvolgende bestellingen           | Twee verschillende bestellings-IDs | IDs zijn uniek |
| OC-03     | Bevestigde bestelling                     | Tijdstip van aanmaken is ingevuld en correct (niet leeg, niet in toekomst) | Tijdstip wordt correct vastgelegd |
| OC-04     |Statusupdate "In bereiding" van `kitchen.c'| Klantscherm toont "In bereiding" | Statusdoorvoer werkt |
| OC-05     | Statusupdate "Klaar" van `kitchen.c`      | Klantscherm toont "Klaar" | Statusdoorvoer werkt |
| OC-06     | Levering bevestigd                        | Status van bestelling is "Geleverd" | Statusovergang naar Geleverd werkt |
| OC-07     | Bestelling met status "Geleverd"          | Bestellingshistorie doorgestuurd naar `user.c` | Historieopslag wordt getriggerd |
| OC-08     | Lege bestelling (geen gerechten)          | Foutmelding of weigering om op te slaan | Ongeldige invoer wordt afgehandeld |

## 2. `kitchen.c` – Keukenbeheer
### Verantwoordelijkheden (spec)
- Bestelling ontvangen van `order.c` en doorsturen naar de keuken
- Status instellen op "In bereiding" wanneer keuken start
- Status instellen op "Klaar" wanneer bereiding voltooid is

### Blackbox Tests
| Test ID   | Invoer                                | Verwachte uitvoer | Doel |
|---------  |--------                               |-------------------|------|
| KC-01     | Geldige bestelling van `order.c`      | Bestelling verschijnt in keukensysteem | Doorsturing naar keuken werkt |
| KC-02     | Keuken start verwerking van bestelling| Status = "In bereiding", update gestuurd naar `order.c` | Statusovergang "In bereiding" werkt |
| KC-03     | Keuken rondt bereiding af             | Status = "Klaar", update gestuurd naar `order.c` | Statusovergang "Klaar" werkt |
| KC-04     | Bestelling met meerdere gerechten     | Alle gerechten zichtbaar in keukensysteem | Volledigheid van doorsturing |
| KC-05     | Twee gelijktijdige bestellingen       | Beide bestellingen onafhankelijk verwerkt met correcte status | Gelijktijdigheid werkt correct |
| KC-06     | Geen bestelling ontvangen             | Geen statuswijziging, geen crash | Robuustheid bij lege invoer |

## 3. `payment.c` – Betalingsbeheer
### Verantwoordelijkheden (spec)
- Totaalbedrag berekenen op basis van de bestelling
### Blackbox Tests
| Test ID   | Invoer                                            | Verwachte uitvoer | Doel |
|---------  |--------                                           |-------------------|------|
| PC-01     | Bestelling met 1 gerecht (prijs €10,00)           | Totaalbedrag = €10,00 | Basisberekening correct |
| PC-02     | Bestelling met 3 gerechten (€5, €8, €12)          | Totaalbedrag = €25,00 | Optelling van meerdere gerechten |
| PC-03     | Bestelling met aanpassing zonder meerprijsimpact  | Totaalbedrag gelijk aan basisprijs gerecht | Aanpassingen zonder kost worden genegeerd |
| PC-04     | Lege bestelling (geen gerechten)                  | Totaalbedrag = €0,00 of foutmelding | Randgeval: lege invoer |
| PC-05     | Bestelling met gerecht met prijs €0,00            | Totaalbedrag = €0,00 | Grenswaarde: nulprijs |
| PC-06     | Bestelling aangemaakt gelijktijdig met `kitchen.c`| Totaalbedrag beschikbaar zonder wachttijd op keuken | Parallelle uitvoering werkt correct |

## 4. `user.c` – Gebruikersbeheer
### Verantwoordelijkheden (spec)
- Bestellingshistorie ontvangen en opslaan per gebruiker
### Blackbox Tests
| Test ID   | Invoer                                                            | Verwachte uitvoer | Doel |
|---------  |--------                                                           |-------------------|------|
| UC-01     | Geleverde bestelling van `order.c`                                | Bestelling toegevoegd aan geschiedenis van de gebruiker | Historieopslag werkt |
| UC-02     | Twee opeenvolgende bestellingen van dezelfde gebruiker            | Beide bestellingen aanwezig in geschiedenis, chronologisch gesorteerd | Meerdere bestellingen correct opgeslagen |
| UC-03     | Bestelling van gebruiker A, dan opvragen van gebruiker B          | Geschiedenis van B bevat de bestelling van A niet | Isolatie per gebruiker gewaarborgd |
| UC-04     | Bestellingshistorie opvragen voor gebruiker zonder bestellingen   | Lege lijst teruggegeven, geen fout | Randgeval: nieuwe gebruiker |
| UC-05     | Onvolledige besteldata aangeboden (bv. ontbrekend tijdstip)       | Foutmelding of geweigerd, geen corrupte opslag | Robuustheid bij slechte invoer |