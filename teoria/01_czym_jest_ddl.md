# 01 - Czym jest DDL

SQL mozna podzielic na kilka grup.

Najwazniejsze z nich na tym etapie to:

- DQL - czytanie danych,
- DML - zmienianie danych,
- DDL - tworzenie i zmienianie struktury bazy.

## DQL

DQL to Data Query Language.

Najwazniejsze slowo:

```sql
SELECT
```

DQL odpowiada za odczyt danych.

Przyklad:

```sql
SELECT *
FROM course.customers;
```

## DML

DML to Data Manipulation Language.

Najwazniejsze slowa:

```sql
INSERT
UPDATE
DELETE
```

DML odpowiada za zmiane danych w istniejacych tabelach.

## DDL

DDL to Data Definition Language.

Najwazniejsze slowa:

```sql
CREATE
ALTER
DROP
TRUNCATE
```

DDL odpowiada za strukture bazy danych.

Przyklady:

```sql
CREATE SCHEMA course;
```

```sql
CREATE TABLE course.customers (
    customer_id INT,
    customer_name TEXT
);
```

```sql
ALTER TABLE course.customers
ADD COLUMN email TEXT;
```

```sql
DROP TABLE course.customers;
```

## DDL z perspektywy data engineera

Data engineer uzywa DDL, kiedy przygotowuje miejsce na dane.

Przyklady:

- tworzy tabele pod dane z API,
- tworzy tabele pod dane z plikow CSV,
- tworzy warstwy danych, np. `raw`, `staging`, `analytics`,
- dodaje constraints, zeby blokowac zle dane,
- dodaje indeksy, zeby raporty dzialaly szybciej,
- tworzy widoki dla analitykow.

DDL odpowiada za projektowanie tego, jak dane maja byc przechowywane.

## DDL jest niebezpieczne

`SELECT` tylko czyta dane.

DDL zmienia sama strukture bazy.

Dlatego przy DDL trzeba uwazac na:

- `DROP`,
- `TRUNCATE`,
- `ALTER TABLE`,
- `CASCADE`.

Jedno polecenie DDL moze usunac tabele albo zmienic jej strukture.

