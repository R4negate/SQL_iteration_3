# Zadania 12 - Zadania przekrojowe DDL

## Zadanie 1

Zresetuj schemat `course`.

Nastepnie stworz go ponownie.

## Zadanie 2

Stworz od zera tabele `course.customers`.

Tabela powinna miec:

- primary key,
- `NOT NULL` na wymaganych kolumnach,
- `UNIQUE` na emailu,
- `CHECK` na dozwolonych krajach.

## Zadanie 3

Stworz od zera tabele `course.products`.

Tabela powinna miec:

- primary key,
- `NOT NULL` na wymaganych kolumnach,
- `CHECK` na cenie,
- `CHECK` na dozwolonych kategoriach.

## Zadanie 4

Stworz od zera tabele `course.orders`.

Tabela powinna miec:

- primary key,
- foreign key do `course.customers`,
- `DEFAULT` dla statusu,
- `CHECK` dla statusu,
- `CHECK` dla kwoty zamowienia.

## Zadanie 5

Stworz od zera tabele `course.order_items`.

Tabela powinna miec:

- primary key,
- foreign key do `course.orders`,
- foreign key do `course.products`,
- `CHECK` dla ilosci,
- `CHECK` dla ceny jednostkowej.

## Zadanie 6

Dodaj po dwa poprawne rekordy do:

- `course.customers`,
- `course.products`,
- `course.orders`,
- `course.order_items`.

## Zadanie 7

Przetestuj minimum piec blednych insertow.

Kazdy insert powinien zostac zablokowany przez inny constraint.

## Zadanie 8

Dodaj do `course.orders` nowa kolumne:

```text
currency
```

Domyslna wartosc:

```text
PLN
```

## Zadanie 9

Stworz indeksy:

- na `course.orders(order_date)`,
- na `course.orders(customer_id)`,
- na `course.order_items(order_id)`,
- na `course.order_items(product_id)`.

## Zadanie 10

Stworz widok `course.v_full_order_details`, ktory laczy:

- `course.customers`,
- `course.orders`,
- `course.order_items`,
- `course.products`.

Wynik powinien zawierac:

- `customer_id`,
- `customer_name`,
- `order_id`,
- `order_date`,
- `status`,
- `product_id`,
- `product_name`,
- `quantity`,
- `unit_price`,
- `line_value`.

## Zadanie 11

Stworz materialized view `course.mv_sales_by_product`.

Wynik powinien zawierac:

- `product_id`,
- `product_name`,
- `units_sold`,
- `total_revenue`.

## Zadanie 12

Dodaj komentarze do:

- tabeli `course.customers`,
- tabeli `course.orders`,
- tabeli `course.order_items`,
- kolumny `course.orders.total_amount`.

## Zadanie 13

Napisz jedno query do `information_schema.columns`, ktore pokazuje kolumny wszystkich tabel w schemacie `course`.

Wynik powinien zawierac:

- `table_name`,
- `column_name`,
- `data_type`.

## Zadanie 14

Napisz jedno query do `information_schema.table_constraints`, ktore pokazuje constrainty w schemacie `course`.

Wynik powinien zawierac:

- `table_name`,
- `constraint_name`,
- `constraint_type`.

## Zadanie 15

Napisz krotka notatke:

```text
Jak DDL pomaga data engineerowi budowac bezpieczne i czytelne dane?
```
