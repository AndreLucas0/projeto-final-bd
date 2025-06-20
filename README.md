# Projeto Final: Banco de Dados Relacional

## Objetivo

Este repositório contém o projeto final da disciplina de Banco de Dados Relacional. O objetivo é aplicar os conceitos estudados, incluindo modelagem, normalização e manipulação de dados utilizando DDL, DML, DQL — com bonificação para comandos DCL e DTL.

## Integrantes do Grupo

- André Lucas Ferreira
- Leonardo Rodrigues
- Nickolas Machado
- Odirlei Lima

## Tema Escolhido

> _Sistema de Gerenciamento de Hotelaria_

O Sistema de Gerenciamento de Hotelaria tem como propósito principal organizar, armazenar e gerenciar de forma eficiente todas as informações relacionadas às operações de um hotel, tais como cadastros de hóspedes, reservas, quartos, pagamentos, serviços e funcionários. Este sistema permite automatizar processos, reduzir erros operacionais e oferecer uma gestão mais precisa e eficaz.
Sua relevância está diretamente associada à aplicação prática dos conceitos de banco de dados relacionais, uma vez que as informações são estruturadas em tabelas interligadas por meio de chaves primárias e chaves estrangeiras, representando fielmente os relacionamentos existentes no ambiente real.

## Modelagem de Dados

### Entidades, Atributos e Relacionamentos

### Entidades Principais

- **Cliente**
  - Representa qualquer pessoa (física ou jurídica) que realiza uma reserva ou contrata serviços no hotel.
  - Relaciona-se diretamente com:
    - `pessoa_fisica` ou `pessoa_juridica` (especialização),
    - `endereco`,
    - `telefone`,
    - `email`,
    - `reserva`,
    - `hospede_reserva`.

- **Pessoa Física**
  - Subentidade de `cliente`.
  - Armazena:
    - Nome,
    - CPF,
    - Data de nascimento.

- **Pessoa Jurídica**
  - Subentidade de `cliente`.
  - Armazena:
    - Nome fantasia,
    - Razão social,
    - CNPJ.

- **Hotel**
  - Representa uma unidade hoteleira.
  - Relaciona-se com:
    - `quarto`,
    - `atendente`,
    - `endereco`,
    - `telefone`,
    - `email`.

- **Quarto**
  - Acomodação oferecida pelo hotel.
  - Dados:
    - Tipo (Suite, Padrão),
    - Capacidade,
    - Preço,
    - Status de disponibilidade.
  - Relacionado a `hotel` e `reserva`.

- **Atendente**
  - Funcionário vinculado ao hotel.
  - Relacionado a `hotel` e às `reservas`.

- **Reserva**
  - Registro de reserva de um quarto em uma data específica.
  - Relaciona-se com:
    - `cliente`,
    - `atendente`,
    - `quarto`,
    - `contrato`,
    - `pagamento`,
    - `contrata` (serviços adicionais),
    - `hospede_reserva` (hóspedes da reserva).

- **Serviço**
  - Serviços adicionais oferecidos:
    - Café da manhã, almoço, jantar, entre outros.
  - Relacionamento N:N com `reserva` por meio de `contrata`.

- **Contrato**
  - Formaliza uma reserva:
    - Número do contrato,
    - Datas de início e fim,
    - Situação atual.

- **Pagamento**
  - Registro financeiro vinculado à reserva:
    - Valor,
    - Data,
    - Forma de pagamento,
    - Situação.

- **Endereço**
  - Dados de localização associados a:
    - `cliente` ou `hotel`.

- **Telefone**
  - Dados de contato telefônico de:
    - `cliente` ou `hotel`.

- **Email**
  - Dados de contato eletrônico de:
    - `cliente` ou `hotel`.

- **Contrata** *(Tabela de junção)*
  - Relacionamento N:N entre `reserva` e `servico`.

- **Hospede_Reserva** *(Tabela de junção)*
  - Relacionamento N:N entre `reserva` e `cliente` (hóspedes).

---

### Relacionamentos Entre as Entidades

- Um **cliente** é especializado em **pessoa física** ou **pessoa jurídica** (relação 1:1).
- Um **hotel** possui muitos:
  - **quartos**,
  - **atendentes**,
  - **telefones**,
  - **endereços**,
  - **e-mails** (1:N).
- Um **quarto** pertence a um único **hotel**, mas pode ter múltiplas **reservas** ao longo do tempo (N:1).
- Um **atendente** está vinculado a um único **hotel** e atende múltiplas **reservas** (N:1).
- Uma **reserva** está associada a um único:
  - **cliente**,
  - **quarto**,
  - **atendente**, além de gerar:
  - **contrato** (1:1),
  - **pagamento** (1:1).
- Uma **reserva** pode contratar vários **serviços**, e cada **serviço** pode estar associado a várias reservas (**N:N**, via `contrata`).
- A tabela `hospede_reserva` permite associar múltiplos **clientes/hóspedes** a uma única **reserva**, suportando reservas de grupos (**N:N**).
- **Endereço**, **telefone** e **email** podem estar vinculados tanto a **clientes** quanto a **hotéis** (N:1).

---

### Diagrama Entidade-Relacionamento (DER)

> Inserir imagem ou link para o DER (use a pasta `/docs` ou uma imagem hospedada):

![DER](./Docs/diagrama projeto final (1).png)


## Normalização

O banco de dados foi normalizado até a **Terceira Forma Normal (3NF)**.
A modelagem deste banco de dados foi construída considerando rigorosamente os princípios de normalização, com o objetivo de garantir **consistência, integridade dos dados e evitar redundâncias desnecessárias**. Foram aplicadas as três primeiras formas normais (1FN, 2FN e 3FN), amplamente aceitas como suficientes para a maioria dos sistemas relacionais.

---

### 🔹 1ª Forma Normal (1FN) — Eliminação de Grupos Repetitivos

- **Regras Aplicadas:**
  - Todos os atributos possuem valores atômicos (não divididos).
  - Não existem colunas multivaloradas nem listas em uma única célula.

- **Exemplos no Projeto:**
  - A tabela `telefone` foi criada para armazenar múltiplos telefones associados a um cliente ou hotel, eliminando a possibilidade de vários números em uma única coluna.
  - Da mesma forma, `email` e `endereco` seguem esse princípio, permitindo quantidades variáveis de contatos sem repetir colunas.

---

### 🔹 2ª Forma Normal (2FN) — Eliminação de Dependências Parciais

- **Regras Aplicadas:**
  - Aplicada apenas a tabelas com chave primária composta.
  - Todos os atributos não-chave dependem da chave completa, e não apenas de parte dela.

- **Exemplos no Projeto:**
  - A tabela `contrata` possui chave composta (`servico_id`, `reserva_id`), e não há atributos adicionais além das chaves — portanto, não existe dependência parcial.
  - Na tabela `hospede_reserva`, a chave composta (`cliente_id`, `reserva_id`) representa exatamente o relacionamento, sem atributos dependentes apenas de `cliente_id` ou `reserva_id`.

---

### 🔹 3ª Forma Normal (3FN) — Eliminação de Dependências Transitivas

- **Regras Aplicadas:**
  - Nenhum atributo não-chave depende de outro atributo não-chave.
  - Todos os atributos dependem apenas da chave primária.

- **Exemplos no Projeto:**
  - Na tabela `reserva`, os dados de `data_entrada`, `data_saida` e `concluido` dependem diretamente da chave `id` da reserva.
  - Informações como `logradouro`, `cep`, `cidade` e `estado` foram corretamente isoladas na tabela `endereco` em vez de serem armazenadas diretamente em `cliente` ou `hotel`.
  - Dados de contato como `email` e `telefone` foram separados em suas próprias tabelas, removendo dependências transitivas com relação ao cliente ou hotel.

---


## Scripts SQL

Todos os scripts estão localizados neste repositório.

### DDL (Data Definition Language)

- Criação de tabelas
- Definição de chaves primárias e estrangeiras
- Restrições de integridade

> Caminho: `ddl.sql`

### DML (Data Manipulation Language)

- Inserção de dados
- Atualização de registros
- Exclusão de registros

> Caminho: `dml.sql`

### DQL (Data Query Language)

- Consultas para recuperar dados relevantes

> Caminho: `dql.sql`

### DCL (Data Control Language)

> Caminho: `dcl.sql`

- Uso de `GRANT` e `REVOKE` para controle de acesso

### DTL (Data Transaction Language)

> Caminho: `dml.sql`

- Uso de `BEGIN`, `COMMIT`, `ROLLBACK`, `SAVEPOINT` para garantir a integridade transacional


## Documentação (ABNT)

A documentação completa está disponível na pasta `/Docs`, estruturada conforme as normas da ABNT, contendo:

- Introdução
- Modelagem conceitual e lógica
- Scripts comentados
- Conclusão e referências

> Caminho: `Docs/Projeto Final - BD.pdf`


## Requisitos Técnicos

- **SGBD utilizado**: PostgreSQL
- **Versão recomendada**: PostgreSQL 15+
- **Ferramentas utilizadas**:
  - PgAdmin, VSCode.
