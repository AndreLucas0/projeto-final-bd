# DDL - Data Definition Language

``` sql

CREATE TABLE cliente (
    id SERIAL PRIMARY KEY,
    ativo BOOLEAN NOT NULL DEFAULT TRUE
);

CREATE TABLE hotel (
    id SERIAL PRIMARY KEY,
    nome_fantasia VARCHAR(100) NOT NULL
);

CREATE TABLE servico (
    id SERIAL PRIMARY KEY,
    tipo VARCHAR(30) NOT NULL CHECK (tipo IN ('Nenhum', 'Café da manhã', 'Almoço', 'Jantar')) DEFAULT 'Nenhum',
    preco NUMERIC(10, 2) NOT NULL
);

CREATE TABLE atendente (
    id SERIAL PRIMARY KEY,
    hotel_id INTEGER REFERENCES hotel(id) ON DELETE CASCADE,
    nome VARCHAR(100) NOT NULL,
    cpf VARCHAR(14) UNIQUE NOT NULL,
    nascimento DATE NOT NULL
);

CREATE TABLE quarto (
    id SERIAL PRIMARY KEY,
    hotel_id INTEGER REFERENCES hotel(id) ON DELETE CASCADE,
    preco NUMERIC(10, 2) NOT NULL,
    nome VARCHAR(100) NOT NULL,
    tipo VARCHAR(20) NOT NULL CHECK (tipo IN ('Suite', 'Padrao')) DEFAULT 'Padrao',
    capacidade INT NOT NULL,
    liberado BOOLEAN NOT NULL DEFAULT TRUE
);

CREATE TABLE reserva (
    id SERIAL PRIMARY KEY,
    cliente_id INTEGER REFERENCES cliente(id) ON DELETE CASCADE,
    quarto_id INTEGER REFERENCES quarto(id) ON DELETE CASCADE,
    atendente_id INTEGER REFERENCES atendente(id) ON DELETE CASCADE,
    concluido BOOLEAN NOT NULL DEFAULT FALSE,
    data_entrada DATE NOT NULL,
    data_saida DATE NOT NULL
);

CREATE TABLE pessoa_fisica (
    id INTEGER PRIMARY KEY REFERENCES cliente(id) ON DELETE CASCADE,/* chave primária e estrangeira, fazendo referencia ao campo ID do cliente e ao deletar, deleta os dados desta tabela também */
    nome VARCHAR(100) NOT NULL,
    cpf VARCHAR(14) UNIQUE NOT NULL, /* não usamos int, pq tem uma limitação de até qual números podemos usar (capacidade de armazenamento) */
    nascimento DATE NOT NULL
);

CREATE TABLE pessoa_juridica (
    id INTEGER PRIMARY KEY REFERENCES cliente(id) ON DELETE CASCADE,
    nome_fantasia VARCHAR(100) NOT NULL,
    razao_social VARCHAR(100) NOT NULL,
    cnpj VARCHAR(18) UNIQUE NOT NULL
);

CREATE TABLE endereco (
    id SERIAL PRIMARY KEY,
    cliente_id INTEGER REFERENCES cliente(id) ON DELETE CASCADE,
    hotel_id INTEGER REFERENCES hotel(id) ON DELETE CASCADE,
    logradouro VARCHAR(100) NOT NULL,
    bairro VARCHAR(100),
    numero VARCHAR(10),
    complemento VARCHAR(100),
    cep VARCHAR (9) NOT NULL,
    cidade VARCHAR (100) NOT NULL,
    pais VARCHAR (100) NOT NULL,
    estado VARCHAR(2) NOT NULL,
    tipo VARCHAR(20) NOT NULL CHECK (tipo IN ('Comercial', 'Residencial')) DEFAULT 'Residencial'
);

CREATE TABLE telefone (
    id SERIAL PRIMARY KEY,
    cliente_id INTEGER REFERENCES cliente(id) ON DELETE CASCADE,
    hotel_id INTEGER REFERENCES hotel(id) ON DELETE CASCADE,
    ddd VARCHAR(2) NOT NULL,
    numero VARCHAR(10) NOT NULL,
    tipo VARCHAR(12) NOT NULL CHECK (tipo IN ('Movel', 'Fixo', 'Recado')) DEFAULT 'Fixo'
);

CREATE TABLE email (
    id SERIAL PRIMARY KEY,
    cliente_id INTEGER REFERENCES cliente(id) ON DELETE CASCADE,
    hotel_id INTEGER REFERENCES hotel(id) ON DELETE CASCADE,
    email VARCHAR(200) NOT NULL
);

CREATE TABLE contrato (
    id SERIAL PRIMARY KEY,
    reserva_id INTEGER REFERENCES reserva(id) ON DELETE CASCADE UNIQUE,
    numero_contrato VARCHAR(50) UNIQUE NOT NULL,
    data_inicio DATE NOT NULL,
    data_fim DATE,
    situacao VARCHAR(20) NOT NULL DEFAULT 'Ativo'
);

CREATE TABLE pagamento (
    id SERIAL PRIMARY KEY,
    reserva_id INTEGER REFERENCES reserva(id) ON DELETE CASCADE UNIQUE,
    data_pagamento DATE NOT NULL,
    valor NUMERIC(10, 2) NOT NULL,
    forma_pagamento VARCHAR(50),
    situacao VARCHAR(20) DEFAULT 'Pago'
);

CREATE TABLE contrata (
    servico_id INTEGER REFERENCES servico(id) ON DELETE CASCADE,
    reserva_id INTEGER REFERENCES reserva(id) ON DELETE CASCADE,
    PRIMARY KEY (servico_id, reserva_id)
);

CREATE TABLE hospede_reserva (
    cliente_id INTEGER REFERENCES cliente(id) ON DELETE CASCADE,
    reserva_id INTEGER REFERENCES reserva(id) ON DELETE CASCADE,
    PRIMARY KEY (cliente_id, reserva_id)
);

```