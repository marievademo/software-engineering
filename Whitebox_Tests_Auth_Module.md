# Whitebox Tests – Authenticatie Module (`auth.c`)

## Korte beschrijving van de module (metacode)

```
MODULE auth.c

  FUNCTIE registreer(naam, email, wachtwoord):
    ALS wachtwoord NIET voldoet aan vereisten (min. 8 tekens, hoofdletter, cijfer, speciaal teken):
      RETURN foutmelding "Wachtwoord voldoet niet aan vereisten"
    ALS email BESTAAT IN systeem:
      RETURN foutmelding "E-mail bestaat al"
    maakAccount aan(naam, email, hash(wachtwoord))
    stuurVerificatieEmail(email)
    WACHT op emailVerificatie()
      ALS niet geverifieerd: stuurHerinnering()
      ALS geverifieerd: activeerAccount()
    RETURN succes

  FUNCTIE login(email, wachtwoord):
    vergelijkMetHash(wachtwoord, opgeslagen_hash)
    ALS inloggegevens INCORRECT:
      registreerMisluktePoging(email)
      ALS aantalMisluktePogingenGelijkAanVijf(email):
        blokkeerAccountTijdelijk(email)
        RETURN foutmelding "Account geblokkeerd"
      RETURN foutmelding "Ongeldige gegevens"
    token = maakJWTToken(email)
    ALS token = null:
      RETURN foutmelding "Token generatie mislukt"
    rol = bepaalGebruikersrol(email)
    SWITCH rol:
      CASE klant      → navigeer naar klantmenu
      CASE admin      → navigeer naar beheerderspaneel
      CASE keuken/bar → navigeer naar keuken/bar interface
    RETURN token, rol

END MODULE
```

---

## Legenda – Knooppuntlabels

> Deze labels worden gebruikt in de **Pad**-verwijzingen bij elke test.

| Label | Type | Beschrijving |
|-------|------|--------------|
| A | Start | Start: Gebruiker opent app |
| B | Beslissing | Heeft account? |
| C | Stap | Registratieformulier tonen |
| D | Stap | Gebruiker vult naam, e-mail en wachtwoord in |
| E | Beslissing | Wachtwoord voldoet aan vereisten? |
| F | Stap | Foutmelding: min. 8 tekens, hoofdletter, cijfer, speciaal teken |
| G | Beslissing | E-mail al in gebruik? |
| H | Stap | Foutmelding: e-mail bestaat al |
| I | Stap | Account aanmaken |
| J | Stap | Verificatie-e-mail versturen |
| K | Beslissing | E-mail geverifieerd? |
| L | Stap | Herinnering versturen |
| M | Stap | Account geactiveerd |
| N | Stap | Inlogscherm tonen |
| O | Stap | Gebruiker voert e-mail en wachtwoord in |
| P | Stap | Wachtwoord vergelijken met hash |
| Q | Beslissing | Inloggegevens correct? |
| R | Stap | Mislukte poging registreren |
| S | Beslissing | 5 mislukte pogingen? |
| T | Stap | Account tijdelijk blokkeren |
| U | Stap | Foutmelding: account geblokkeerd |
| V | Einde | Einde: probeer later opnieuw |
| W | Stap | Foutmelding: ongeldige gegevens |
| X | Stap | JWT-token aanmaken |
| Y | Stap | Gebruikersrol bepalen |
| Z | Beslissing | Rol? (Klant / Admin / Keuken/Bar) |
| AA | Stap | Naar klantmenu |
| AB | Stap | Naar beheerderspaneel |
| AC | Stap | Naar keuken/bar interface |
| AD | Einde | Einde |

---

## Whitebox Tests

### WT-01 – Registratie: wachtwoord voldoet niet aan vereisten

**Functie:** `registreer()`  
**Doel:** Verifieer dat een te zwak wachtwoord wordt geweigerd en de gebruiker opnieuw kan invoeren.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Voer wachtwoord in zonder hoofdletter (`abc123!`) | Foutmelding getoond |
| 2 | Voer wachtwoord in zonder cijfer (`Abcdefg!`) | Foutmelding getoond |
| 3 | Voer wachtwoord in zonder speciaal teken (`Abcdef12`) | Foutmelding getoond |
| 4 | Voer wachtwoord in korter dan 8 tekens (`Ab1!`) | Foutmelding getoond |
| 5 | Controleer systeemopslag | Geen account aangemaakt |

**Pad:** E -- Nee → F → D  
**Testtype:** Grensgeval / foutpad

---

### WT-02 – Registratie: e-mail al in gebruik

**Functie:** `registreer()`  
**Doel:** Verifieer dat een reeds geregistreerde e-mail wordt gedetecteerd en geweigerd.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Geef geldig wachtwoord én een al bestaand e-mailadres | Foutmelding "E-mail bestaat al" getoond |
| 2 | Controleer systeemopslag | Geen nieuw account aangemaakt |
| 3 | Controleer verificatiemail | Geen e-mail verstuurd |

**Pad:** E -- Ja → G -- Ja → H → D  
**Testtype:** Foutpad

---

### WT-03 – Registratie: normaal pad met succesvolle verificatie

**Functie:** `registreer()`  
**Doel:** Verifieer het volledige gelukkige pad van registratie tot accountactivatie.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Geef geldig wachtwoord en nieuw e-mailadres | Account aangemaakt in systeem |
| 2 | Controleer verificatie-e-mail | E-mail is verstuurd naar het opgegeven adres |
| 3 | Simuleer klik op verificatielink | Account geactiveerd |
| 4 | Controleer doorstuur | Inlogscherm getoond |

**Pad:** E -- Ja → G -- Nee → I → J → K -- Ja → M → N  
**Testtype:** Normaal pad (happy path)

---

### WT-04 – Registratie: herinnering bij niet-geverifieerde e-mail

**Functie:** `registreer()` / `stuurHerinnering()`  
**Doel:** Verifieer dat een herinnering wordt verstuurd zolang de e-mail niet geverifieerd is.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Account aangemaakt, verificatiemail verstuurd | E-mail in wachtrij |
| 2 | Simuleer dat gebruiker niet klikt binnen tijdslimiet | Herinnering verstuurd |
| 3 | Simuleer tweede herinnering | Systeem blijft herhalen totdat verificatie plaatsvindt |

**Pad:** K -- Nee → L → K (lus)  
**Testtype:** Luspad (loop coverage)

---

### WT-05 – Login: correcte inloggegevens, rol = Klant

**Functie:** `login()`  
**Doel:** Verifieer dat een klant na correcte login naar het klantmenu wordt doorverwezen met een geldig JWT-token.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Geef correct e-mail en wachtwoord van een klant | Inloggegevens correct = Ja |
| 2 | Controleer JWT-token | Token is aangemaakt en niet leeg |
| 3 | Controleer rol | Rol = `klant` |
| 4 | Controleer navigatie | Doorverwezen naar klantmenu |

**Pad:** Q -- Ja → X → Y → Z -- Klant → AA → AD  
**Testtype:** Normaal pad

---

### WT-06 – Login: correcte inloggegevens, rol = Admin

**Functie:** `login()`  
**Doel:** Verifieer dat een admin na correcte login naar het beheerderspaneel wordt doorverwezen.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Geef correct e-mail en wachtwoord van een admin | Inloggegevens correct = Ja |
| 2 | Controleer JWT-token | Token is aangemaakt en niet leeg |
| 3 | Controleer rol | Rol = `admin` |
| 4 | Controleer navigatie | Doorverwezen naar beheerderspaneel |

**Pad:** Q -- Ja → X → Y → Z -- Admin → AB → AD  
**Testtype:** Normaal pad

---

### WT-07 – Login: correcte inloggegevens, rol = Keuken/Bar

**Functie:** `login()`  
**Doel:** Verifieer dat een keuken/bar-medewerker na correcte login correct wordt doorverwezen.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Geef correct e-mail en wachtwoord van keuken/bar-gebruiker | Inloggegevens correct = Ja |
| 2 | Controleer JWT-token | Token is aangemaakt en niet leeg |
| 3 | Controleer rol | Rol = `keuken/bar` |
| 4 | Controleer navigatie | Doorverwezen naar keuken/bar interface |

**Pad:** Q -- Ja → X → Y → Z -- Keuken/Bar → AC → AD  
**Testtype:** Normaal pad

---

### WT-08 – Login: verkeerde inloggegevens (< 5 pogingen)

**Functie:** `login()`  
**Doel:** Verifieer dat een mislukte poging wordt geregistreerd en de gebruiker opnieuw kan proberen.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Geef fout wachtwoord (poging 1 t/m 4) | Foutmelding "Ongeldige gegevens" getoond |
| 2 | Controleer teller | Mislukte pogingen = huidig aantal (< 5) |
| 3 | Controleer accountstatus | Account blijft actief |
| 4 | Controleer doorstuur | Gebruiker kan opnieuw invoeren |

**Pad:** Q -- Nee → R → S -- Nee → W → O  
**Testtype:** Foutpad met terugkeer (loop)

---

### WT-09 – Login: account blokkeren na 5 mislukte pogingen

**Functie:** `login()`  
**Doel:** Verifieer dat het account tijdelijk wordt geblokkeerd na exact 5 mislukte inlogpogingen.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Simuleer 5 opeenvolgende foute wachtwoorden | Teller bereikt 5 |
| 2 | Controleer accountstatus | Account tijdelijk geblokkeerd |
| 3 | Controleer foutmelding | "Account geblokkeerd" wordt getoond |
| 4 | Controleer eindstatus | Sessie beëindigd: "Probeer later opnieuw" |
| 5 | Probeer opnieuw in te loggen terwijl geblokkeerd | Toegang geweigerd |

**Pad:** Q -- Nee → R → S -- Ja → T → U → V  
**Testtype:** Grenswaarde (5 = drempelwaarde)

---

### WT-10 – Bestaand account: inlogscherm rechtstreeks getoond

**Functie:** flow-ingang  
**Doel:** Verifieer dat een gebruiker met bestaand account het registratieproces volledig overslaat.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Open app als gebruiker met bestaand account | Registratieformulier NIET getoond |
| 2 | Controleer scherm | Inlogscherm direct getoond |

**Pad:** B -- Ja → N  
**Testtype:** Beslissingspad (branch coverage)

---

### WT-11 – Login: e-mail bestaat niet in systeem

**Functie:** `login()`  
**Doel:** Verifieer dat een niet-bestaand e-mailadres correct wordt afgehandeld zonder de pogingenteller te verhogen.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Geef e-mailadres dat niet bestaat in de databank | Login wordt geweigerd |
| 2 | Controleer pogingenteller | Teller wordt niet verhoogd voor onbekend account |
| 3 | Controleer foutmelding | "Ongeldige gegevens" getoond |
| 4 | Controleer systeemopslag | Geen nieuwe record aangemaakt |

**Pad:** O → P → Q -- Nee (e-mail niet gevonden) → W → O  
**Testtype:** Foutpad / branch coverage

---

### WT-12 – Login: JWT-token generatie mislukt

**Functie:** `login()`  
**Doel:** Verifieer dat een fout tijdens token generatie correct wordt afgehandeld en login wordt geweigerd.

| Stap | Actie | Verwacht resultaat |
|------|-------|--------------------|
| 1 | Simuleer fout in `maakJWTToken()` zodat token = null | Token generatie mislukt |
| 2 | Controleer loginresultaat | Login wordt geweigerd, gebruiker krijgt foutmelding |
| 3 | Controleer navigatie | Gebruiker blijft op inlogscherm |
| 4 | Controleer logging | Fout wordt intern gelogd in systeem |

**Pad:** Q -- Ja → X (fout) → foutpad  
**Testtype:** Exception path / robuustheid

---

## Dekkingsmatrix

| Test | Functie | Pad/Branch | Conditie getest |
|------|---------|------------|-----------------|
| WT-01 | `registreer` | E -- Nee → F → D | Zwak wachtwoord (4 varianten) |
| WT-02 | `registreer` | G -- Ja → H → D | E-mail al in gebruik |
| WT-03 | `registreer` | Volledig happy path | Normale registratie + verificatie |
| WT-04 | `registreer` | K -- Nee → L → K | Verificatielus met herinnering |
| WT-05 | `login` | Z -- Klant → AA → AD | Rol: klant |
| WT-06 | `login` | Z -- Admin → AB → AD | Rol: admin |
| WT-07 | `login` | Z -- Keuken/Bar → AC → AD | Rol: keuken/bar |
| WT-08 | `login` | S -- Nee → W → O | Mislukt < 5 pogingen |
| WT-09 | `login` | S -- Ja → T → U → V | Blokkering bij exact 5 pogingen |
| WT-10 | flow-ingang | B -- Ja → N | Bestaand account, registratie overgeslagen |
| WT-11 | `login` | O → P → Q -- Nee (onbekend e-mail) | E-mail niet gevonden |
| WT-12 | `login` | Q -- Ja → X (fout) | JWT-token generatie mislukt |
