# Zadania 08 - CREATE TABLE AS SELECT

## Zadanie 1

Stworz tabele `course.customers_copy` jako kopie calej tabeli `course.customers`.

## Zadanie 2

Stworz tabele `course.paid_orders` zawierajaca tylko zamowienia ze statusem:

```text
paid
```

## Zadanie 3

Stworz tabele `course.orders_with_customers` na podstawie joina:

- `course.orders`
- `course.customers`

Wynik powinien zawierac:

- `order_id`
- `order_date`
- `status`
- `total_amount`
- `customer_id`
- `customer_name`
- `country`

## Zadanie 4

Stworz tabele `course.sales_by_country` z wyniku agregacji.

Wynik powinien zawierac:

- `country`
- `orders_count`
- `total_revenue`

## Zadanie 5

Stworz pusta tabele `course.empty_orders_report` na podstawie zapytania do `course.orders`.

Tabela ma miec kolumny:

- `order_id`
- `order_date`
- `status`
- `total_amount`

Ale nie powinna zawierac zadnych wierszy.

## Zadanie 6

Sprawdz w `information_schema.table_constraints`, czy tabela `course.customers_copy` ma primary key.

Napisz jednym zdaniem, co zauwazasz.
