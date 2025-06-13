# Data Query Language

``` sql

SELECT pf.nome, pf.cpf
FROM pessoa_fisica pf
JOIN cliente c ON pf.id = c.id
JOIN reserva r ON c.id = r.cliente_id
WHERE r.data_entrada = '2025-06-10';


SELECT e.*, pf.nome, pf.cpf
FROM endereco e
JOIN cliente c ON e.cliente_id = c.id
JOIN pessoa_fisica pf ON c.id = pf.id
WHERE pf.cpf = '123.456.789-00';



```