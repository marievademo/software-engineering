# Integration Test – Volledige Bestelflow

## Scenario

Een geregistreerde klant logt in, plaatst een bestelling met een aanpassing, volgt de status op en bekijkt nadien de historiek vanuit zijn profiel.

Dit scenario test de samenwerking tussen alle modules: `auth.c`, `order.c`, `kitchen.c`, `payment.c` en `user.c`.

---

## Stap-voor-stap flow met datadoorgifte

```
┌────────────────────────────────────────────────────────────────────────┐
│  STAP 1 – Inloggen                            MODULE: auth.c           │
├────────────────────────────────────────────────────────────────────────┤
│  Input:                                                                │
│    email    = "jan@example.com"                                        │
│    password = "Welkom1!"                                               │
│                                                                        │
│  Verwerking:                                                           │
│    - Wachtwoord vergeleken met opgeslagen hash                         │
│    - JWT-token aangemaakt                                              │
│    - Gebruikersrol bepaald: "klant"                                    │
│                                                                        │
│  Output (doorgegeven aan volgende stap):                               │
│    jwt_token  = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."            │
│    klant_id   = 42                                                     │
│    rol        = "klant"                                                │
└──────────────────────────────┬─────────────────────────────────────────┘
                               │ jwt_token, klant_id
                               ▼
┌────────────────────────────────────────────────────────────────────────┐
│  STAP 2 – Profiel & allergieën ophalen        MODULE: user.c           │
├────────────────────────────────────────────────────────────────────────┤
│  Input:                                                                │
│    klant_id = 42                                                       │
│                                                                        │
│  Verwerking:                                                           │
│    - Profiel opgehaald: naam, taalvoorkeur, allergieënlijst            │
│                                                                        │
│  Output (doorgegeven aan menu.c):                                      │
│    allergieënlijst = ["gluten", "noten"]                               │
│    taalvoorkeur    = "NL"                                              │
└──────────────────────────────┬─────────────────────────────────────────┘
                               │ allergieënlijst
                               ▼
┌────────────────────────────────────────────────────────────────────────┐
│  STAP 3 – Menu filteren & bestelling samenstellen                      │
│                                               MODULE: menu.c (extern)  │
├────────────────────────────────────────────────────────────────────────┤
│  Input:                                                                │
│    allergieënlijst = ["gluten", "noten"]                               │
│                                                                        │
│  Verwerking:                                                           │
│    - Menu gefilterd op allergenen                                      │
│    - Klant selecteert gerecht + voegt aanpassing toe                   │
│                                                                        │
│  Output (doorgegeven aan order.c):                                     │
│    gerechten = [                                                       │
│      { gerecht_id: 7, naam: "Pasta", prijs: 12.50,                    │
│        aanpassing: "geen look" }                                       │
│    ]                                                                   │
└──────────────────────────────┬─────────────────────────────────────────┘
                               │ klant_id, gerechten[]
                               ▼
┌────────────────────────────────────────────────────────────────────────┐
│  STAP 4 – Bestelling opslaan                  MODULE: order.c          │
├────────────────────────────────────────────────────────────────────────┤
│  Input:                                                                │
│    klant_id  = 42                                                      │
│    gerechten = [{ gerecht_id: 7, prijs: 12.50,                         │
│                   aanpassing: "geen look" }]                           │
│                                                                        │
│  Verwerking:                                                           │
│    - Bestelling opgeslagen in databank                                 │
│    - Uniek bestellings_id aangemaakt                                   │
│    - Tijdstip vastgelegd                                               │
│                                                                        │
│  Output (doorgestuurd):                                                │
│    bestellings_id = "ORD-2026-0081"                                    │
│    tijdstip       = "2026-03-12T14:32:00"                              │
│                                                                        │
│    → naar kitchen.c  (zie stap 5)                                      │
│    → naar payment.c  (zie stap 6)                                      │
└───────────────────────────────────────────────────────────────────┬────┘
                                                                    │ bestellings_id, gerechten[]
                                                                    ▼
┌────────────────────────────┐                                  ┌─────────────────────────────────────┐
│  STAP 6 – Keuken           │                                  │  STAP 5 – Betaling berekenen        │
│  MODULE: kitchen.c         │                                  │  MODULE: payment.c                  │
├────────────────────────────┤                                  ├─────────────────────────────────────┤
│  Input:                    │<--bestellings_id,                |                                     |
|                            |   gerechten[], betaling_status---│  Input:                             │
│    bestellings_id =        │                                  │    gerechten = [{ prijs: 12.50 }]   │
│      "ORD-2026-0081"       │                                  │                                     │
│    gerechten[] (incl.      │                                  │  Verwerking:                        │
│      aanpassing)           │                                  │    - Prijzen opgeteld               │
│                            │                                  │                                     │
│  Verwerking:               │                                  │  Output:                            │
│    - Bestelling zichtbaar  │                                  │    totaalbedrag = €12.50            │
│      in keukensysteem      │                                  │    betaling_status = "succes"       │
│    - Status → In bereiding │                                  └─────────────────────────────────────┘
│    - Status → Klaar        │
│                            │
│  Output → order.c:         │
│    status = "IN_BEREIDING" │
│    status = "KLAAR"        │
└────────────┬───────────────┘
             │ bestellings_id, nieuwe_status
             ▼
┌────────────────────────────────────────────────────────────────────────┐
│  STAP 7 – Statusupdates verwerken             MODULE: order.c          │
├────────────────────────────────────────────────────────────────────────┤
│  Input:                                                                │
│    bestellings_id = "ORD-2026-0081"                                    │
│    status         = "IN_BEREIDING" → daarna "KLAAR"                   │
│                                                                        │
│  Verwerking:                                                           │
│    - Status bijgewerkt in databank                                     │
│    - Klantscherm geüpdatet                                             │
│                                                                        │
│  Output (zichtbaar voor klant):                                        │
│    klantscherm toont: "In bereiding" → "Klaar"                         │
└──────────────────────────────┬─────────────────────────────────────────┘
                               │ bevestiging levering
                               ▼
┌────────────────────────────────────────────────────────────────────────┐
│  STAP 8 – Levering & afsluiting               MODULE: order.c          │
├────────────────────────────────────────────────────────────────────────┤
│  Input:                                                                │
│    bestellings_id = "ORD-2026-0081"                                    │
│    status         = "GELEVERD"                                         │
│                                                                        │
│  Verwerking:                                                           │
│    - Status → Geleverd opgeslagen                                      │
│    - Historiekoppeling getriggerd                                      │
│                                                                        │
│  Output → user.c:                                                      │
│    bestellings_id = "ORD-2026-0081"                                    │
│    klant_id       = 42                                                 │
└──────────────────────────────┬─────────────────────────────────────────┘
                               │ bestellings_id, klant_id
                               ▼
┌────────────────────────────────────────────────────────────────────────┐
│  STAP 9 – Bestellingshistorie opslaan         MODULE: user.c           │
├────────────────────────────────────────────────────────────────────────┤
│  Input:                                                                │
│    klant_id       = 42                                                 │
│    bestellings_id = "ORD-2026-0081"                                    │
│                                                                        │
│  Verwerking:                                                           │
│    - Bestelling gekoppeld aan klantprofiel                             │
│    - Historierecord opgeslagen in databank                             │
│                                                                        │
│  Output:                                                               │
│    historierecord = {                                                  │
│      klant_id:       42,                                               │
│      bestellings_id: "ORD-2026-0081",                                  │
│      tijdstip:       "2026-03-12T14:32:00",                            │
│      gerechten:      [{ naam: "Pasta", aanpassing: "geen look" }],     │
│      totaalbedrag:   12.50,                                            │
│      status:         "GELEVERD"                                        │
│    }                                                                   │
└────────────────────────────────────────────────────────────────────────┘
```

---

## Verwachte eindresultaten

| Controle                                  | Verwacht |
|----------                                 |----------|
| JWT-token aangemaakt bij inloggen         | Geldig token voor klant_id 42 |
| Allergieënlijst doorgestuurd naar menu    |  Menu gefilterd op gluten en noten |
| Bestelling opgeslagen met uniek ID        | `ORD-2026-0081` aanwezig in databank |
| Tijdstip correct vastgelegd               | Niet leeg, niet in toekomst |
| kitchen.c ontving bestelling              | Bestelling zichtbaar in keukensysteem |
| payment.c berekende totaalbedrag          | €12.50 |
| Klantscherm toonde statuswijzigingen      | "In bereiding" → "Klaar" |
| Status bijgewerkt naar "Geleverd"         | In databank opgeslagen |
| Historierecord aangemaakt in user.c       | Gekoppeld aan klant_id 42 |

---

## Gelinkte blackbox- en whitebox-tests

| Stap  | Module    | Blackbox test     | Whitebox test |
|------ |--------   |---------------    |---------------|
| 1     | auth.c    | AUTH-09           | WT-05 |
| 2     | user.c    | USR-06            | WT-07 |
| 3     | order.c   | ORD-01, ORD-02    | WT-01 |
| 4     | order.c   | ORD-05            | WT-01 |
| 5     | kitchen.c | ORD-06, ORD-07    | WT-03, WT-04 |
| 6     | payment.c | ORD-10            | WT-08 |
| 7     | order.c   | ORD-06, ORD-07    | WT-03, WT-04 |
| 8     | order.c   | ORD-08            | WT-06 |
| 9     | user.c    | ORD-09            | WT-06 |
