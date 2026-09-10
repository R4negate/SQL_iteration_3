# 03 - CREATE TABLE

## Do czego sluzy CREATE TABLE?

`CREATE TABLE` tworzy nowa tabele w bazie danych.

Tabela sklada sie z:

- nazwy,
- kolumn,
- typow danych,
- opcjonalnych constraintow.

## Podstawowa skladnia

```sql
CREATE TABLE schema_name.table_name (
    column_name DATA_TYPE,
    column_name DATA_TYPE
);
```

Przyklad:

```sql
CREATE TABLE course.customers (
    customer_id INT,
    customer_name TEXT,
    email TEXT
);
```

## Kolejnosc kolumn

Kolejnosc kolumn nie zmienia logiki bazy, ale wplywa na czytelnosc.

Dobra praktyka:

- najpierw identyfikator,
- potem najwazniejsze pola biznesowe,
- potem daty,
- potem pola techniczne.

Przyklad:

```sql
CREATE TABLE course.orders (
    order_id INT,
    customer_id INT,
    order_date DATE,
    status TEXT,
    total_amount NUMERIC(10, 2)
);
```

## CREATE TABLE IF NOT EXISTS

```sql
CREATE TABLE IF NOT EXISTS course.customers (
    customer_id INT,
    customer_name TEXT
);
```

Ta wersja nie wyrzuci bledu, jezeli tabela juz istnieje.

Uwaga: jezeli tabela istnieje, PostgreSQL nie przebuduje jej automatycznie wedlug nowej definicji.

## Kolejnosc tworzenia tabel

Jezeli tabele maja foreign key, kolejnosc ma znaczenie.

Najpierw tworzymy tabele nadrzedne:

- `customers`,
- `products`.

Potem tabele zalezne:

- `orders`,
- `order_items`.

Dlaczego?

Bo `orders.customer_id` wskazuje na `customers.customer_id`, wiec tabela `customers` musi juz istniec.

## Najwazniejsze do zapamietania

- `CREATE TABLE` tworzy strukture tabeli.
- Kazda kolumna musi miec typ danych.
- Dobra tabela ma jasny grain, czyli wiadomo, co oznacza jeden wiersz.
- Przy foreign key najpierw tworzymy tabele nadrzedne, potem zalezne.
