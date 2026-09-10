# 02 - Schema

## Czym jest schema?

Schema to logiczny folder w bazie danych.

Pozwala grupowac tabele i inne obiekty.

Przyklad pelnej nazwy tabeli:

```text
course.customers
```

Tutaj:

- `course` to schema,
- `customers` to tabela.

## Po co uzywac schematow?

Schematy pomagaja utrzymac porzadek.

Bez schematow wszystkie tabele laduja w jednym miejscu.

W projektach data engineeringowych czesto spotkasz schematy:

- `raw` - dane surowe,
- `staging` - dane lekko oczyszczone,
- `analytics` - dane gotowe do raportow,
- `course` - schemat treningowy w naszych lekcjach.

## Tworzenie schematu

```sql
CREATE SCHEMA course;
```

Bezpieczniejsza wersja:

```sql
CREATE SCHEMA IF NOT EXISTS course;
```

Ta wersja nie wyrzuci bledu, jezeli schema juz istnieje.

## Usuwanie schematu

```sql
DROP SCHEMA course;
```

Jezeli schema zawiera tabele, PostgreSQL moze zablokowac usuniecie.

Wtedy istnieje mocniejsza wersja:

```sql
DROP SCHEMA course CASCADE;
```

`CASCADE` usuwa schemat razem z obiektami w srodku.

Trzeba z tym uwazac.

## RESTRICT

`RESTRICT` oznacza: nie usuwaj, jezeli sa zalezne obiekty.

```sql
DROP SCHEMA course RESTRICT;
```

To jest bezpieczniejsze zachowanie.

## Search path

PostgreSQL ma ustawienie `search_path`.

Okresla ono, w jakich schematach baza szuka tabel, jezeli nie podasz pelnej nazwy.

Lepiej pisac pelne nazwy:

```sql
SELECT *
FROM course.customers;
```

niz:

```sql
SELECT *
FROM customers;
```

Pelna nazwa jest bardziej czytelna i mniej podatna na pomylki.

## Najwazniejsze do zapamietania

- Schema to logiczny folder na obiekty bazy.
- Pelna nazwa tabeli ma format `schema.table`.
- `CREATE SCHEMA IF NOT EXISTS` jest bezpieczne przy ponownym uruchamianiu skryptu.
- `DROP SCHEMA ... CASCADE` usuwa duzo rzeczy naraz.
- W projektach danych schematy pomagaja oddzielac warstwy danych.
