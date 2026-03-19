sequenceDiagram
    actor Klant
    participant Browser
    participant Server
    participant Auth
    participant DB
 
    Note over Klant,DB: Flow 1 - Authenticatie
 
    Klant->>Browser: Vult e-mail en wachtwoord in
    Browser->>Server: HTTP POST /login
    Server->>Auth: Geef inloggegevens door
    Auth->>DB: Zoek gebruiker op via e-mail
    DB-->>Auth: Gebruikersrecord + gehashte wachtwoord + rol
    Auth->>Auth: Vergelijk wachtwoord met hash
    alt Inloggegevens correct
        Auth-->>Server: Gebruikers-ID + rol
        Server-->>Browser: HTTP 200 OK + JWT-token + rol
        Browser-->>Klant: Doorsturen naar correcte pagina
    else Inloggegevens fout
        Auth-->>Server: Authenticatie mislukt
        Server-->>Browser: HTTP 401 Unauthorized
        Browser-->>Klant: Foutmelding tonen
    end


 sequenceDiagram
    actor Klant
    participant Browser
    participant Server
    participant Menu
    participant DB

    Note over Klant,DB: Flow 2 - Menu ophalen

    Klant->>Browser: Opent menupagina
    Browser->>Server: HTTP GET /menu
    Server->>Menu: Haal menulijst op
    Menu->>DB: SELECT alle gerechten
    DB-->>Menu: Lijst van gerechten
    Menu-->>Server: Gefilterde menulijst
    Server-->>Browser: HTTP 200 OK + gerechten
    Browser-->>Klant: Menu weergeven

    sequenceDiagram
    actor Klant
    participant Browser
    participant Server
    participant Auth
    participant Order
    participant Kitchen
    participant DB

    Note over Klant,DB: Flow 3 - Bestelling plaatsen

    Klant->>Browser: Bevestigt winkelmandje
    Browser->>Server: HTTP POST /order + JWT-token + items + tafelID
    Server->>Auth: Valideer JWT-token
    Auth-->>Server: Token geldig
    Server->>Order: Verwerk nieuwe bestelling
    Order->>DB: INSERT bestelling met status geplaatst
    DB-->>Order: BestellingsID 456
    Order->>Kitchen: Stuur bestelling door
    Kitchen->>DB: UPDATE status naar in bereiding
    Order-->>Server: Bestelling aangemaakt
    Server-->>Browser: HTTP 201 Created + bestellingsID 456
    Browser-->>Klant: Bevestigingspagina tonen

    sequenceDiagram
    actor Klant
    participant Browser
    participant Server
    participant Auth
    participant Order
    participant DB
 
    Note over Klant,DB: Flow 4 - Orderstatus opvolgen
 
    Klant->>Browser: Opent statuspagina
    Browser->>Server: HTTP GET /order/456/status + JWT-token
    Server->>Auth: Valideer JWT-token
    Auth-->>Server: Token geldig
    Server->>Order: Haal status op voor bestelling 456
    Order->>DB: SELECT status WHERE id is 456
    DB-->>Order: status is in bereiding
    Order-->>Server: Status opgehaald
    Server-->>Browser: HTTP 200 OK + status in bereiding
    Browser-->>Klant: Uw bestelling is in bereiding

    sequenceDiagram
    actor Klant
    participant Browser
    participant Server
    participant Auth
    participant Payment
    participant DB
 
    Note over Klant,DB: Flow 5 - Betaling verwerken
 
    Klant->>Browser: Klikt op Betalen
    Browser->>Server: HTTP POST /payment + JWT-token + bestellingsID 456
    Server->>Auth: Valideer JWT-token
    Auth-->>Server: Token geldig
    Server->>Payment: Bereken totaalbedrag
    Payment->>DB: SELECT items en prijzen WHERE bestellingsID is 456
    DB-->>Payment: Items en prijzen
    Payment-->>Server: Totaalbedrag 34.50 euro
    Server-->>Browser: HTTP 200 OK + totaal + redirect naar betalingsprovider
    Browser-->>Klant: Betalingspagina tonen
 

 sequenceDiagram
    actor Klant
    participant Browser
    participant Server
    participant Payment
    participant DB

    Note over Klant,DB: Flow 6 - Betalingsbevestiging

    Browser->>Server: HTTP POST /payment/confirm + bestellingsID 456 + status geslaagd
    Server->>Payment: Registreer betaling
    Payment->>DB: UPDATE betaald is true WHERE id is 456
    DB-->>Payment: OK
    Payment-->>Server: Betaling geregistreerd
    Server-->>Browser: HTTP 200 OK + betaling geslaagd
    Browser-->>Klant: Bedankpagina en rekening per e-mail