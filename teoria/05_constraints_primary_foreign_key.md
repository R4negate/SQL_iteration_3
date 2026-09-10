# 05 - Constraints, Primary Key i Foreign Key

## Czym sa constraints?

`CONSTRAINT` to regula nalozona na tabele albo kolumne.

Baza danych sprawdza te reguly podczas:

- `INSERT`, czyli dodawania nowych danych,
- `UPDATE`, czyli zmiany istniejacych danych,
- czasami tez podczas `DELETE`, jezeli usuwany rekord jest powiazany z innymi tabelami.

Constraints sa po to, zeby baza nie przyjmowala danych, ktore sa niepoprawne biznesowo albo technicznie.

Przyklad:

```sql
total_amount NUMERIC(10, 2) CHECK (total_amount >= 0)
```

Ta regula oznacza: wartosc zamowienia nie moze byc ujemna.

## Po co constraints data engineerowi?

Data engineer czesto buduje procesy, ktore pobieraja dane z plikow, API albo innych baz.

Bez constraints pipeline moze zaladowac dane, ktore wygladaja poprawnie technicznie, ale sa zle biznesowo.

Przyklady problemow:

- zamowienie bez klienta,
- produkt bez nazwy,
- dwa produkty z tym samym `product_id`,
- zamowienie z ujemna kwota,
- pozycja zamowienia z iloscia `0`,
- email klienta powtorzony wiele razy.

Constraints dzialaja jak bramka jakosci danych na poziomie bazy.

## Najwazniejsze typy constraints

W PostgreSQL najczesciej spotkasz:

- `NOT NULL` - kolumna musi miec wartosc,
- `UNIQUE` - wartosc nie moze sie powtarzac,
- `PRIMARY KEY` - glowny unikalny identyfikator rekordu,
- `FOREIGN KEY` - relacja do rekordu z innej tabeli,
- `CHECK` - wlasny warunek logiczny,
- `EXCLUDE` - bardziej zaawansowany constraint, np. do blokowania nakladajacych sie zakresow.

Oprocz tego czesto omawia sie razem z constraintami:

- `DEFAULT` - wartosc domyslna,
- `GENERATED` / `IDENTITY` - automatyczne generowanie wartosci, np. ID.

`DEFAULT` nie jest klasycznym constraintem tak jak `PRIMARY KEY` albo `CHECK`, ale pelni podobna role praktyczna: pomaga utrzymac przewidywalne dane.

## NOT NULL

`NOT NULL` oznacza, ze kolumna musi miec wartosc.

```sql
customer_name TEXT NOT NULL
```

Nie da sie wtedy dodac klienta bez nazwy.

Przyklad blednego inserta:

```sql
INSERT INTO course.customers (
    customer_id,
    customer_name,
    country,
    signup_date
)
VALUES (
    1,
    NULL,
    'PL',
    '2026-01-01'
);
```

Baza odrzuci taki rekord, bo `customer_name` nie moze byc `NULL`.

## UNIQUE

`UNIQUE` oznacza, ze wartosc w kolumnie nie moze sie powtarzac.

```sql
email TEXT UNIQUE
```

Dzieki temu dwoch klientow nie moze miec tego samego maila.

Przyklad:

```sql
CREATE TABLE course.customers (
    customer_id INT PRIMARY KEY,
    customer_name TEXT NOT NULL,
    email TEXT UNIQUE
);
```

To przejdzie:

```sql
INSERT INTO course.customers (customer_id, customer_name, email)
VALUES (1, 'Anna Nowak', 'anna@example.com');
```

To juz nie przejdzie:

```sql
INSERT INTO course.customers (customer_id, customer_name, email)
VALUES (2, 'Jan Kowalski', 'anna@example.com');
```

Bo email `anna@example.com` juz istnieje.

## CHECK

`CHECK` pozwala zapisac warunek logiczny, ktory musi byc prawdziwy.

```sql
total_amount NUMERIC(10, 2) CHECK (total_amount >= 0)
```

Przyklady:

```sql
quantity INT CHECK (quantity > 0)
```

```sql
discount_pct NUMERIC(5, 2) CHECK (discount_pct >= 0 AND discount_pct <= 100)
```

```sql
status TEXT CHECK (status IN ('pending', 'paid', 'cancelled'))
```

`CHECK` jest przydatny, kiedy chcesz ograniczyc mozliwe wartosci.

## CHECK w praktyce

`CHECK` jest najbardziej elastycznym constraintem, bo pozwala opisac wlasna regule biznesowa.

### Liczba musi byc wieksza od 0

To jest bardzo czesty przypadek dla ilosci sztuk.

```sql
quantity INT CHECK (quantity > 0)
```

Taki constraint oznacza:

```text
quantity moze byc 1, 2, 3...
quantity nie moze byc 0 ani liczba ujemna.
```

Przyklad:

```sql
INSERT INTO course.order_items (
    order_id,
    line_number,
    product_id,
    quantity,
    unit_price
)
VALUES (
    1,
    1,
    100,
    0,
    149.00
);
```

Ten insert powinien zostac zablokowany, jezeli tabela ma:

```sql
CHECK (quantity > 0)
```

### Liczba musi byc wieksza lub rowna 0

To pasuje np. do ceny albo kwoty zamowienia.

```sql
base_price NUMERIC(10, 2) CHECK (base_price >= 0)
```

To oznacza:

```text
Cena moze byc 0 albo wiecej.
Cena nie moze byc ujemna.
```

Przyklady:

```text
0.00    OK
99.99   OK
-10.00  blad
```

### Liczba musi byc w zakresie

To pasuje np. do procentow.

```sql
discount_pct NUMERIC(5, 2)
    CHECK (discount_pct >= 0 AND discount_pct <= 100)
```

To oznacza:

```text
Rabat moze byc od 0 do 100.
```

Przyklady:

```text
0       OK
15.50   OK
100     OK
120     blad
-5      blad
```

### Tekst musi byc jedna z dozwolonych wartosci

To pasuje np. do statusow.

```sql
status TEXT CHECK (status IN ('pending', 'paid', 'cancelled'))
```

To oznacza, ze baza przyjmie tylko te trzy wartosci.

Przyklady:

```text
pending    OK
paid       OK
cancelled  OK
finished   blad
```

Ten typ constraintu jest bardzo wazny, bo bez niego w danych moga pojawic sie rozne wersje tego samego statusu:

```text
paid
Paid
PAID
payment_done
finished
```

Potem raporty zaczynaja liczyc kazda wersje osobno.

### Tekst musi miec minimalna dlugosc

Mozesz tez sprawdzac dlugosc tekstu.

```sql
customer_name TEXT CHECK (LENGTH(customer_name) >= 2)
```

To oznacza:

```text
Nazwa klienta musi miec przynajmniej 2 znaki.
```

Przyklad:

```text
Anna  OK
A     blad
```

### Data nie moze byc z przyszlosci

Mozna pilnowac zasad dotyczacych dat.

```sql
signup_date DATE CHECK (signup_date <= CURRENT_DATE)
```

To oznacza:

```text
Data rejestracji klienta nie moze byc pozniejsza niz dzisiejsza data.
```

### Data koncowa nie moze byc przed data poczatkowa

To jest przyklad constraintu na poziomie tabeli, bo porownuje dwie kolumny.

```sql
CONSTRAINT chk_valid_date_range
    CHECK (end_date >= start_date)
```

Taki constraint ma sens np. dla promocji, subskrypcji albo kampanii marketingowych.

### Jedna kolumna zalezy od drugiej

Mozna tez pilnowac zaleznosci miedzy wartosciami.

```sql
CONSTRAINT chk_discounted_total_not_greater_than_total
    CHECK (discounted_total <= total)
```

To oznacza:

```text
Kwota po rabacie nie moze byc wieksza niz kwota przed rabatem.
```

### CHECK z kilkoma warunkami

Warunki mozna laczyc przez `AND` oraz `OR`.

```sql
CONSTRAINT chk_valid_order_amount
    CHECK (
        total_amount >= 0
        AND total_amount <= 100000
    )
```

To oznacza:

```text
Kwota zamowienia musi byc od 0 do 100000.
```

## CHECK a NULL

Wazna rzecz: `CHECK` i `NULL` potrafia zaskoczyc.

Constraint:

```sql
quantity INT CHECK (quantity > 0)
```

nie oznacza automatycznie, ze `quantity` nie moze byc `NULL`.

Jezeli chcesz wymusic wartosc i dodatnia liczbe, zapisz oba warunki:

```sql
quantity INT NOT NULL CHECK (quantity > 0)
```

Dlatego bardzo czesto laczy sie:

```sql
NOT NULL + CHECK
```

Przyklad:

```sql
quantity INT NOT NULL CHECK (quantity > 0)
```

To oznacza:

- wartosc musi byc podana,
- wartosc musi byc wieksza od `0`.

## UNIQUE na jednej i wielu kolumnach

`UNIQUE` moze dotyczyc jednej kolumny:

```sql
email TEXT UNIQUE
```

Moze tez dotyczyc pary albo grupy kolumn.

Przyklad:

```sql
CONSTRAINT uq_customer_channel
    UNIQUE (customer_id, acquisition_channel)
```

To oznacza, ze ta sama para wartosci nie moze sie powtorzyc.

Sama kolumna `customer_id` moglaby sie powtarzac.

Sama kolumna `acquisition_channel` tez moglaby sie powtarzac.

Ale para:

```text
customer_id + acquisition_channel
```

musi byc unikalna.

## PRIMARY KEY vs UNIQUE

`PRIMARY KEY` i `UNIQUE` sa podobne, bo oba blokuja duplikaty.

Roznica:

- `PRIMARY KEY` identyfikuje caly rekord,
- tabela powinna miec maksymalnie jeden primary key,
- primary key nie moze byc `NULL`,
- `UNIQUE` moze byc na wielu kolumnach niezaleznie,
- `UNIQUE` sluzy do dodatkowych unikalnych zasad, np. email.

Przyklad:

```sql
customer_id INT PRIMARY KEY,
email TEXT UNIQUE
```

Tu:

- `customer_id` jest technicznym identyfikatorem klienta,
- `email` tez nie moze sie powtarzac, ale nie jest glownym identyfikatorem tabeli.

## GENERATED AS IDENTITY

W PostgreSQL czesto nie wpisuje sie ID recznie, tylko pozwala bazie wygenerowac je automatycznie.

Przyklad:

```sql
customer_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY
```

Wtedy przy insercie nie trzeba podawac `customer_id`:

```sql
INSERT INTO course.customers (
    customer_name,
    email,
    country,
    signup_date
)
VALUES (
    'Anna Nowak',
    'anna@example.com',
    'PL',
    '2026-01-10'
);
```

Baza sama nada kolejny numer.

W projektach edukacyjnych czesto wpisujemy ID recznie, bo latwiej widac relacje.

W aplikacjach produkcyjnych `IDENTITY` jest bardzo czeste.

## EXCLUDE

`EXCLUDE` to bardziej zaawansowany constraint PostgreSQL.

Uzywa sie go rzadziej na poczatku nauki.

Pozwala blokowac konflikty bardziej zlozone niz zwykle `UNIQUE`.

Typowy przyklad: nie pozwol, zeby dwa okresy czasu nachodzily na siebie dla tego samego zasobu.

Przyklad biznesowy:

```text
Ten sam pokoj hotelowy nie moze miec dwoch rezerwacji w tym samym czasie.
```

Na tym etapie wystarczy wiedziec, ze `EXCLUDE` istnieje, ale podstawowe i najwazniejsze constraints to:

- `NOT NULL`,
- `UNIQUE`,
- `PRIMARY KEY`,
- `FOREIGN KEY`,
- `CHECK`.

## DEFAULT

`DEFAULT` ustawia wartosc domyslna, jezeli przy `INSERT` nie podasz wartosci dla danej kolumny.

```sql
status TEXT DEFAULT 'pending'
```

Przyklad:

```sql
CREATE TABLE course.orders (
    order_id INT PRIMARY KEY,
    status TEXT NOT NULL DEFAULT 'pending'
);
```

Wtedy taki insert:

```sql
INSERT INTO course.orders (order_id)
VALUES (1);
```

doda rekord ze statusem:

```text
pending
```

## PRIMARY KEY

`PRIMARY KEY` to glowny identyfikator rekordu w tabeli.

Przyklad:

```sql
customer_id INT PRIMARY KEY
```

Oznacza to, ze:

- `customer_id` nie moze byc `NULL`,
- `customer_id` nie moze sie duplikowac,
- jeden `customer_id` wskazuje dokladnie jeden rekord.

Tabela bez primary key moze miec duplikaty, ktore trudno potem rozroznic.

Tabela z primary key wymusza jednoznacznosc.

Przyklad:

```sql
CREATE TABLE course.customers (
    customer_id INT PRIMARY KEY,
    customer_name TEXT NOT NULL
);
```

To przejdzie:

```sql
INSERT INTO course.customers (customer_id, customer_name)
VALUES (1, 'Anna Nowak');
```

To nie przejdzie:

```sql
INSERT INTO course.customers (customer_id, customer_name)
VALUES (1, 'Jan Kowalski');
```

Bo `customer_id = 1` juz istnieje.

## Primary key jako regola biznesowa

Primary key odpowiada na pytanie:

```text
Po czym jednoznacznie rozpoznaje jeden rekord?
```

Przyklady:

- klient: `customer_id`,
- produkt: `product_id`,
- zamowienie: `order_id`,
- pozycja zamowienia: czasem `order_item_id`, a czasem para `order_id + line_number`.

## Composite Primary Key

Composite primary key to klucz glowny zlozony z kilku kolumn.

Przyklad:

```sql
PRIMARY KEY (order_id, line_number)
```

To oznacza, ze sama kolumna `order_id` moze sie powtarzac, bo jedno zamowienie ma wiele pozycji.

Ale para:

```text
order_id + line_number
```

musi byc unikalna.

Przyklad poprawnych danych:

| order_id | line_number | product_id |
|---|---|---|
| 1 | 1 | 100 |
| 1 | 2 | 101 |
| 2 | 1 | 100 |

Przyklad blednych danych:

| order_id | line_number | product_id |
|---|---|---|
| 1 | 1 | 100 |
| 1 | 1 | 101 |

Drugi przypadek jest bledny, bo dwa razy wystepuje para `(1, 1)`.

## FOREIGN KEY

`FOREIGN KEY` to relacja do rekordu z innej tabeli.

Przyklad:

```sql
customer_id INT REFERENCES course.customers(customer_id)
```

Oznacza to:

```text
W orders.customer_id moze pojawic sie tylko taki customer_id,
ktory istnieje w course.customers.customer_id.
```

## Jak lacza sie Primary Key i Foreign Key?

Primary key tworzy liste poprawnych identyfikatorow.

Foreign key korzysta z tej listy i pilnuje, zeby inna tabela wskazywala tylko istniejace rekordy.

Przyklad:

```text
course.customers
----------------
customer_id  PRIMARY KEY

        ^
        |
        |

course.orders
-------------
customer_id  FOREIGN KEY
```

`course.customers.customer_id` mowi:

```text
To sa legalni klienci.
```

`course.orders.customer_id` mowi:

```text
Kazde zamowienie musi wskazywac legalnego klienta.
```

## Przyklad relacji customers -> orders

```sql
CREATE TABLE course.customers (
    customer_id INT PRIMARY KEY,
    customer_name TEXT NOT NULL,
    email TEXT UNIQUE,
    country TEXT NOT NULL,
    signup_date DATE NOT NULL,
    acquisition_channel TEXT
);
```

```sql
CREATE TABLE course.orders (
    order_id INT PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date DATE NOT NULL,
    status TEXT NOT NULL DEFAULT 'pending',
    total_amount NUMERIC(10, 2) NOT NULL CHECK (total_amount >= 0),

    CONSTRAINT fk_orders_customer
        FOREIGN KEY (customer_id)
        REFERENCES course.customers(customer_id)
);
```

Ten fragment:

```sql
CONSTRAINT fk_orders_customer
    FOREIGN KEY (customer_id)
    REFERENCES course.customers(customer_id)
```

oznacza:

```text
Kolumna orders.customer_id wskazuje na customers.customer_id.
```

## Dlaczego constraint ma nazwe?

Constraint mozna zapisac bez nazwy:

```sql
customer_id INT REFERENCES course.customers(customer_id)
```

Ale w wiekszych projektach lepiej nadawac nazwy:

```sql
CONSTRAINT fk_orders_customer
    FOREIGN KEY (customer_id)
    REFERENCES course.customers(customer_id)
```

Nazwa pomaga:

- zrozumiec blad z bazy,
- usunac constraint przez `ALTER TABLE`,
- dokumentowac relacje w tabeli.

## Przyklad relacji orders -> order_items

Jedno zamowienie moze miec wiele pozycji.

```text
orders
------
order_id = 1

order_items
-----------
order_id = 1, line_number = 1
order_id = 1, line_number = 2
order_id = 1, line_number = 3
```

Tabela:

```sql
CREATE TABLE course.order_items (
    order_id INT NOT NULL,
    line_number INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL CHECK (quantity > 0),
    unit_price NUMERIC(10, 2) NOT NULL CHECK (unit_price >= 0),

    CONSTRAINT pk_order_items
        PRIMARY KEY (order_id, line_number),

    CONSTRAINT fk_order_items_order
        FOREIGN KEY (order_id)
        REFERENCES course.orders(order_id),

    CONSTRAINT fk_order_items_product
        FOREIGN KEY (product_id)
        REFERENCES course.products(product_id)
);
```

Tutaj dzieje sie kilka rzeczy naraz:

- `(order_id, line_number)` identyfikuje jedna pozycje zamowienia,
- `order_id` musi istniec w `course.orders`,
- `product_id` musi istniec w `course.products`,
- `quantity` musi byc wieksze od `0`,
- `unit_price` nie moze byc ujemny.

## Foreign key a JOIN

`FOREIGN KEY` nie robi joina automatycznie.

Foreign key tylko pilnuje, zeby relacja byla poprawna.

Join nadal piszesz sam:

```sql
SELECT
    o.order_id,
    c.customer_name,
    o.total_amount
FROM course.orders o
JOIN course.customers c
    ON o.customer_id = c.customer_id;
```

Ale dzieki foreign key masz wieksza pewnosc, ze `orders.customer_id` wskazuje prawdziwego klienta.

## Co sie stanie przy DELETE?

Jezeli tabela `orders` ma foreign key do `customers`, to baza moze zablokowac usuniecie klienta, ktory ma zamowienia.

Przyklad:

```sql
DELETE FROM course.customers
WHERE customer_id = 1;
```

Jezeli klient ma zamowienia, baza moze odpowiedziec bledem, bo usuniecie klienta zostawiloby zamowienia bez wlasciciela.

To jest dobre zabezpieczenie.

## ON DELETE

Przy foreign key mozna okreslic, co ma sie stac z rekordami zaleznymi podczas usuwania rekordu nadrzednego.

Najczestsze opcje:

- `ON DELETE RESTRICT` albo domyslne zachowanie - nie pozwol usunac rekordu, jezeli ktos na niego wskazuje,
- `ON DELETE CASCADE` - usun tez rekordy zalezne,
- `ON DELETE SET NULL` - ustaw foreign key na `NULL`.

Przyklad:

```sql
FOREIGN KEY (order_id)
REFERENCES course.orders(order_id)
ON DELETE CASCADE
```

To oznacza:

```text
Jesli usuniesz zamowienie, baza usunie tez jego pozycje.
```

Trzeba z tym uwazac, bo `CASCADE` moze usunac wiecej danych, niz sie spodziewasz.

## Column-level vs table-level constraint

Constraint mozna zapisac przy kolumnie:

```sql
customer_id INT PRIMARY KEY
```

Albo na poziomie tabeli:

```sql
CONSTRAINT pk_customers
    PRIMARY KEY (customer_id)
```

W prostych przypadkach oba style dzialaja podobnie.

Table-level jest czytelniejszy, gdy:

- constraint ma nazwe,
- primary key sklada sie z kilku kolumn,
- foreign key opisuje relacje,
- check dotyczy kilku kolumn naraz.

Przyklad checka na kilku kolumnach:

```sql
CONSTRAINT chk_discount_not_greater_than_total
    CHECK (discounted_total <= total)
```

## Pelny przyklad czterech tabel

```sql
CREATE SCHEMA IF NOT EXISTS course;

CREATE TABLE course.customers (
    customer_id INT,
    customer_name TEXT NOT NULL,
    email TEXT UNIQUE,
    country TEXT NOT NULL,
    signup_date DATE NOT NULL,
    acquisition_channel TEXT,

    CONSTRAINT pk_customers
        PRIMARY KEY (customer_id)
);
```

```sql
CREATE TABLE course.products (
    product_id INT,
    product_name TEXT NOT NULL,
    category TEXT NOT NULL,
    base_price NUMERIC(10, 2) NOT NULL CHECK (base_price >= 0),

    CONSTRAINT pk_products
        PRIMARY KEY (product_id)
);
```

```sql
CREATE TABLE course.orders (
    order_id INT,
    customer_id INT NOT NULL,
    order_date DATE NOT NULL,
    status TEXT NOT NULL DEFAULT 'pending',
    total_amount NUMERIC(10, 2) NOT NULL CHECK (total_amount >= 0),

    CONSTRAINT pk_orders
        PRIMARY KEY (order_id),

    CONSTRAINT fk_orders_customer
        FOREIGN KEY (customer_id)
        REFERENCES course.customers(customer_id),

    CONSTRAINT chk_orders_status
        CHECK (status IN ('pending', 'paid', 'cancelled'))
);
```

```sql
CREATE TABLE course.order_items (
    order_id INT NOT NULL,
    line_number INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL CHECK (quantity > 0),
    unit_price NUMERIC(10, 2) NOT NULL CHECK (unit_price >= 0),

    CONSTRAINT pk_order_items
        PRIMARY KEY (order_id, line_number),

    CONSTRAINT fk_order_items_order
        FOREIGN KEY (order_id)
        REFERENCES course.orders(order_id),

    CONSTRAINT fk_order_items_product
        FOREIGN KEY (product_id)
        REFERENCES course.products(product_id)
);
```

## Najwazniejsze do zapamietania

- Constraint to regula, ktorej baza pilnuje za ciebie.
- `NOT NULL` wymaga wartosci.
- `UNIQUE` blokuje duplikaty.
- `PRIMARY KEY` jednoznacznie identyfikuje rekord i nie pozwala na `NULL` ani duplikaty.
- `FOREIGN KEY` pilnuje relacji miedzy tabelami.
- `CHECK` pilnuje warunkow biznesowych.
- `CHECK (quantity > 0)` blokuje wartosci `0`, ujemne i inne niezgodne z warunkiem.
- `DEFAULT` ustawia wartosc domyslna.
- `IDENTITY` pozwala bazie automatycznie generowac ID.
- Primary key i foreign key sa fundamentem relacyjnej bazy danych.
- Foreign key nie robi joina automatycznie, ale sprawia, ze join ma sens biznesowy.
