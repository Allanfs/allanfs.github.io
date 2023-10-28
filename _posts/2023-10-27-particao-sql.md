---
layout: post
title:  "Aprimorando consultas de logs com partição no Postgres"
date:   2023-10-27 21:07:00 -0300
categories: sql postgres
---
Aqui falarei sobre _window functions_, como descobri que existiam e como usei.

## O problema
Começa quando preciso buscar informações sobre a primeira ocorrência de uma 
transação em uma tabela de log, que registra os status da mesma.

Nesse exemplo, vou buscar todas as transações cujo primeiro status é pendente.

| id  | transacao_id | status      | data                | outra_info |
| --- | ------------ | ----------- | ------------------- | ---------- |
| 1   | 10           | PENDENTE    | 2023-10-27 11:00:00 | x          |
| 2   | 10           | PROCESSANDO | 2023-10-27 11:10:00 | y          |
| 3   | 10           | CONCLUIDO   | 2023-10-27 11:35:00 | z          |
| 4   | 11           | PENDENTE    | 2023-10-27 12:40:00 | y          |
| 5   | 11           | PROCESSANDO | 2023-10-27 12:46:00 | z          |
| 5   | 12           | REJEITADO   | 2023-10-27 11:51:00 | y          |

## Tentativas

Nas primeiras consultas tentei ordernar os registros cronologicamente e usar
`limit 1`, mas isso não trazia o que eu queria.

Pensei em dividir o trabalho com a aplicação, fazendo com que ela consultasse o 
primeiro log de cada transação, mas logo vi que isso não seria muito saudável
para o sistema ir e voltar ao banco de dados tantas vezes.

Depois de algumas pesquisas encontrei a função `row_number()` que trazia exatamente
a informação que eu queria: uma ordenação para cada registro de cada transação.

## Os resultados

| id  | transacao_id | status      | data                | outra_info | numb |
| --- | ------------ | ----------- | ------------------- | ---------- | ---- |
| 1   | 10           | PENDENTE    | 2023-10-27 11:00:00 | x          | 1    |
| 2   | 10           | PROCESSANDO | 2023-10-27 11:10:00 | y          | 2    |
| 3   | 10           | CONCLUIDO   | 2023-10-27 11:35:00 | z          | 3    |
| 4   | 11           | PENDENTE    | 2023-10-27 12:40:00 | y          | 1    |
| 5   | 11           | PROCESSANDO | 2023-10-27 12:46:00 | z          | 2    |
| 5   | 12           | REJEITADO   | 2023-10-27 11:51:00 | y          | 1    |

Veja que _para cada transaction\_id_ existe um sequencial identicando elas.
Assim eu consigo selecionar especificamente o primeiro registro de cada transação
em uma subquery ou com uma cláusula `WITH`.

```sql
with base as (   
    SELECT *, row_number() over (PARTITION BY transaction_id ORDER BY data) as numb
    FROM transacao_logs
)
SELECT * FROM base
WHERE numb = 1
AND status = 'PENDENTE'
```


| id  | transacao_id | status      | data                | outra_info | numb |
| --- | ------------ | ----------- | ------------------- | ---------- | ---- |
| 1   | 10           | PENDENTE    | 2023-10-27 11:00:00 | x          | 1    |
| 4   | 11           | PENDENTE    | 2023-10-27 12:40:00 | y          | 1    |

## Quer saber mais?

- https://www.devmedia.com.br/postgresql-partition-trabalhando-com-particoes-no-postgresql/33969
- https://medium.com/nerd-for-tech/partition-by-clause-in-postgresql-the-easy-way-10772ac41dfd
- https://www.postgresql.org/docs/current/tutorial-window.html
- https://www.postgresql.org/docs/current/queries-with.html