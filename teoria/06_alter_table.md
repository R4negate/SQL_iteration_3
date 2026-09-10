# 06 - ALTER TABLE

## Do czego sluzy ALTER TABLE?

`ALTER TABLE` sluzy do zmiany istniejacej tabeli.

Za jego pomoca mozna:

- dodac kolumne,
- usunac kolumne,
- zmienic nazwe kolumny,
- zmienic nazwe tabeli,
- zmienic typ danych,
- dodac constraint,
- usunac constraint.

## Dodanie kolumny

```sql
ALTER TABLE course.customers
ADD COLUMN phone_number TEXT;
```

Od tego momentu tabela ma nowa kolumne.

Istniejace rekordy dostana w tej kolumnie `NULL`, chyba ze podasz `DEFAULT`.

## Dodanie kolumny z DEFAULT

```sql
ALTER TABLE course.orders
ADD COLUMN currency TEXT DEFAULT 'PLN';
```

Nowe rekordy bez podanej waluty dostana `PLN`.

## Usuniecie kolumny

```sql
ALTER TABLE course.customers
DROP COLUMN phone_number;
```

To usuwa kolumne razem z danymi w tej kolumnie.

## Zmiana nazwy kolumny

```sql
ALTER TABLE course.orders
RENAME COLUMN total_amount TO order_total;
```

Po takiej zmianie stare zapytania uzywajace `total_amount` przestana dzialac.

## Zmiana nazwy tabeli

```sql
ALTER TABLE course.customers
RENAME TO clients;
```

Nowa nazwa tabeli to:

```text
course.clients
```

## Zmiana typu danych

```sql
ALTER TABLE course.orders
ALTER COLUMN total_amount TYPE NUMERIC(12, 2);
```

Zmiana typu jest mozliwa tylko wtedy, gdy PostgreSQL potrafi przekonwertowac istniejace dane.

Czasem trzeba uzyc `USING`:

```sql
ALTER TABLE course.orders
ALTER COLUMN total_amount TYPE NUMERIC(12, 2)
USING total_amount::NUMERIC(12, 2);
```

## Dodanie constraintu

```sql
ALTER TABLE course.orders
ADD CONSTRAINT chk_orders_total_amount
CHECK (total_amount >= 0);
```

PostgreSQL sprawdzi istniejace dane.

Jezeli w tabeli sa dane lamanace constraint, dodanie constraintu sie nie uda.

## Usuniecie constraintu

```sql
ALTER TABLE course.orders
DROP CONSTRAINT chk_orders_total_amount;
```

Do tego potrzebujesz znac nazwe constraintu.

## Bezpieczna praca z ALTER TABLE

Przed `ALTER TABLE` warto sprawdzic:

- czy tabela istnieje,
- jakie ma kolumny,
- czy sa dane, ktore zlamia nowy constraint,
- czy inne zapytania uzywaja zmienianej kolumny,
- czy zmiana nie zepsuje widokow albo raportow.

## Najwazniejsze do zapamietania

- `ALTER TABLE` zmienia istniejaca tabele.
- Dodanie kolumny jest zwykle bezpieczne.
- Usuniecie kolumny usuwa dane.
- Zmiana nazwy kolumny moze popsuc istniejace query.
- Dodanie constraintu moze nie przejsc, jezeli stare dane sa niepoprawne.
