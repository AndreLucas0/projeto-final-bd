# DML - Data Manipulation Language

``` sql

BEGIN;

SAVEPOINT ponto1;
/* 1. INSERIR TABELA ---------------------------------------------------------------------------------------------------------------*/
INSERT INTO cliente DEFAULT VALUES;

/* 1-- INSERINDO EM PESSOA FISICA */
INSERT INTO pessoa_fisica (nome, cpf, nascimento)
VALUES ('Leonardo Rodrigues', '123.456.789-00', '2003-01-24'),
       ('Lucas Ianovski', '123.456.789-00', '1998-08-01'),
       ('Nicolas Cadique', '123.456.789-00', '2002-07-28');

/* 1-- INSERINDO EM PESSOA JURIDICA */
INSERT INTO pessoa_juridica (nome, Razão social, cnpj)
VALUES ('Kaualcáida', 'Kauan Cáida.ltda', '91.739.439/0001-96'),
       ('Matias Trikas', 'São Matias Roscas.ltda', '22.444.433/0001-79'),
       ('André Judas', 'Talarica Quimicas.ltda', '05.070.470/0001-45');

/* 2-- INSERINDO EM QUARTOS */
INSERT INTO quarto (preco, nome, tipo, capacidade, liberado)
VALUES ('R$2.150', 'Suite meia noite', 'Luxo', '40', 'Não'),
       ('R$1.500', 'premium night', 'Premium', '20', 'Não'),
       ('R$2.000', 'Suite manhã', 'Luxo', '30', 'Sim');

/* 3-- INSERÇÃO COM DO ... BEGIN*/
DO $$
DECLARE
    id INT;
BEGIN
    -- Cliente 1
    INSERT INTO cliente (ativo) VALUES (true) RETURNING id INTO id;
    INSERT INTO pessoa_fisica (nome, cpf, nascimento)
    VALUES ('João Machado', '123.456.789-00', '1970-04-23');
    INSERT INTO endereco (cliente_id, logradouro, numero, cidade, estado, cep, tipo)
    VALUES (1, 'Rua das Flores', '100', 'São Paulo', 'SP', '01000-000', 'Residencial');
    INSERT INTO telefone (cliente_id, ddd, numero, tipo)
    VALUES (1, '11', '912345678', 'Movel');
    INSERT INTO email (cliente_id, email)
    VALUES (1, 'joao@email.com');

    -- Cliente 2
    INSERT INTO cliente (ativo) VALUES (true) RETURNING id INTO id;
    INSERT INTO pessoa_fisica (nome, cpf, nascimento)
    VALUES ('Maria Souza', '987.654.321-00', '1985-08-12');
    INSERT INTO endereco (cliente_id, logradouro, numero, cidade, estado, cep, tipo)
    VALUES (2, 'Rua dos Sonhos', '200', 'Rio de Janeiro', 'RJ', '22000-000', 'Residencial');
    INSERT INTO telefone (cliente_id, ddd, numero, tipo)
    VALUES (2, '21', '998877665', 'Fixo');
    INSERT INTO email (cliente_id, email)
    VALUES (2, 'maria@email.com');

    -- Cliente 3
    INSERT INTO cliente (ativo) VALUES (true) RETURNING id INTO id;
    INSERT INTO pessoa_fisica (nome, cpf, nascimento)
    VALUES ('Carlos Lima', '111.222.333-44', '1992-01-30');
    INSERT INTO endereco (cliente_id, logradouro, numero, cidade, estado, cep, tipo)
    VALUES (3, 'Av. Central', '300', 'Belo Horizonte', 'MG', '31000-000', 'Residencial');
    INSERT INTO telefone (cliente_id, ddd, numero, tipo)
    VALUES (3, '31', '934567890', 'Movel');
    INSERT INTO email (cliente_id, email)
    VALUES (3, 'carlos@email.com');
END $$;

/* 2. UPDATE ------------------------------------------------------------------------------------------------------------*/

UPDATE pessoa_fisica
SET nome = 'João Pedro da Silva'
WHERE cpf = '123.456.789-00';

UPDATE cliente
SET ativo = FALSE
WHERE id = (
    SELECT id FROM pessoa_juridica
    WHERE cnpj = '05.070.470/0001-45'
);

/* 3. DELETE ------------------------------------------------------------------------------------------------------------*/

DELETE FROM cliente
WHERE id = (
    SELECT id FROM pessoa_fisica WHERE cpf = '111.222.333-44'
);

// CASO DE CERTO
COMMIT;

//CASO DE ALGO ERRADO
ROLLBACK TO SAVEPOINT ponto1;

```