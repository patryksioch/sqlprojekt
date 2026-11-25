sqlprojekt
Diagram encji i związków (ER) – system obsługi hotelu
Model koncepcyjny obejmuje 13 encji i 13 związków.
```mermaid
erDiagram
    GOSC {
        W email "ID"
        W imie
        W nazwisko
        O telefon
        O adres
    }

    TYP_POKOJU {
        W nazwa "część ID"
        W standard "część ID"
        W maks_liczba_osob
        W cena_katalogowa_za_dobe
    }

    POKOJ {
        W numer_pokoju "ID"
        W pietro
        W stan_techniczny
    }

    WYPOSAZENIE {
        W nazwa "ID"
        O opis
    }

    WYPOSAZENIE_POKOJU {
        W liczba_sztuk
        W stan_wyposazenia
    }

    PRACOWNIK {
        W email "ID"
        W imie
        W nazwisko
        W stanowisko
        O telefon
    }

    ZGLOSZENIE_USTERKI {
        W id_zgloszenia "ID systemowy"
        W data_zgloszenia
        W typ_zgloszenia
        W opis
        W status_zgloszenia
    }

    REZERWACJA {
        W id_rezerwacji "ID systemowy"
        W planowany_przyjazd
        W planowany_wyjazd
        W liczba_osob
        W ustalona_cena_za_dobe
        W status_rezerwacji
    }

    POBYT {
        W data_zameldowania
        O data_wymeldowania
    }

    USLUGA {
        W nazwa "ID"
        O opis
        W cena_katalogowa
    }

    ZAMOWIENIE_USLUGI {
        W id_zamowienia "ID systemowy"
        W ilosc
        W cena_jednostkowa
        O data_realizacji
    }

    PLATNOSC {
        W id_platnosci "ID systemowy"
        W kwota
        W data_platnosci
        W metoda_platnosci
    }

    FAKTURA {
        W numer_faktury "ID"
        W data_wystawienia
        W kwota_calkowita
    }

    GOSC ||..o{ REZERWACJA : "składa / jest składana przez"
    TYP_POKOJU ||..o{ POKOJ : "klasyfikuje / należy do typu"
    POKOJ ||..o{ REZERWACJA : "jest objęty rezerwacją / dotyczy pokoju"
    POKOJ ||--o{ WYPOSAZENIE_POKOJU : "ma wyposażenie / dotyczy pokoju"
    WYPOSAZENIE ||--o{ WYPOSAZENIE_POKOJU : "jest wykazywane w / dotyczy wyposażenia"
    POKOJ ||..o{ ZGLOSZENIE_USTERKI : "ma zgłoszenia / dotyczy pokoju"
    PRACOWNIK ||..o{ ZGLOSZENIE_USTERKI : "obsługuje / jest obsługiwane przez"
    PRACOWNIK ||..o{ REZERWACJA : "rejestruje / jest rejestrowana przez"
    REZERWACJA ||--o| POBYT : "prowadzi do / wynika z"
    REZERWACJA ||..o{ PLATNOSC : "ma wpłaty / dotyczy rezerwacji"
    POBYT ||..o{ ZAMOWIENIE_USLUGI : "obejmuje / jest składane w ramach"
    USLUGA ||..o{ ZAMOWIENIE_USLUGI : "jest zamawiana w / dotyczy usługi"
    POBYT ||..o| FAKTURA : "jest dokumentowany / dokumentuje"
```
Oznaczenia
`W` oznacza atrybut wymagany, a `O` atrybut opcjonalny. `ID` wskazuje identyfikator. Dwa atrybuty oznaczone jako `część ID` tworzą wspólnie jeden identyfikator.
Linia ciągła oznacza związek identyfikujący. Pobyt jest identyfikowany przez związek z rezerwacją, a wyposażenie pokoju przez parę związków z pokojem i wyposażeniem.
Etykieta każdego związku zawiera dwie nazwy oddzielone ukośnikiem. Pierwszą czyta się od encji wymienionej jako pierwsza w kodzie Mermaid, a drugą od encji po przeciwnej stronie związku.
Założenia modelu
E-mail jest wymagany i unikatowy osobno wśród gości i pracowników. Nazwy usług i wyposażenia są unikatowe w swoich katalogach. Typ pokoju identyfikuje para nazwa + standard.
Rezerwacja dotyczy jednego pokoju i może prowadzić do najwyżej jednego pobytu. Pobyt może mieć najwyżej jedną fakturę.
Pracownik rejestruje każdą rezerwację. Zgłoszenie usterki ma przypisanego pracownika od chwili rejestracji.
Zaliczka może wpłynąć przed zameldowaniem. Usługi zamawia się podczas pobytu. Daty wymeldowania i realizacji usługi mogą początkowo być puste.
Aktywne rezerwacje tego samego pokoju nie mogą się nakładać. Kolejny gość może przyjechać w dniu wyjazdu poprzedniego. Stawka za dobę i cena jednostkowa usługi są zapisywane w chwili rezerwacji lub zamówienia.
