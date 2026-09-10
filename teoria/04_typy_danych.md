# 04 - Typy danych

## Po co sa typy danych?

Typ danych mowi bazie, jakiego rodzaju wartosc moze znajdowac sie w kolumnie.

Przyklady:

- liczba calkowita,
- tekst,
- data,
- kwota,
- prawda/falsz,
- JSON,
- identyfikator UUID.

Dobry typ danych pomaga:

- unikac blednych wartosci,
- poprawnie sortowac,
- poprawnie liczyc,
- oszczedzac miejsce,
- lepiej dokumentowac tabele,
- szybciej wykonywac zapytania,
- unikac dziwnych bledow w raportach.

Zly typ danych potrafi zepsuc analityke.

Przyklad: jezeli kwote zapiszesz jako `TEXT`, baza pozwoli wpisac:

```text
abc
duzo
brak
100 PLN
```

Jezeli kwote zapiszesz jako `NUMERIC`, baza oczekuje liczby.

Typ danych to pierwsza warstwa kontroli jakosci danych.

## Liczby calkowite

Liczby calkowite to liczby bez czesci po przecinku.

W PostgreSQL najczesciej spotkasz:

- `SMALLINT`,
- `INT` / `INTEGER`,
- `BIGINT`.

## SMALLINT

Malo uzywany

`SMALLINT` przechowuje male liczby calkowite.

Przyklad:

```sql
rating SMALLINT
```

Moze miec sens dla:

- ocen od 1 do 5,
- malych kodow,
- malych licznikow.

Na start zwykle czesciej uzywa sie `INT`, bo jest prostszy i bezpieczniejszy.

## INT / INTEGER

Najczesciej uzywany

`INT` i `INTEGER` oznaczaja praktycznie to samo.

Przechowuja liczby calkowite.

Przyklad:

```sql
customer_id INT
```

Dobre dla:

- identyfikatorow,
- liczby sztuk,
- licznikow,
- numerow stron,
- ilosci produktow.

Przyklady:

```sql
quantity INT
total_products INT
customer_id INT
```

## BIGINT

`BIGINT` przechowuje bardzo duze liczby calkowite.

Przyklad:

```sql
event_id BIGINT
```

Dobre dla:

- bardzo duzych tabel,
- logow zdarzen,
- danych klikniec,
- systemow, gdzie ID moze przekroczyc zakres zwyklego `INT`,
- danych przychodzacych z systemow, ktore uzywaja duzych ID.

W data engineeringu `BIGINT` czesto pojawia sie przy duzych wolumenach danych.

Przyklad:

```sql
CREATE TABLE analytics.events (
    event_id BIGINT,
    user_id BIGINT,
    event_name TEXT
);
```

## Liczby dziesietne: NUMERIC

`NUMERIC(10, 2)` przechowuje liczby dziesietne z dokladnoscia.

Przyklad:

```sql
total_amount NUMERIC(10, 2)
```

`10` oznacza maksymalna liczbe cyfr lacznie.

`2` oznacza liczbe miejsc po przecinku.

Przyklad:

```text
12345678.90
```

To ma 10 cyfr lacznie, z czego 2 po przecinku.

`NUMERIC` jest dobre dla:

- cen,
- kwot,
- rabatow,
- podatkow,
- wartosci finansowych,
- danych, gdzie liczy sie dokladnosc.

Przyklad:

```sql
base_price NUMERIC(10, 2)
discount_percentage NUMERIC(5, 2)
total_amount NUMERIC(12, 2)
```

## FLOAT, REAL, DOUBLE PRECISION

W PostgreSQL typy przyblizone to m.in.:

- `REAL`,
- `DOUBLE PRECISION`,
- czasem potocznie mowi sie na nie floaty.

One nie przechowuja liczby idealnie dokladnie, tylko przyblizenie.

To znaczy, ze operacje na takich liczbach moga dac minimalnie dziwne wyniki.

Przyklad koncepcyjny:

```text
0.1 + 0.2 moze nie wyjsc idealnie jako 0.3
```

To nie jest blad PostgreSQL. Tak dzialaja liczby zmiennoprzecinkowe w komputerach.

## Kiedy uzywac floata?

Floaty sa dobre dla pomiarow, gdzie mala niedokladnosc jest akceptowalna.

Przyklady:

- temperatura,
- predkosc,
- wysokosc,
- odleglosc,
- wspolrzedne geograficzne,
- wyniki modeli ML,
- pomiary sensorow.

Przyklad:

```sql
temperature_c DOUBLE PRECISION
latitude DOUBLE PRECISION
longitude DOUBLE PRECISION
```

## Kiedy NIE uzywac floata?

Nie uzywaj `REAL` ani `DOUBLE PRECISION` do pieniedzy.

Zle:

```sql
total_amount DOUBLE PRECISION
```

Lepiej:

```sql
total_amount NUMERIC(10, 2)
```

Dlaczego?

Bo w finansach oczekujesz dokladnosci co do grosza/centa.

Float jest typem przyblizonym, wiec moze powodowac drobne roznice w obliczeniach.

Do pieniedzy, faktur, platnosci i raportow finansowych wybieraj `NUMERIC`.

## NUMERIC vs DOUBLE PRECISION

| Typ | Dokladnosc | Kiedy uzywac |
|---|---|---|
| `NUMERIC` | dokladna | pieniadze, podatki, rabaty |
| `DOUBLE PRECISION` | przyblizona | pomiary, nauka, geolokalizacja, ML |

Prosta zasada:

```text
Pieniadze -> NUMERIC
Pomiary -> DOUBLE PRECISION
```

## TEXT

`TEXT` przechowuje tekst dowolnej dlugosci.

Przyklad:

```sql
customer_name TEXT
```

Dobre dla:

- nazw,
- opisow,
- maili,
- kategorii,
- statusow.

W PostgreSQL `TEXT` jest bardzo czesto uzywany.

Przyklady:

```sql
customer_name TEXT
email TEXT
status TEXT
product_name TEXT
```

## VARCHAR

`VARCHAR(n)` przechowuje tekst z limitem dlugosci.

Przyklad:

```sql
country_code VARCHAR(2)
```

To oznacza maksymalnie 2 znaki.

Przyklady:

```sql
country_code VARCHAR(2)
currency_code VARCHAR(3)
```

W PostgreSQL czesto mozna uzyc `TEXT` i dodac `CHECK`, jezeli limit ma znaczenie biznesowe.

Przyklad:

```sql
country_code TEXT CHECK (LENGTH(country_code) = 2)
```

## CHAR

`CHAR(n)` przechowuje tekst o stalej dlugosci.

Przyklad:

```sql
country_code CHAR(2)
```

W praktyce nie uzywa sie `CHAR`, bo dopelnia tekst spacjami i potrafi zaskoczyc.

Najczesciej wystarczy:

- `TEXT`,
- ewentualnie `VARCHAR(n)`.

## DATE

`DATE` przechowuje date bez godziny.

Przyklad:

```sql
order_date DATE
```

Przykladowa wartosc:

```text
2026-01-15
```

Dobre dla:

- daty zamowienia, jezeli godzina nie ma znaczenia,
- daty rejestracji,
- daty raportowej,
- daty przetwarzania danych.

## TIME

`TIME` przechowuje sama godzine bez daty.

Przyklad:

```sql
opening_time TIME
```

W data engineeringu spotyka sie rzadziej niz `DATE` i `TIMESTAMP`.

## TIMESTAMP

`TIMESTAMP` przechowuje date i godzine.

Przyklad:

```sql
created_at TIMESTAMP
```

Przykladowa wartosc:

```text
2026-01-15 10:30:00
```

Dobre dla:

- czasu utworzenia rekordu,
- czasu aktualizacji,
- czasu zdarzenia,
- logow,
- danych eventowych.

## TIMESTAMP WITH TIME ZONE

Malo uzywany

W PostgreSQL zapisuje sie to jako:

```sql
TIMESTAMPTZ
```

albo:

```sql
TIMESTAMP WITH TIME ZONE
```

Ten typ jest dobry, kiedy dane maja znaczenie globalne i dotycza roznych stref czasowych

Przyklady:

- eventy z aplikacji,
- logi systemowe,
- platnosci,
- integracje API z wielu krajow.

Praktyczna zasada:

```text
Lokalna data raportowa -> DATE
Moment zdarzenia w systemie -> TIMESTAMPTZ
```

## BOOLEAN

`BOOLEAN` przechowuje prawde albo falsz.

Przyklad:

```sql
is_active BOOLEAN
```

Typowe wartosci:

```text
true
false
```

Dobre dla:

- flag,
- statusow tak/nie,
- informacji typu aktywny/nieaktywny.

Przyklady:

```sql
is_paid BOOLEAN
is_active BOOLEAN
has_discount BOOLEAN
```

Uwaga: jezeli masz wiecej niz dwa stany, `BOOLEAN` nie wystarczy.

Przyklad:

```text
pending
paid
failed
refunded
```

To nie jest dobry przypadek na boolean. Lepiej uzyc `TEXT` z `CHECK`.

## UUID

`UUID` przechowuje uniwersalny identyfikator.

Przyklad:

```sql
event_uuid UUID
```

Przykladowa wartosc:

```text
550e8400-e29b-41d4-a716-446655440000
```

UUID jest dobre, gdy:

- dane pochodza z wielu systemow,
- ID musi byc unikalne globalnie,
- nie chcesz prostego numerowania `1, 2, 3`,
- system zrodlowy juz wysyla UUID.

Na start w tabelach treningowych uzywamy `INT`, bo latwiej zrozumiec relacje.

## JSON i JSONB

Zostawia do poczytania we wlasnym zakresie czym jest json, przyda sie nam to gdy dojdziemy do pythona

PostgreSQL pozwala przechowywac JSON.

Najczesciej uzywa sie:

```sql
JSONB
```

Przyklad:

```sql
raw_payload JSONB
```

To jest przydatne w data engineeringu, gdy zapisujesz surowa odpowiedz z API.

Przyklad tabeli raw:

```sql
CREATE TABLE raw.api_responses (
    response_id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    source_name TEXT NOT NULL,
    loaded_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    raw_payload JSONB NOT NULL
);
```

`JSONB` jest dobre do warstwy raw/bronze.

Ale do raportowania zwykle lepiej rozbic JSON na normalne kolumny.

Zle dla raportu:

```sql
raw_payload JSONB
```

Lepsze dla raportu:

```sql
customer_id INT
order_date DATE
total_amount NUMERIC(10, 2)
```

## ARRAY

PostgreSQL ma tez tablice, np.:

```sql
tags TEXT[]
```

Tablice bywaja wygodne, ale w relacyjnej bazie czesto lepiej stworzyc osobna tabele.

Przyklad:

Zamiast:

```sql
product_tags TEXT[]
```

czesto lepiej:

```text
products
product_tags
```

czyli jedna tabela na produkty i druga tabela na tagi produktow.

## Dobor typu danych w naszych tabelach

### course.customers

```sql
customer_id INT
customer_name TEXT
email TEXT
country TEXT
signup_date DATE
acquisition_channel TEXT
```

Dlaczego:

- `customer_id` to liczba calkowita,
- nazwy i email to tekst,
- kraj i kanal pozyskania to kategorie tekstowe,
- `signup_date` to data bez godziny.

### course.products

```sql
product_id INT
product_name TEXT
category TEXT
base_price NUMERIC(10, 2)
```

Dlaczego:

- `product_id` to identyfikator,
- nazwa i kategoria to tekst,
- cena powinna byc `NUMERIC`, nie floatem.

### course.orders

```sql
order_id INT
customer_id INT
order_date DATE
status TEXT
total_amount NUMERIC(10, 2)
```

Dlaczego:

- `order_id` i `customer_id` to identyfikatory,
- `order_date` to data raportowa,
- `status` to tekst ograniczony constraintem,
- `total_amount` to kwota, wiec `NUMERIC`.

### course.order_items

```sql
order_item_id INT
order_id INT
product_id INT
quantity INT
unit_price NUMERIC(10, 2)
```

Dlaczego:

- identyfikatory jako `INT`,
- `quantity` jako liczba calkowita,
- `unit_price` jako `NUMERIC`, bo to cena.

## Czego unikac?

### Nie zapisuj wszystkiego jako TEXT

Zle:

```sql
total_amount TEXT
order_date TEXT
quantity TEXT
```

Problem:

- trudniej liczyc,
- trudniej sortowac,
- latwiej wpuscic zle dane,
- raporty moga dawac dziwne wyniki.

### Nie uzywaj FLOAT do pieniedzy

Zle:

```sql
price DOUBLE PRECISION
```

Lepiej:

```sql
price NUMERIC(10, 2)
```

### Nie uzywaj BOOLEAN dla wielu statusow

Zle:

```sql
is_paid BOOLEAN
```

jezeli realne statusy to:

```text
pending
paid
failed
refunded
cancelled
```

Lepiej:

```sql
status TEXT CHECK (status IN ('pending', 'paid', 'failed', 'refunded', 'cancelled'))
```

### Nie przesadzaj z VARCHAR

W PostgreSQL `TEXT` jest normalnym i czesto dobrym wyborem.

Nie trzeba robic:

```sql
customer_name VARCHAR(37)
```

jezeli limit nie ma prawdziwego znaczenia biznesowego.

## Szybka sciaga

| Dane | Dobry typ |
|---|---|
| ID w malej/sredniej tabeli | `INT` |
| ID w bardzo duzej tabeli | `BIGINT` |
| Ilosc sztuk | `INT` |
| Cena / kwota | `NUMERIC(10, 2)` |
| Procent | `NUMERIC(5, 2)` |
| Nazwa / email / status | `TEXT` |
| Kod kraju | `TEXT` albo `VARCHAR(2)` |
| Data bez godziny | `DATE` |
| Data z godzina | `TIMESTAMP` |
| Moment zdarzenia globalnie | `TIMESTAMPTZ` |
| Flaga tak/nie | `BOOLEAN` |
| Surowa odpowiedz z API | `JSONB` |
| Globalny identyfikator | `UUID` |
| Pomiar, np. temperatura | `DOUBLE PRECISION` |

## Najwazniejsze do zapamietania

- `INT` jest dla liczb calkowitych.
- `BIGINT` jest dla bardzo duzych liczb calkowitych.
- `TEXT` jest dobrym domyslnym typem dla tekstu w PostgreSQL.
- `NUMERIC(10, 2)` jest dobre dla pieniedzy.
- `DOUBLE PRECISION` jest dobre dla pomiarow, ale nie dla pieniedzy.
- `DATE` jest dla daty bez godziny.
- `TIMESTAMP` jest dla daty z godzina.
- `TIMESTAMPTZ` jest dobre dla momentow zdarzen w systemach globalnych.
- `BOOLEAN` jest dla `true` / `false`.
- `JSONB` jest dobre dla surowych danych z API, ale nie jako docelowy model raportowy.
- Typ danych to pierwsze zabezpieczenie przed zlymi danymi.
