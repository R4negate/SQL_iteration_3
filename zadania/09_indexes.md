# Zadania 09 - Indexes

## Zadanie 1

Stworz indeks na kolumnie:

```text
course.orders(order_date)
```

## Zadanie 2

Stworz indeks na kolumnie:

```text
course.orders(customer_id)
```

## Zadanie 3

Stworz indeks na kolumnie:

```text
course.order_items(order_id)
```

## Zadanie 4

Stworz indeks na kolumnie:

```text
course.order_items(product_id)
```

## Zadanie 5

Stworz composite index na:

```text
course.orders(customer_id, order_date)
```

## Zadanie 6

Uruchom `EXPLAIN` dla zapytania filtrujacego `course.orders` po `order_date`.

## Zadanie 7

Uruchom `EXPLAIN` dla zapytania laczacego:

- `course.orders`
- `course.customers`

po `customer_id`.

## Zadanie 8

Napisz jednym zdaniem, dlaczego indeks moze przyspieszyc `SELECT`.

## Zadanie 9

Napisz jednym zdaniem, dlaczego zbyt duzo indeksow moze byc problemem.
