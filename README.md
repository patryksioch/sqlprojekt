sqlprojekt
Diagram Encji i Związków (ERD) - System Obsługi Hotelu
```mermaid
erDiagram
    GOSC {
        int id_goscia PK
        varchar imie
        varchar nazwisko
        varchar email UK
        varchar telefon
        varchar adres
    }

    TYP_POKOJU {
        int id_typu PK
        varchar nazwa
        varchar standard
        int maks_liczba_osob
        decimal cena_za_dobe
    }

    POKOJ {
        int id_pokoju PK
        int id_typu FK
        varchar numer UK
        int pietro
        varchar status "stan techniczny pokoju"
    }

    WYPOSAZENIE {
        int id_wyposazenia PK
        varchar nazwa
        varchar opis
    }

    WYPOSAZENIE_POKOJU {
        int id_pokoju PK, FK
        int id_wyposazenia PK, FK
        int ilosc
        varchar stan
    }

    PRACOWNIK {
        int id_pracownika PK
        varchar imie
        varchar nazwisko
        varchar stanowisko
        varchar email UK
        varchar telefon
    }

    ZGLOSZENIE_USTERKI {
        int id_zgloszenia PK
        int id_pokoju FK
        int id_pracownika FK
        timestamp data_zgloszenia
        varchar typ_zgloszenia
        text opis
        varchar status
    }

    REZERWACJA {
        int id_rezerwacji PK
        int id_goscia FK
        int id_pokoju FK
        int id_pracownika FK
        date data_od
        date data_do
        int liczba_osob
        decimal cena_za_dobe "stawka ustalona przy rezerwacji"
        varchar status
    }

    POBYT {
        int id_pobytu PK
        int id_rezerwacji FK, UK
        timestamp data_zameldowania
        timestamp data_wymeldowania "NULL do wymeldowania"
    }

    USLUGA {
        int id_uslugi PK
        varchar nazwa
        text opis
        decimal cena_aktualna
    }

    ZAMOWIENIE_USLUGI {
        int id_zamowienia PK
        int id_pobytu FK
        int id_uslugi FK
        timestamp data_realizacji "NULL do realizacji"
        int ilosc
        decimal cena_jednostkowa
    }

    PLATNOSC {
        int id_platnosci PK
        int id_rezerwacji FK
        decimal kwota
        timestamp data_platnosci
        varchar metoda_platnosci
    }

    FAKTURA {
        int id_faktury PK
        int id_pobytu FK, UK
        varchar numer_faktury UK
        date data_wystawienia
        decimal kwota_calkowita
    }

    GOSC ||..o{ REZERWACJA : "dokonuje"
    TYP_POKOJU ||..o{ POKOJ : "klasyfikuje"
    POKOJ ||..o{ REZERWACJA : "jest rezerwowany w"
    POKOJ ||--o{ WYPOSAZENIE_POKOJU : "posiada"
    WYPOSAZENIE ||--o{ WYPOSAZENIE_POKOJU : "wchodzi w skład"
    POKOJ ||..o{ ZGLOSZENIE_USTERKI : "ma zgłoszenia"
    PRACOWNIK ||..o{ ZGLOSZENIE_USTERKI : "obsługuje"
    PRACOWNIK ||..o{ REZERWACJA : "rejestruje"
    REZERWACJA ||..o| POBYT : "prowadzi do"
    REZERWACJA ||..o{ PLATNOSC : "ma wpłaty"
    POBYT ||..o{ ZAMOWIENIE_USLUGI : "obejmuje"
    USLUGA ||..o{ ZAMOWIENIE_USLUGI : "jest zamawiana w"
    POBYT ||..o| FAKTURA : "jest dokumentowany przez"
```
Założenia modelu
Rezerwacja dotyczy jednego pokoju i jednego gościa rezerwującego. Może prowadzić do jednego pobytu, a pobyt może mieć jedną fakturę.
Płatności są przypisane do rezerwacji, dzięki czemu można zapisać zaliczkę przed zameldowaniem. Zamówienia usług składa się podczas pobytu i przypisuje do tego pobytu.
Każdą rezerwację rejestruje pracownik. Każde zgłoszenie usterki jest przypisane do pracownika.
Jeden wiersz `WYPOSAZENIE_POKOJU` opisuje łączną liczbę sztuk danego rodzaju wyposażenia w pokoju. Para `(id_pokoju, id_wyposazenia)` tworzy jeden klucz główny.
Reguły do wymuszenia w bazie SQL
Każdy `FK` widoczny w diagramie jest obowiązkowy (`NOT NULL`). `POBYT.id_rezerwacji` i `FAKTURA.id_pobytu` wymagają także `UNIQUE`.
`data_od < data_do`. Dwie aktywne rezerwacje jednego pokoju nie mogą się nakładać, również przy równoczesnym zapisie. Termin obejmuje dzień przyjazdu i kończy się przed dniem wyjazdu (`[data_od, data_do)`). Dostępność zależy od terminu oraz stanu technicznego pokoju.
`liczba_osob` musi wynosić co najmniej 1 i mieścić się w limicie typu pokoju. Ilości wyposażenia i zamawianych usług muszą być dodatnie.
Gdy podano `data_wymeldowania`, musi ona być późniejsza niż `data_zameldowania`. Stawkę za dobę i cenę jednostkową usługi zapisuje się odpowiednio w chwili rezerwacji i zamówienia.
