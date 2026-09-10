# 08 - CREATE TABLE AS SELECT

## Czym jest CTAS?

`CREATE TABLE AS SELECT` czesto zapisuje sie jako CTAS.

To polecenie tworzy nowa tabele na podstawie wyniku zapytania.

Podstawowa skladnia:

```sql
CREATE TABLE new_table AS
SELECT ...
FROM old_table;
```

## Przyklad

```sql
CREATE TABLE course.paid_orders AS
SELECT
    order_id,
    customer_id,
    order_date,
    total_amount
FROM course.orders
WHERE status = 'paid';
```

Powstaje nowa tabela `course.paid_orders` z danymi z zapytania.

## Kiedy uzywac CTAS?

CTAS jest przydatny, gdy chcesz:

- stworzyc snapshot danych,
- stworzyc tabele analityczna,
- skopiowac fragment danych,
- przygotowac tabele pod raport,
- pokazac prosta wersje warstwy silver/gold.

## CTAS a constraints

Wazne: CTAS tworzy tabele z danych, ale zwykle nie przenosi wszystkich constraintow z tabel zrodlowych.

Przyklad:

```sql
CREATE TABLE course.customers_copy AS
SELECT *
FROM course.customers;
```

Taka kopia moze nie miec `PRIMARY KEY`, `FOREIGN KEY`, `CHECK` itd.

Dlatego do tabel produkcyjnych czesto lepiej najpierw zrobic `CREATE TABLE` z pelna struktura, a potem `INSERT INTO ... SELECT`.

## CREATE TABLE AS bez danych

Mozna stworzyc tabele z taka sama struktura wyniku, ale bez wierszy:

```sql
CREATE TABLE course.paid_orders_empty AS
SELECT
    order_id,
    customer_id,
    order_date,
    total_amount
FROM course.orders
WHERE 1 = 0;
```

`WHERE 1 = 0` sprawia, ze zapytanie nie zwraca zadnych rekordow.

## CTAS vs INSERT INTO SELECT

CTAS tworzy nowa tabele:

```sql
CREATE TABLE course.orders_copy AS
SELECT *
FROM course.orders;
```

`INSERT INTO SELECT` laduje dane do tabeli, ktora juz istnieje:

```sql
INSERT INTO course.orders_copy
SELECT *
FROM course.orders;
```

## Najwazniejsze do zapamietania

- CTAS tworzy tabele z wyniku zapytania.
- CTAS jest szybki i wygodny do tabel analitycznych.
- CTAS nie jest najlepszy, jezeli musisz miec pelne constraints.
- Do produkcyjnych tabel czesto lepsze jest `CREATE TABLE` + `INSERT INTO SELECT`.
