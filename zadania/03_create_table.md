# Zadania 03 - CREATE TABLE

## Zadanie 1

Stworz schemat `course`, jezeli jeszcze nie istnieje.

## Zadanie 2

Stworz tabele `course.customers` z kolumnami:

- `customer_id`
- `customer_name`
- `email`
- `country`
- `signup_date`
- `acquisition_channel`

Dobierz podstawowe typy danych.

## Zadanie 3

Stworz tabele `course.products` z kolumnami:

- `product_id`
- `product_name`
- `category`
- `base_price`

Dobierz podstawowe typy danych.

## Zadanie 4

Stworz tabele `course.orders` z kolumnami:

- `order_id`
- `customer_id`
- `order_date`
- `status`
- `total_amount`

Dobierz podstawowe typy danych.

## Zadanie 5

Stworz tabele `course.order_items` z kolumnami:

- `order_item_id`
- `order_id`
- `product_id`
- `quantity`
- `unit_price`

Dobierz podstawowe typy danych.

## Zadanie 6

Napisz query do `information_schema.columns`, ktore pokazuje kolumny tabeli `course.orders`.

Wynik powinien zawierac:

- `column_name`
- `data_type`

## Zadanie 7

Napisz jednym zdaniem, co oznacza jeden wiersz w tabeli `course.orders`.

## Zadanie 8

Napisz jednym zdaniem, co oznacza jeden wiersz w tabeli `course.order_items`.
