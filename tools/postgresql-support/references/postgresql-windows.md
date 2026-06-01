# PostgreSQL Windows Reference

## Configuracao alvo

- IP fixo principal: `192.168.1.10`.
- Subnet LAN esperada: `192.168.1.0/24`.
- Porta: `5432`.
- Banco: `ai_platform`.
- Usuario administrativo: `ai_admin`.
- Schemas: `core`, `rag`, `chat`, `logs`, `config`, `monitoring`, `automation`.
- Backup fora do repositorio: `C:\backup\postgresql`.

## Validacoes uteis

```powershell
Get-NetTCPConnection -LocalPort 5432
Get-NetIPAddress -IPAddress 192.168.1.10
Test-NetConnection -ComputerName 192.168.1.10 -Port 5432
psql --version
pg_isready
```

## SQL base

```sql
CREATE SCHEMA IF NOT EXISTS core;
CREATE SCHEMA IF NOT EXISTS rag;
CREATE SCHEMA IF NOT EXISTS chat;
CREATE SCHEMA IF NOT EXISTS logs;
CREATE SCHEMA IF NOT EXISTS config;
CREATE SCHEMA IF NOT EXISTS monitoring;
CREATE SCHEMA IF NOT EXISTS automation;

CREATE EXTENSION IF NOT EXISTS vector;

CREATE TABLE IF NOT EXISTS rag.documents (
    id UUID PRIMARY KEY,
    titulo TEXT,
    conteudo TEXT,
    criado_em TIMESTAMP
);

CREATE TABLE IF NOT EXISTS rag.embeddings (
    id UUID PRIMARY KEY,
    document_id UUID,
    embedding vector(1536),
    metadata JSONB
);
```

## Acesso LAN

- `postgresql.conf`: permitir escuta de rede local (`listen_addresses` compativel com LAN).
- `pg_hba.conf`: permitir `192.168.1.0/24` com autenticacao segura.
- Firewall do Windows: liberar TCP `5432` apenas em perfil privado/local sempre que possivel.
