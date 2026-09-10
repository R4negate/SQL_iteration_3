# Zadania 06 - ALTER TABLE

## Zadanie 1

Dodaj do tabeli `course.customers` kolumne:

```text
phone_number
```

Typ danych: `TEXT`.

## Zadanie 2

Dodaj do tabeli `course.orders` kolumne:

```text
currency
```

Typ danych: `TEXT`.

Domyslna wartosc: `PLN`.

## Zadanie 3

Sprawdz w `information_schema.columns`, czy kolumna `currency` zostala dodana.

## Zadanie 4

Dodaj do tabeli `course.products` constraint, ktory pilnuje, ze `base_price >= 0`.

## Zadanie 5

Dodaj do tabeli `course.orders` constraint, ktory pilnuje, ze `total_amount >= 0`.

## Zadanie 6

Zmien nazwe kolumny `phone_number` w `course.customers` na:

```text
phone
```

## Zadanie 7

Usun kolumne `phone` z tabeli `course.customers`.

## Zadanie 8

Napisz jednym zdaniem, dlaczego `DROP COLUMN` jest operacja ryzykowna.

## Zadanie 9

Zmien typ kolumny `total_amount` w `course.orders` na:

```text
NUMERIC(12, 2)
```

## Zadanie 10

Usun constraint sprawdzajacy `total_amount >= 0`.

Najpierw sprawdz jego nazwe, jezeli nie pamietasz jak sie nazywa.
