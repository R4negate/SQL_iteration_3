# Zadania - SQL iteration 3 DDL

Zadania dotycza DDL, czyli tworzenia i zmieniania struktury bazy danych.

Pracujemy na schemacie:

```text
course
```

Jezeli chcesz zaczac od czystego stanu, uzyj:

```sql
DROP SCHEMA IF EXISTS course CASCADE;
CREATE SCHEMA course;
```

Uwaga: `DROP SCHEMA ... CASCADE` usuwa schemat razem ze wszystkimi obiektami w srodku.

## Kolejnosc zadan

1. `01_czym_jest_ddl.md`
2. `02_schema.md`
3. `03_create_table.md`
4. `04_typy_danych.md`
5. `05_constraints_primary_foreign_key.md`
6. `06_alter_table.md`
7. `07_drop_truncate_delete_roznice.md`
8. `08_create_table_as_select.md`
9. `09_indexes.md`
10. `10_views_materialized_views.md`
11. `11_komentarze_i_dokumentacja.md`
12. `12_zadania_przekrojowe.md`
