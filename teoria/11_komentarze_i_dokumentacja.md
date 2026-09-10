# 11 - Komentarze i dokumentacja w bazie

## Po co dokumentowac baze?

Tabela bez dokumentacji zmusza ludzi do zgadywania.

Komentarze w bazie pomagaja zrozumiec:

- co oznacza tabela,
- co oznacza kolumna,
- jaki jest grain tabeli,
- skad pochodza dane,
- do czego tabela jest uzywana.

## COMMENT ON TABLE

Komentarz do tabeli:

```sql
COMMENT ON TABLE course.orders IS
'One row represents one customer order.';
```

## COMMENT ON COLUMN

Komentarz do kolumny:

```sql
COMMENT ON COLUMN course.orders.total_amount IS
'Total order amount before additional reporting transformations.';
```

## Dokumentowanie grainu

Grain mowi, co oznacza jeden wiersz.

Przyklad:

```sql
COMMENT ON TABLE course.order_items IS
'One row represents one product line inside one order.';
```

To jest bardzo wazne w analityce.

Bez grainu nie wiadomo, czy mozna liczyc:

```sql
COUNT(*)
```

albo czy trzeba najpierw agregowac dane.

## Komentarze nie zastepuja dobrych nazw

Najpierw dobra nazwa:

```text
total_amount
```

Potem komentarz, jezeli trzeba doprecyzowac znaczenie.

Nie warto robic slabych nazw i ratowac ich komentarzem.

## Najwazniejsze do zapamietania

- Komentarze pomagaja innym ludziom zrozumiec baze.
- `COMMENT ON TABLE` opisuje tabele.
- `COMMENT ON COLUMN` opisuje kolumne.
- Dobrze opisany grain tabeli zmniejsza ryzyko blednych raportow.
- Dokumentacja jest czescia pracy data engineera.
