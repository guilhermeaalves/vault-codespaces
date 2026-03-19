FROM hashicorp/vault:latest

# Instala ferramentas base (incluindo bash para o devcontainer e curl para o healthcheck)
RUN apk add --no-cache git jq logrotate openssl curl unzip bash

# Instala o PostgreSQL
RUN apk add --no-cache postgresql postgresql-client

# Baixa e instala a última versão do Terraform
RUN LATEST_VERSION=$(curl -s https://api.github.com/repos/hashicorp/terraform/releases/latest | grep '"tag_name":' | sed -E 's/.*"v([^"]+)".*/\1/') && \
    curl -LO https://releases.hashicorp.com/terraform/${LATEST_VERSION}/terraform_${LATEST_VERSION}_linux_amd64.zip && \
    unzip terraform_${LATEST_VERSION}_linux_amd64.zip -d /usr/local/bin && \
    rm terraform_${LATEST_VERSION}_linux_amd64.zip

# Configura os diretórios e permissões do PostgreSQL
# Nota: O pacote postgresql do Alpine já cria o usuário 'postgres' automaticamente
RUN mkdir -p /var/lib/postgresql/data /run/postgresql /var/log/postgresql && \
    chown -R postgres:postgres /var/lib/postgresql/data /run/postgresql /var/log/postgresql

# Variáveis de ambiente para o Vault em modo Dev
ENV VAULT_DEV_ROOT_TOKEN_ID=root
ENV VAULT_DEV_LISTEN_ADDRESS=0.0.0.0:8200

# Expõe a porta padrão do Vault
EXPOSE 8200
