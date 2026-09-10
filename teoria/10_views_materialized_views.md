# 10 - VIEW i MATERIALIZED VIEW

## Czym jest VIEW?

`VIEW` to zapisane zapytanie SQL.

Widok zachowuje sie podobnie do tabeli, ale sam nie przechowuje danych.

Przyklad:

```sql
CREATE VIEW course.v_orders_with_customers AS
SELECT
    o.order_id,
    o.order_date,
    o.status,
    o.total_amount,
    c.customer_id,
    c.customer_name,
    c.country
FROM course.orders o
JOIN course.customers c
    ON o.customer_id = c.customer_id;
```

Potem mozna pisac:

```sql
SELECT *
FROM course.v_orders_with_customers;
```

Baza wykona zapytanie zapisane w widoku.

## Po co sa widoki?

Widoki pomagaja:

- uproscic trudne zapytania,
- ukryc szczegoly joinow,
- stworzyc gotowy obiekt dla analityka,
- ujednolicic logike raportowa,
- ograniczyc dostep do wybranych kolumn.

## VIEW nie przechowuje danych

Widok pokazuje aktualne dane z tabel zrodlowych.

Jezeli zmienisz dane w `course.orders`, widok pokaze nowy wynik przy kolejnym odczycie.

## Czym jest MATERIALIZED VIEW?

`MATERIALIZED VIEW` to zapisany wynik zapytania.

Czyli materialized view przechowuje dane fizycznie.

Przyklad:

```sql
CREATE MATERIALIZED VIEW course.mv_sales_by_country AS
SELECT
    c.country,
    COUNT(o.order_id) AS orders_count,
    SUM(o.total_amount) AS total_revenue
FROM course.customers c
JOIN course.orders o
    ON c.customer_id = o.customer_id
GROUP BY c.country;
```

## Odwiezenie materialized view

Materialized view nie aktualizuje sie samo po zmianie danych zrodlowych.

Trzeba je odswiezyc:

```sql
REFRESH MATERIALIZED VIEW course.mv_sales_by_country;
```

## VIEW vs MATERIALIZED VIEW

| Cecha | VIEW | MATERIALIZED VIEW |
|---|---|---|
| Przechowuje dane? | nie | tak |
| Pokazuje zawsze aktualne dane? | tak | nie, wymaga refresh |
| Moze przyspieszyc ciezki raport? | nie zawsze | czesto tak |
| Wymaga odswiezania? | nie | tak |

## Kiedy uzyc VIEW?

Uzyj `VIEW`, gdy:

- chcesz uproscic zapytanie,
- dane maja byc zawsze aktualne,
- zapytanie nie jest bardzo ciezkie.

## Kiedy uzyc MATERIALIZED VIEW?

Uzyj `MATERIALIZED VIEW`, gdy:

- zapytanie jest ciezkie,
- raport nie musi byc aktualny co sekunde,
- chcesz zapisac wynik agregacji.

## Najwazniejsze do zapamietania

- `VIEW` to zapisane query.
- `VIEW` nie przechowuje danych.
- `MATERIALIZED VIEW` przechowuje wynik query.
- `MATERIALIZED VIEW` trzeba odswiezac.
- Widoki sa bardzo przydatne do raportow i analityki.
