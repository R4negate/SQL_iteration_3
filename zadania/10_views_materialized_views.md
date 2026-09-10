# Zadania 10 - VIEW i MATERIALIZED VIEW

## Zadanie 1

Stworz widok:

```text
course.v_orders_with_customers
```

Widok powinien zawierac:

- `order_id`
- `order_date`
- `status`
- `total_amount`
- `customer_id`
- `customer_name`
- `country`

## Zadanie 2

Napisz `SELECT`, ktory czyta dane z widoku `course.v_orders_with_customers`.

## Zadanie 3

Stworz widok:

```text
course.v_order_items_with_products
```

Widok powinien laczyc:

- `course.order_items`
- `course.products`

Wynik powinien zawierac:

- `order_id`
- `order_item_id` albo `line_number`
- `product_id`
- `product_name`
- `category`
- `quantity`
- `unit_price`
- `line_value`

## Zadanie 4

Stworz materialized view:

```text
course.mv_sales_by_country
```

Wynik powinien zawierac:

- `country`
- `orders_count`
- `total_revenue`

## Zadanie 5

Odswiez materialized view `course.mv_sales_by_country`.

## Zadanie 6

Napisz jednym zdaniem, czym rozni sie `VIEW` od `MATERIALIZED VIEW`.

## Zadanie 7

Napisz jednym zdaniem, kiedy lepiej uzyc `VIEW`.

## Zadanie 8

Napisz jednym zdaniem, kiedy lepiej uzyc `MATERIALIZED VIEW`.

## Zadanie 9

Usun widok `course.v_orders_with_customers`.

## Zadanie 10

Usun materialized view `course.mv_sales_by_country`.
