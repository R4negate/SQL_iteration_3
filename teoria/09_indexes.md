# 09 - Indexes

## Czym jest indeks?

Indeks to dodatkowa struktura, ktora pomaga bazie szybciej znajdowac dane.

Najprostsza analogia: indeks w ksiazce.

Bez indeksu baza moze musiec sprawdzic wiele wierszy.

Z indeksem baza ma skrot do danych.

## Tworzenie indeksu

```sql
CREATE INDEX idx_orders_order_date
ON course.orders(order_date);
```

Ten indeks pomaga przy filtrowaniu po `order_date`.

## Kiedy indeks pomaga?

Indeks moze pomoc przy:

- `WHERE`,
- `JOIN`,
- `ORDER BY`,
- czestym filtrowaniu po danej kolumnie.

Przyklad:

```sql
SELECT *
FROM course.orders
WHERE order_date >= '2026-01-01';
```

## Indeks na foreign key

Foreign key nie zawsze tworzy indeks automatycznie.

Czesto warto dodac indeks na kolumnie uzywanej do joinow.

Przyklad:

```sql
CREATE INDEX idx_orders_customer_id
ON course.orders(customer_id);
```

Pomaga przy joinie:

```sql
SELECT
    o.order_id,
    c.customer_name
FROM course.orders o
JOIN course.customers c
    ON o.customer_id = c.customer_id;
```

## Composite index

Indeks moze miec wiecej niz jedna kolumne.

```sql
CREATE INDEX idx_orders_customer_date
ON course.orders(customer_id, order_date);
```

Taki indeks moze pomoc przy pytaniach:

```text
Pokaz zamowienia klienta w czasie.
```

## Koszt indeksow

Indeksy nie sa darmowe.

Pomagaja przy odczycie, ale moga spowalniac:

- `INSERT`,
- `UPDATE`,
- `DELETE`.

Dlaczego?

Bo baza musi aktualizowac nie tylko tabele, ale tez indeks.

## EXPLAIN

`EXPLAIN` pokazuje, jak baza planuje wykonac zapytanie.

```sql
EXPLAIN
SELECT *
FROM course.orders
WHERE order_date >= '2026-01-01';
```

Na poczatku wystarczy rozumiec, ze `EXPLAIN` pomaga sprawdzic, czy baza korzysta z indeksu.

## Najwazniejsze do zapamietania

- Indeks przyspiesza wybrane odczyty.
- Indeks moze spowalniac zapisy.
- Warto indeksowac kolumny czesto uzywane w `WHERE` i `JOIN`.
- Composite index ma sens dla czestych wzorcow filtrowania po kilku kolumnach.
- Nie dodaje sie indeksow wszedzie.
