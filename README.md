# SQL iteration 3 - DDL

Ta iteracja dotyczy DDL, czyli czesci SQL odpowiedzialnej za tworzenie i zmienianie struktury bazy danych.

W poprzednich iteracjach:

- DQL uczyl czytania danych przez `SELECT`,
- DML uczyl zmieniania danych przez `INSERT`, `UPDATE`, `DELETE`,
- DDL uczy projektowania obiektow bazy: schematow, tabel, relacji, constraintow, indeksow i widokow.

## Na czym pracujemy

Pracujemy na schemacie:

```text
course
```

Docelowe tabele:

- `course.customers`
- `course.products`
- `course.orders`
- `course.order_items`

W tej iteracji zaczynasz od pustej bazy i uczysz sie samodzielnie tworzyc strukture.

## Kolejnosc nauki

1. `teoria/01_czym_jest_ddl.md`
2. `teoria/02_schema.md`
3. `teoria/03_create_table.md`
4. `teoria/04_typy_danych.md`
5. `teoria/05_constraints_primary_foreign_key.md`
6. `teoria/06_alter_table.md`
7. `teoria/07_drop_truncate_delete_roznice.md`
8. `teoria/08_create_table_as_select.md`
9. `teoria/09_indexes.md`
10. `teoria/10_views_materialized_views.md`
11. `teoria/11_komentarze_i_dokumentacja.md`
12. `zadania/12_zadania_przekrojowe.md`

## Najwazniejsza mysl

DDL odpowiada na pytanie:

```text
Jak baza danych ma byc zbudowana, zeby dane byly poprawne, czytelne i wygodne do analizy?
```

Data engineer nie tylko pobiera i przetwarza dane. Data engineer bardzo czesto projektuje tez miejsca, do ktorych dane trafiaja.
