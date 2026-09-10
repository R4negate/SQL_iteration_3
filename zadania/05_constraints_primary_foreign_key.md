# Zadania 05 - Constraints, Primary Key i Foreign Key

## Przygotowanie

W tych zadaniach pracujesz na schemacie:

```text
course
```

Jezeli chcesz zaczac od zera, mozesz usunac i stworzyc schemat ponownie:

```sql
DROP SCHEMA IF EXISTS course CASCADE;
CREATE SCHEMA course;
```

## Zadanie 1

Stworz tabele `course.customers`.

Tabela powinna zawierac kolumny:

- `customer_id`
- `customer_name`
- `email`
- `country`
- `signup_date`
- `acquisition_channel`

Wymagania:

- `customer_id` ma byc primary key,
- `customer_name` nie moze byc `NULL`,
- `customer_name` musi miec przynajmniej 2 znaki,
- `email` ma byc unikalny,
- `country` nie moze byc `NULL`,
- `country` moze miec tylko wartosci: `PL`, `DE`, `FR`, `US`,
- `signup_date` nie moze byc `NULL`.

## Zadanie 2

Stworz tabele `course.products`.

Tabela powinna zawierac kolumny:

- `product_id`
- `product_name`
- `category`
- `base_price`

Wymagania:

- `product_id` ma byc primary key,
- `product_name` nie moze byc `NULL`,
- `category` nie moze byc `NULL`,
- `category` moze miec tylko wartosci: `course`, `ebook`, `template`, `consulting`,
- `base_price` nie moze byc `NULL`,
- `base_price` nie moze byc mniejsze od `0`,
- `base_price` nie moze byc wieksze niz `10000`.

## Zadanie 3

Stworz tabele `course.orders`.

Tabela powinna zawierac kolumny:

- `order_id`
- `customer_id`
- `order_date`
- `status`
- `total_amount`

Wymagania:

- `order_id` ma byc primary key,
- `customer_id` nie moze byc `NULL`,
- `customer_id` ma byc foreign key do `course.customers(customer_id)`,
- `order_date` nie moze byc `NULL`,
- `status` nie moze byc `NULL`,
- `status` ma miec domyslna wartosc `pending`,
- `status` moze miec tylko wartosci: `pending`, `paid`, `cancelled`,
- `total_amount` nie moze byc `NULL`,
- `total_amount` nie moze byc mniejsze od `0`.

## Zadanie 4

Stworz tabele `course.order_items`.

Tabela powinna zawierac kolumny:

- `order_id`
- `line_number`
- `product_id`
- `quantity`
- `unit_price`

Wymagania:

- primary key ma skladac sie z dwoch kolumn: `order_id`, `line_number`,
- `order_id` ma byc foreign key do `course.orders(order_id)`,
- `product_id` ma byc foreign key do `course.products(product_id)`,
- `line_number` musi byc wieksze od `0`,
- `quantity` nie moze byc `NULL`,
- `quantity` musi byc wieksze od `0`,
- `unit_price` nie moze byc `NULL`,
- `unit_price` nie moze byc mniejsze od `0`,
- `unit_price` nie moze byc wieksze niz `10000`.

## Zadanie 5

Dodaj poprawnego klienta do tabeli `course.customers`.

Dane:

- `customer_id`: `1`
- `customer_name`: `Anna Nowak`
- `email`: `anna@example.com`
- `country`: `PL`
- `signup_date`: `2026-01-10`
- `acquisition_channel`: `google`

## Zadanie 6

Sprobuj dodac drugiego klienta z tym samym `customer_id`.

Zaobserwuj blad.

Napisz jednym zdaniem, ktory constraint zablokowal ten insert.

## Zadanie 7

Sprobuj dodac drugiego klienta z tym samym `email`.

Zaobserwuj blad.

Napisz jednym zdaniem, ktory constraint zablokowal ten insert.

## Zadanie 8

Sprobuj dodac klienta bez `customer_name`.

Zaobserwuj blad.

Napisz jednym zdaniem, ktory constraint zablokowal ten insert.

## Zadanie 9

Dodaj poprawny produkt do tabeli `course.products`.

Dane:

- `product_id`: `100`
- `product_name`: `SQL Starter Pack`
- `category`: `course`
- `base_price`: `149.00`

## Zadanie 10

Sprobuj dodac produkt z ujemna cena.

Zaobserwuj blad.

Napisz jednym zdaniem, ktory constraint zablokowal ten insert.

## Zadanie 11

Dodaj poprawne zamowienie dla klienta `customer_id = 1`.

Dane:

- `order_id`: `1000`
- `customer_id`: `1`
- `order_date`: `2026-02-01`
- `status`: `paid`
- `total_amount`: `149.00`

## Zadanie 12

Sprobuj dodac zamowienie dla klienta, ktory nie istnieje.

Przyklad:

- `order_id`: `1001`
- `customer_id`: `999`
- `order_date`: `2026-02-01`
- `status`: `paid`
- `total_amount`: `200.00`

Zaobserwuj blad.

Napisz jednym zdaniem, dlaczego foreign key zablokowal ten insert.

## Zadanie 13

Dodaj zamowienie bez kolumny `status`.

Sprawdz, jaka wartosc statusu zostala wpisana do tabeli.

## Zadanie 14

Sprobuj dodac zamowienie ze statusem:

```text
finished
```

Zaobserwuj blad.

Napisz jednym zdaniem, ktory constraint zablokowal ten insert.

## Zadanie 15

Dodaj poprawna pozycje zamowienia do `course.order_items`.

Dane:

- `order_id`: `1000`
- `line_number`: `1`
- `product_id`: `100`
- `quantity`: `1`
- `unit_price`: `149.00`

## Zadanie 16

Sprobuj dodac druga pozycje zamowienia z taka sama para:

- `order_id`: `1000`
- `line_number`: `1`

Zaobserwuj blad.

Napisz jednym zdaniem, dlaczego composite primary key zablokowal ten insert.

## Zadanie 17

Sprobuj dodac pozycje zamowienia dla produktu, ktory nie istnieje.

Przyklad:

- `order_id`: `1000`
- `line_number`: `2`
- `product_id`: `999`
- `quantity`: `1`
- `unit_price`: `99.00`

Zaobserwuj blad.

Napisz jednym zdaniem, ktory foreign key zablokowal ten insert.

## Zadanie 18

Sprobuj dodac pozycje zamowienia z `quantity = 0`.

Zaobserwuj blad.

Napisz jednym zdaniem, ktory constraint zablokowal ten insert.

## Zadanie 19

Napisz query, ktore pokazuje zamowienia razem z klientami.

Wynik powinien zawierac:

- `order_id`
- `order_date`
- `customer_id`
- `customer_name`
- `total_amount`

Po wykonaniu query odpowiedz jednym zdaniem:

Czy foreign key sam wykonuje join?

## Zadanie 20

Napisz query, ktore pokazuje pozycje zamowien razem z nazwa produktu.

Wynik powinien zawierac:

- `order_id`
- `line_number`
- `product_id`
- `product_name`
- `quantity`
- `unit_price`

## Zadanie 21

Sprobuj usunac klienta, ktory ma zamowienie.

Przyklad:

```sql
DELETE FROM course.customers
WHERE customer_id = 1;
```

Zaobserwuj blad.

Napisz jednym zdaniem, dlaczego baza nie pozwolila usunac tego klienta.

## Zadanie 22

Napisz krotka notatke:

```text
Jaka jest roznica miedzy PRIMARY KEY a FOREIGN KEY?
```

W notatce uzyj przykladu:

- `customers.customer_id`,
- `orders.customer_id`.

## Zadanie 23

Napisz krotka notatke:

```text
Dlaczego constraints sa wazne w pracy data engineera?
```

Uzyj przynajmniej dwoch przykladow problemow, ktore constraints moga zatrzymac.

## Zadanie 24

Napisz jedna tabele `course.test_payments`.

Tabela powinna zawierac:

- `payment_id`
- `order_id`
- `amount`
- `payment_status`

Wymagania:

- `payment_id` ma byc primary key,
- `order_id` ma byc foreign key do `course.orders(order_id)`,
- `amount` nie moze byc `NULL`,
- `amount` musi byc wieksze lub rowne `0`,
- `payment_status` ma miec domyslna wartosc `pending`,
- `payment_status` moze miec tylko wartosci: `pending`, `paid`, `failed`, `refunded`.

## Zadanie 25

Przetestuj tabele `course.test_payments`.

Wykonaj:

- jeden poprawny `INSERT`,
- jeden `INSERT` z nieistniejacym `order_id`,
- jeden `INSERT` z ujemnym `amount`,
- jeden `INSERT` z niedozwolonym `payment_status`.

Po kazdej probie zapisz krotko, czy rekord powinien przejsc, czy powinien zostac zablokowany.

## Zadanie 26

Sprobuj dodac klienta z krajem:

```text
ES
```

Zaobserwuj blad.

Napisz jednym zdaniem, ktory `CHECK` zablokowal ten insert.

## Zadanie 27

Sprobuj dodac klienta, ktorego `customer_name` ma tylko jeden znak.

Przyklad:

```text
A
```

Zaobserwuj blad.

Napisz jednym zdaniem, ktory `CHECK` zablokowal ten insert.

## Zadanie 28

Sprobuj dodac produkt z kategoria:

```text
video
```

Zaobserwuj blad.

Napisz jednym zdaniem, dlaczego baza nie przyjela tej kategorii.

## Zadanie 29

Sprobuj dodac produkt z `base_price = 15000`.

Zaobserwuj blad.

Napisz jednym zdaniem, ktory `CHECK` zablokowal ten insert.

## Zadanie 30

Sprobuj dodac pozycje zamowienia z `line_number = 0`.

Zaobserwuj blad.

Napisz jednym zdaniem, dlaczego `line_number` powinien zaczynac sie od `1`.

## Zadanie 31

Sprobuj dodac pozycje zamowienia z `unit_price = -1`.

Zaobserwuj blad.

Napisz jednym zdaniem, ktory constraint zablokowal ten insert.

## Zadanie 32

Sprobuj dodac pozycje zamowienia z `unit_price = 20000`.

Zaobserwuj blad.

Napisz jednym zdaniem, dlaczego taki limit ceny moze miec sens w tabeli treningowej.

## Zadanie 33

Stworz tabele `course.test_campaigns`.

Tabela powinna zawierac:

- `campaign_id`,
- `campaign_name`,
- `start_date`,
- `end_date`,
- `budget`.

Wymagania:

- `campaign_id` ma byc primary key,
- `campaign_name` nie moze byc `NULL`,
- `start_date` nie moze byc `NULL`,
- `end_date` nie moze byc `NULL`,
- `end_date` nie moze byc wczesniejsze niz `start_date`,
- `budget` musi byc wiekszy lub rowny `0`.

## Zadanie 34

Przetestuj tabele `course.test_campaigns`.

Wykonaj:

- jeden poprawny `INSERT`,
- jeden `INSERT`, w ktorym `end_date` jest przed `start_date`,
- jeden `INSERT` z ujemnym budzetem.

Po kazdej probie zapisz krotko, ktory constraint powinien zadzialac.

## Zadanie 35

Stworz tabele `course.test_signups`.

Tabela powinna zawierac:

- `signup_id`,
- `email`,
- `channel`,
- `signup_date`.

Wymagania:

- `signup_id` ma byc generowane automatycznie przez `GENERATED ALWAYS AS IDENTITY`,
- `signup_id` ma byc primary key,
- `email` nie moze byc `NULL`,
- para `email`, `channel` ma byc unikalna,
- `signup_date` ma miec domyslna wartosc dzisiejszej daty.

## Zadanie 36

Przetestuj tabele `course.test_signups`.

Wykonaj:

- dwa inserty z tym samym `email`, ale roznym `channel`,
- dwa inserty z tym samym `email` i tym samym `channel`.

Napisz jednym zdaniem, kiedy `UNIQUE (email, channel)` blokuje rekord.
