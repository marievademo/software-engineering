# Blackbox Tests – Authenticatiemodule
Gebaseerd op het activiteitendiagram voor de authenticatieflow.

## Module 1: Registratie – Wachtwoordvalidatie
**Beschrijving:** Het systeem controleert of het ingevoerde wachtwoord voldoet aan de eisen (min. 8 tekens, hoofdletter, cijfer, speciaal teken).
| Test ID   | Input         | Verwachte output |
|---------  |-------        |-----------------|
| WV-01     | `wachtwoord`  | Foutmelding: voldoet niet aan vereisten (geen hoofdletter, cijfer, speciaal teken)|
| WV-02     | `Wacht1!`     | Foutmelding: minimum 8 tekens vereist (7 tekens)|
| WV-03     | `wacht1!woord`| Foutmelding: hoofdletter vereist (geen hoofdletter)|
| WV-04     | `Wachtwoord!` | Foutmelding: cijfer vereist (geen cijfer)|
| WV-05     | `Wachtwoord1` | Foutmelding: speciaal teken vereist (geen speciaal teken)|
| WV-06     | `Wacht1!woord`| Validatie geslaagd, verdergaan naar e-mailcheck (voldoet aan alle eisen)|

## Module 2: Registratie – E-mailduplicaatcontrole
**Beschrijving:** Het systeem controleert of het opgegeven e-mailadres al in gebruik is.
| Test ID   | Input                                             | Verwachte output |
|---------  |-------                                            |-----------------|
| EC-01     | E-mailadres dat al bestaat in de database         | Foutmelding: e-mail bestaat al |
| EC-02     | E-mailadres dat nog niet bestaat in de database   | Account wordt aangemaakt |
| EC-03     | E-mailadres met afwijkend hoofdlettergebruik (bv. `Test@mail.com` vs `test@mail.com`) | Foutmelding: e-mail bestaat al (case-insensitive check) |

## Module 3: Registratie – Account aanmaken & verificatie-e-mail
**Beschrijving:** Na succesvolle validatie wordt een account aangemaakt en een verificatie-e-mail verstuurd.
| Test ID   | Scenario                                                      | Verwachte output |
|---------  |----------                                                     |-----------------|
| AV-01     | Geldige registratiegegevens ingevoerd                         | Account aangemaakt, verificatie-e-mail ontvangen |
| AV-02     | Gebruiker klikt verificatielink in de e-mail                  | Account geactiveerd, inlogscherm getoond |
| AV-03     | Gebruiker klikt verificatielink niet(e-mail niet geverifieerd)| Herinneringsmail verstuurd |
| AV-04     | Verificatielink verlopen of ongeldig                          | Foutmelding of nieuwe verificatielink aangeboden |

## Module 4: Inloggen – Referentievalidatie
**Beschrijving:** Het systeem vergelijkt het ingevoerde wachtwoord met de opgeslagen hash.
| Test ID   | Input                                              | Verwachte output |
|---------  |-------                                             |-----------------|
| IV-01     | Correct e-mailadres + correct wachtwoord           | Inloggen geslaagd, JWT-token aangemaakt |
| IV-02     | Correct e-mailadres + fout wachtwoord              | Foutmelding: ongeldige gegevens |
| IV-03     | Niet-bestaand e-mailadres + willekeurig wachtwoord | Foutmelding: ongeldige gegevens |
| IV-04     | Leeg e-mailveld of leeg wachtwoordveld             | Foutmelding: velden mogen niet leeg zijn |

## Module 5: Inloggen – Accountblokkering bij mislukte pogingen
**Beschrijving:** Na 5 opeenvolgende mislukte inlogpogingen wordt het account tijdelijk geblokkeerd.
| Test ID | Scenario                                | Verwachte output |
|---------|----------                               |-----------------|
| AB-01   | 1 mislukte inlogpoging                  | Foutmelding: ongeldige gegevens, account niet geblokkeerd |
| AB-02   | 4 mislukte inlogpogingen                | Foutmelding: ongeldige gegevens, account nog actief |
| AB-03   | 5e mislukte inlogpoging                 | Account tijdelijk geblokkeerd, melding: probeer later opnieuw |
| AB-04   | Inlogpoging op geblokkeerd account      | Foutmelding: account geblokkeerd, flow eindigt |
| AB-05   | Succesvolle login na 4 mislukte pogingen| Inloggen geslaagd, teller gereset |

## Module 6: Autorisatie – JWT-token & rolbepaling
**Beschrijving:** Na succesvolle login wordt een JWT-token aangemaakt en de gebruikersrol bepaald. Op basis van de rol wordt de gebruiker naar de juiste interface geleid.
| Test ID | Gebruikersrol                       | Verwachte output |
|---------|--------------                       |-----------------|
| RB-01   | Klant                               | Doorgestuurd naar klantmenu |
| RB-02   | Admin                               | Doorgestuurd naar beheerderspaneel |
| RB-03   | Keuken/Bar                          | Doorgestuurd naar keuken/bar interface |
| RB-04   | Gebruiker zonder toegewezen rol     | Foutmelding of standaard fallback-gedrag |
| RB-05   | JWT-token aanwezig maar verlopen    | Gebruiker uitgelogd / teruggestuurd naar inlogscherm |
| RB-06   | JWT-token gemanipuleerd (ongeldig)  | Toegang geweigerd, foutmelding |