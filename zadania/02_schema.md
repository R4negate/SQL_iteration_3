# Zadania 02 - Schema

## Zadanie 1

Stworz schemat:

```text
course_example
```
przeczytaj 

## Zadanie 2

Stworz schemat:

```text
practice
```

Uzyj wersji, ktora nie wyrzuci bledu, jezeli schemat juz istnieje.

## Zadanie 3

Napisz query, ktore pokazuje wszystkie schematy z `information_schema.schemata`.

Wynik powinien zawierac:

- `schema_name`

## Zadanie 4

Stworz tabele:

```text
practice.test_customers
```

Kolumny:

- `customer_id`
- `customer_name`

## Zadanie 5

Napisz `SELECT`, ktory czyta dane z tabeli `practice.test_customers` uzywajac pelnej nazwy tabeli.

## Zadanie 6

Usun schemat `practice` oraz `course_example` razem ze wszystkimi obiektami w srodku.

## Zadanie 7

Zastanów, dlaczego `DROP SCHEMA ... CASCADE` jest niebezpieczne.
