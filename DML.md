# DML - Data Manipulation Language

``` sql

BEGIN;

SAVEPOINT ponto1;
/* 1.0 -- INSERIR TABELA ---------------------------------------------------------------------------------------------------------------*/
INSERT INTO cliente (ativo) values
(TRUE), (TRUE), (FALSE), (TRUE), (FALSE), (TRUE);

/* 1.1 -- INSERINDO EM PESSOA FISICA */
INSERT INTO pessoa_fisica (id, nome, cpf, nascimento)
VALUES (1, 'Leonardo Rodrigues', '124.456.789-00', '2003-01-24'),
       (2, 'Lucas Ianovski', '125.456.789-00', '1998-08-01'),
       (3, 'Nicolas Cadique', '126.456.789-00', '2002-07-28');

/* 1.2 -- INSERINDO EM PESSOA JURIDICA */
INSERT INTO pessoa_juridica (id, nome, razao_social, cnpj)
VALUES (4, 'Kaualcáida', 'Kauan Cáida.ltda', '91.739.439/0001-96'),
       (5, 'Matias Trikas', 'São Matias Roscas.ltda', '22.444.433/0001-79'),
       (6, 'André Judas', 'Talarica Quimicas.ltda', '05.070.470/0001-45');

/* 2 -- INSERINDO EM QUARTOS */
INSERT INTO hotel (id, nome_fantasia)
VALUES(1, 'Hotel Splannate');

INSERT INTO quarto (hotel_id, preco, nome, tipo, capacidade, liberado)
VALUES (1, 2.150, 'Suite meia noite', 'Suite', 40, FALSE),
       (1, 1.500, 'premium night', 'Padrao', 20, FALSE),
       (1, 2.000, 'Suite manhã', 'Suite', 30, TRUE);

/* 3 -- INSERÇÃO COM DO ... BEGIN*/
DO $$
DECLARE
    id INT;
BEGIN
    -- Cliente 1
    INSERT INTO cliente (ativo) VALUES (true) RETURNING id INTO id;
    INSERT INTO pessoa_fisica (id, nome, cpf, nascimento)
    VALUES (id, 'João Machado', '123.456.789-00', '1970-04-23');
    INSERT INTO endereco (cliente_id, logradouro, numero, cidade, estado, cep, tipo)
    VALUES (id, 'Rua das Flores', '100', 'São Paulo', 'SP', '01000-000', 'Residencial');
    INSERT INTO telefone (cliente_id, ddd, numero, tipo)
    VALUES (id, '11', '912345678', 'Movel');
    INSERT INTO email (cliente_id, email)
    VALUES (id, 'joao@email.com');

    -- Cliente 2
    INSERT INTO cliente (ativo) VALUES (true) RETURNING id INTO id;
    INSERT INTO pessoa_fisica (id, nome, cpf, nascimento)
    VALUES (id, 'Maria Souza', '987.654.321-00', '1985-08-12');
    INSERT INTO endereco (cliente_id, logradouro, numero, cidade, estado, cep, tipo)
    VALUES (id, 'Rua dos Sonhos', '200', 'Rio de Janeiro', 'RJ', '22000-000', 'Residencial');
    INSERT INTO telefone (cliente_id, ddd, numero, tipo)
    VALUES (id, '21', '998877665', 'Fixo');
    INSERT INTO email (cliente_id, email)
    VALUES (id, 'maria@email.com');

    -- Cliente 3
    INSERT INTO cliente (ativo) VALUES (true) RETURNING id INTO id;
    INSERT INTO pessoa_fisica (id, nome, cpf, nascimento)
    VALUES (id, 'Carlos Lima', '111.222.333-44', '1992-01-30');
    INSERT INTO endereco (cliente_id, logradouro, numero, cidade, estado, cep, tipo)
    VALUES (id, 'Av. Central', '300', 'Belo Horizonte', 'MG', '31000-000', 'Residencial');
    INSERT INTO telefone (cliente_id, ddd, numero, tipo)
    VALUES (id, '31', '934567890', 'Movel');
    INSERT INTO email (cliente_id, email)
    VALUES (id, 'carlos@email.com');
END $$;

/* 4 UPDATE ------------------------------------------------------------------------------------------------------------*/

UPDATE pessoa_fisica
SET nome = 'João Pedro da Silva'
WHERE cpf = '123.456.789-00';

UPDATE cliente
SET ativo = FALSE
WHERE id = (
    SELECT id FROM pessoa_juridica
    WHERE cnpj = '05.070.470/0001-45'
);

/* 5 DELETE ------------------------------------------------------------------------------------------------------------*/

DELETE FROM cliente
WHERE id = (
    SELECT id FROM pessoa_fisica WHERE cpf = '111.222.333-44'
);

-- CASO OS SCRIPTS SEJAM EXECUTADOS COM SUCESSO:
COMMIT;

-- CASO OCORRA UM RESULTADO NÃO ESPERADO:
ROLLBACK TO SAVEPOINT ponto1;

```