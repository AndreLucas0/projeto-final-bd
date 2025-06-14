# Data Control Language

``` sql
/* Criação de usuários */
CREATE USER adminitrador_01 WITH PASSWORD 'admin2025';
CREATE USER atendente_01 WITH PASSWORD 'atend0125';

/* Concede todas as permissões para o administrador*/
GRANT ALL PRIVILEGES ON SCHEMA public TO administrador_01;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO administrador_01;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO administrador_01;

/* Permissões futuras */
ALTER DEFAULT PRIVILEGES IN SCHEMA public
GRANT ALL PRIVILEGES ON TABLES TO administrador_01;

ALTER DEFAULT PRIVILEGES IN SCHEMA public
GRANT ALL PRIVILEGES ON SEQUENCES TO administrador_01;

/* Concede permissão de apenas acessar ao schema */
GRANT USAGE ON SCHEMA public TO atendente_01;

/* Concede permissão de leitura em todas as tabelas e sequências */
GRANT SELECT ON ALL TABLES IN SCHEMA public TO atendente_01;
GRANT SELECT ON ALL SEQUENCES IN SCHEMA public TO atendente_01;

/* Reservas */
GRANT SELECT, INSERT, UPDATE ON reserva TO atendente_01;

/* Clientes */
GRANT SELECT, INSERT, UPDATE ON cliente TO atendente_01;
GRANT SELECT, INSERT, UPDATE ON pessoa_fisica TO atendente_01;
GRANT SELECT, INSERT, UPDATE ON pessoa_juridica TO atendente_01;

/* Pagamentos */
GRANT SELECT, INSERT, UPDATE ON pagamento TO atendente_01;

/* Contratos (gerados com a reserva) */
GRANT SELECT, INSERT, UPDATE ON contrato TO atendente_01;

/* Dados de contato dos clientes */
GRANT SELECT, INSERT, UPDATE ON endereco TO atendente_01;
GRANT SELECT, INSERT, UPDATE ON telefone TO atendente_01;
GRANT SELECT, INSERT, UPDATE ON email TO atendente_01;

/* Hospedes */
GRANT SELECT, INSERT, DELETE ON hospede_reserva TO atendente_01;

/* Serviços */
GRANT SELECT, INSERT, DELETE ON contrata TO atendente_01;

/* Permissão de apenas leitura para tabelas futurasz */
ALTER DEFAULT PRIVILEGES IN SCHEMA public
GRANT SELECT ON TABLES TO atendente_01;

ALTER DEFAULT PRIVILEGES IN SCHEMA public
GRANT SELECT ON SEQUENCES TO atendente_01;


```