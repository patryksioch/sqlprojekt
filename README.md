# sqlprojekt

## Diagram encji i związków (ER) – system obsługi hotelu

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

### Oznaczenia

- W oznacza atrybut wymagany, a O atrybut opcjonalny. ID wskazuje identyfikator. Dwa atrybuty oznaczone jako część ID tworzą wspólnie jeden identyfikator.
- Linia ciągła oznacza związek identyfikujący. Pobyt jest identyfikowany przez związek z rezerwacją, a wyposażenie pokoju przez parę związków z pokojem i wyposażeniem.
- Przy każdym związku podano obie nazwy. Pierwszą czyta się od lewej encji w kodzie, drugą w odwrotnym kierunku. Rozwijany widok poniżej umieszcza każdą nazwę przy jej końcu związku.

<details>
<summary>Diagram z osobnymi etykietami przy obu końcach związków</summary>

W tym widoku romb i dwa odcinki oznaczają jeden związek. Krotność przy encji określa, ile jej wystąpień może przypadać na jedno wystąpienie po drugiej stronie. Zielone linie oznaczają związki identyfikujące.

mermaid
---
config:
  flowchart:
    nodeSpacing: 28
    rankSpacing: 30
    curve: linear
---
flowchart TB
    accTitle: Diagram koncepcyjny ER systemu obsługi hotelu
    accDescr: 13 encji i 13 związków. Każdy romb łączy dwa nazwane końce jednego związku.

    TYP_POKOJU["<b>Typ pokoju</b><br/>Nazwa · W · ID<br/>Standard · W · ID<br/>Maks. liczba osób · W<br/>Cena katalogowa za dobę · W"]
    POKOJ["<b>Pokój</b><br/>Numer pokoju · W · ID<br/>Piętro · W<br/>Stan techniczny · W"]
    WYPOSAZENIE["<b>Wyposażenie</b><br/>Nazwa · W · ID<br/>Opis · O"]
    WYPOSAZENIE_POKOJU["<b>Wyposażenie pokoju</b><br/>Liczba sztuk · W<br/>Stan wyposażenia · W<br/><b>ID przez oba związki</b>"]
    GOSC["<b>Gość</b><br/>E-mail · W · ID<br/>Imię · W<br/>Nazwisko · W<br/>Telefon · O<br/>Adres · O"]
    PRACOWNIK["<b>Pracownik</b><br/>E-mail · W · ID<br/>Imię · W<br/>Nazwisko · W<br/>Stanowisko · W<br/>Telefon · O"]
    USTERKA["<b>Zgłoszenie usterki</b><br/>ID zgłoszenia · W · ID<br/>Data zgłoszenia · W<br/>Typ zgłoszenia · W<br/>Opis · W<br/>Status zgłoszenia · W"]
    REZERWACJA["<b>Rezerwacja</b><br/>ID rezerwacji · W · ID<br/>Planowany przyjazd · W<br/>Planowany wyjazd · W<br/>Liczba osób · W<br/>Ustalona cena za dobę · W<br/>Status rezerwacji · W"]
    PLATNOSC["<b>Płatność</b><br/>ID płatności · W · ID<br/>Kwota · W<br/>Data płatności · W<br/>Metoda płatności · W"]
    POBYT["<b>Pobyt</b><br/>Data zameldowania · W<br/>Data wymeldowania · O<br/><b>ID przez związek z rezerwacją</b>"]
    FAKTURA["<b>Faktura</b><br/>Numer faktury · W · ID<br/>Data wystawienia · W<br/>Kwota całkowita · W"]
    ZAMOWIENIE["<b>Zamówienie usługi</b><br/>ID zamówienia · W · ID<br/>Ilość · W<br/>Cena jednostkowa · W<br/>Data realizacji · O"]
    USLUGA["<b>Usługa</b><br/>Nazwa · W · ID<br/>Opis · O<br/>Cena katalogowa · W"]

    TYP_POKOJU ---|"1 · klasyfikuje"| R_TYP{"·"} ---|"0..N · należy do typu"| POKOJ
    POKOJ ---|"1 · ma wyposażenie"| R_POK_WYP{"·"} ---|"0..N · dotyczy pokoju"| WYPOSAZENIE_POKOJU
    WYPOSAZENIE ---|"1 · jest wykazywane w"| R_WYP{"·"} ---|"0..N · dotyczy wyposażenia"| WYPOSAZENIE_POKOJU
    POKOJ ---|"1 · jest objęty rezerwacją"| R_POK_REZ{"·"} ---|"0..N · dotyczy pokoju"| REZERWACJA
    POKOJ ---|"1 · ma zgłoszenia"| R_POK_UST{"·"} ---|"0..N · dotyczy pokoju"| USTERKA
    PRACOWNIK ---|"1 · obsługuje"| R_PRAC_UST{"·"} ---|"0..N · jest obsługiwane przez"| USTERKA
    PRACOWNIK ---|"1 · rejestruje"| R_PRAC_REZ{"·"} ---|"0..N · jest rejestrowana przez"| REZERWACJA
    GOSC ---|"1 · składa"| R_GOSC{"·"} ---|"0..N · jest składana przez"| REZERWACJA
    REZERWACJA ---|"1 · ma wpłaty"| R_PLAT{"·"} ---|"0..N · dotyczy rezerwacji"| PLATNOSC
    REZERWACJA ---|"1 · prowadzi do"| R_POBYT{"·"} ---|"0..1 · wynika z"| POBYT
    POBYT ---|"1 · jest dokumentowany"| R_FAKTURA{"·"} ---|"0..1 · dokumentuje"| FAKTURA
    POBYT ---|"1 · obejmuje"| R_ZAM{"·"} ---|"0..N · jest składane w ramach"| ZAMOWIENIE
    USLUGA ---|"1 · jest zamawiana w"| R_USLUGA{"·"} ---|"0..N · dotyczy usługi"| ZAMOWIENIE

    classDef encja fill:#f0f5fc,stroke:#597192,color:#142847
    classDef zwiazek fill:#edf3fa,stroke:#597192,color:#597192
    classDef identyfikujacy fill:#dff4ef,stroke:#0a645e,color:#0a645e
    class TYP_POKOJU,POKOJ,WYPOSAZENIE,WYPOSAZENIE_POKOJU,GOSC,PRACOWNIK,USTERKA,REZERWACJA,PLATNOSC,POBYT,FAKTURA,ZAMOWIENIE,USLUGA encja
    class R_TYP,R_POK_REZ,R_POK_UST,R_PRAC_UST,R_PRAC_REZ,R_GOSC,R_PLAT,R_FAKTURA,R_ZAM,R_USLUGA zwiazek
    class R_POK_WYP,R_WYP,R_POBYT identyfikujacy
    linkStyle 2,3,4,5,18,19 stroke:#0a645e,stroke-width:2px

</details>

### Założenia modelu

- E-mail jest wymagany i unikatowy osobno wśród gości i pracowników. Nazwy usług i wyposażenia są unikatowe w swoich katalogach. Typ pokoju identyfikuje para nazwa + standard.
- Rezerwacja dotyczy jednego pokoju i może prowadzić do najwyżej jednego pobytu. Pobyt może mieć najwyżej jedną fakturę.
- Pracownik rejestruje każdą rezerwację. Zgłoszenie usterki ma przypisanego pracownika od chwili rejestracji.
- Zaliczka może wpłynąć przed zameldowaniem. Usługi zamawia się podczas pobytu. Daty wymeldowania i realizacji usługi mogą początkowo być puste.
- Aktywne rezerwacje tego samego pokoju nie mogą się nakładać. Kolejny gość może przyjechać w dniu wyjazdu poprzedniego. Stawka za dobę i cena jednostkowa usługi są zapisywane w chwili rezerwacji lub zamówienia.
