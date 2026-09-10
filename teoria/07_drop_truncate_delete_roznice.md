# 07 - DROP, TRUNCATE i DELETE

## Trzy podobne, ale rozne operacje

W SQL sa trzy polecenia, ktore czesto sie myla:

- `DELETE`
- `TRUNCATE`
- `DROP`

Kazde z nich usuwa cos innego.

## DELETE

`DELETE` usuwa wiersze z tabeli.

Tabela zostaje.

```sql
DELETE FROM course.orders
WHERE order_id = 1;
```

Mozna usunac wybrane rekordy przez `WHERE`.

Bez `WHERE`:

```sql
DELETE FROM course.orders;
```

usuniesz wszystkie wiersze, ale tabela nadal istnieje.

## TRUNCATE

`TRUNCATE` szybko usuwa wszystkie wiersze z tabeli.

```sql
TRUNCATE TABLE course.orders;
```

Nie uzywa sie tutaj `WHERE`.

`TRUNCATE` jest dobre do czyszczenia tabel technicznych, stagingowych albo tymczasowych.

## DROP

`DROP` usuwa caly obiekt.

```sql
DROP TABLE course.orders;
```

Po `DROP TABLE` nie ma ani danych, ani struktury tabeli.

## CASCADE

`CASCADE` usuwa tez obiekty zalezne.

```sql
DROP TABLE course.customers CASCADE;
```

To moze usunac widoki, constrainty albo inne zaleznosci.

`CASCADE` jest wygodne w nauce, ale niebezpieczne w prawdziwej bazie.

## RESTRICT

`RESTRICT` nie pozwala usunac obiektu, jezeli cos od niego zalezy.

```sql
DROP TABLE course.customers RESTRICT;
```

To jest ostrozniejsze zachowanie.

## Porownanie

| Polecenie | Co usuwa | Czy tabela zostaje? | Czy mozna uzyc WHERE? |
|---|---|---|---|
| `DELETE` | wybrane wiersze albo wszystkie wiersze | tak | tak |
| `TRUNCATE` | wszystkie wiersze | tak | nie |
| `DROP` | caly obiekt | nie | nie |

## Najwazniejsze do zapamietania

- `DELETE` usuwa dane.
- `TRUNCATE` szybko czysci cala tabele.
- `DROP` usuwa tabele jako obiekt.
- `CASCADE` moze usunac wiecej, niz planujesz.
- Przy nauce mozna resetowac baze przez `DROP SCHEMA ... CASCADE`, ale trzeba wiedziec, co to robi.
