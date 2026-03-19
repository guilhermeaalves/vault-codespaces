# Dockerfile
FROM hashicorp/vault:latest

# Install git
RUN apk add --no-cache git jq logrotate openssl

# Download and install latest Terraform
RUN apk add --no-cache curl unzip && \
    add --no-cache postgresql postgresql-client \
    LATEST_VERSION=$(curl -s https://api.github.com/repos/hashicorp/terraform/releases/latest | grep '"tag_name":' | sed -E 's/.*"v([^"]+)".*/\1/') && \
    curl -LO https://releases.hashicorp.com/terraform/${LATEST_VERSION}/terraform_${LATEST_VERSION}_linux_amd64.zip && \
    unzip terraform_${LATEST_VERSION}_linux_amd64.zip -d /usr/local/bin && \
    rm terraform_${LATEST_VERSION}_linux_amd64.zip && \
    apk del curl unzip

RUN mkdir -p /var/lib/postgresql/data /run/postgresql /var/log/postgresql && \
    adduser -D postgres || true && \
    chown -R postgres /var/lib/postgresql/data /run/postgresql /var/log/postgresql

# Set environment variables for dev mode
ENV VAULT_DEV_ROOT_TOKEN_ID=root
ENV VAULT_DEV_LISTEN_ADDRESS=0.0.0.0:8200

# Expose the default Vault port
EXPOSE 8200
