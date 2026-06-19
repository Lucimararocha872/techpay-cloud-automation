# TechPay Credit API Challenge 🚀

## Objetivo📋

Este projeto simula uma API de análise de crédito executada no Google Cloud Platform (GCP), utilizando práticas de DevOps, segurança, automação e infraestrutura como código.

A solução foi desenvolvida para atender aos requisitos técnicos definidos pela diretoria da TechPay, contemplando CI/CD automatizado, gestão segura de segredos, identidade e acesso, conectividade privada e rastreabilidade de deploys.

## Arquitetura🏗️

GitHub
  ↓
Cloud Build Trigger
  ↓
Build Docker Image
  ↓
Artifact Registry
  ↓
Cloud Run
  ↓
Secret Manager

## Tecnologias Utilizadas🛠️

Python
Flask
Docker
Google Cloud Run
Google Cloud Build
Artifact Registry
Secret Manager
IAM
Serverless VPC Access

## Funcionalidades📌

A API expõe o endpoint:

GET /v1/credit-check

Exemplo de resposta:

```
{
  "service": "credit-api",
  "status": "active",
  "revision_tag": <commit_hash>",
  "vault_access": true
}
```

## Execução Local⚙️

Criar ambiente virtual
python3 -m venv venv
source venv/bin/activate
Instalar dependências
pip install -r requirements.txt
Executar aplicação
python main.py
Testar endpoint
curl localhost:8080/v1/credit-check
Execução via Docker
Build da imagem
docker build -t credit-api .
Executar container
docker run -p 8080:8080 credit-api
Testar endpoint
curl localhost:8080/v1/credit-check

## Pipeline CI/CD🔄

A automação é realizada pelo Cloud Build.

A cada merge na branch main:

Build da imagem Docker
Push para o Artifact Registry
Deploy automático no Cloud Run

Todas as etapas estão declaradas no arquivo:

`cloudbuild.yaml`

## Segurança🔐

### Service Account dedicada

A aplicação executa utilizando a Service Account:

**sa-credit-api**

Princípio aplicado:

- Menor privilégio (Least Privilege)

### Secret Manager

A variável:

**CREDIT_API_KEY** é injetada diretamente pelo Secret Manager durante o deploy.

### IAM

Somente a Service Account da aplicação possui permissão de leitura do segredo.

## Rede🌐

O serviço utiliza:

Serverless VPC Access Connector
Egress configurado como **all-traffic**, garantindo que todo o tráfego de saída seja roteado através da VPC.

## Disponibilidade☁️

Configuração aplicada:

min-instances = 1

### Objetivo:

- Evitar cold starts
- Melhorar tempo de resposta

## Controle de Acesso

O Cloud Run está configurado como:

`Allow authenticated only`

Comportamento esperado:

- Requisições anônimas → 403 Forbidden
- Requisições autenticadas → 200 OK

## Rastreabilidade🔍

Cada deploy recebe automaticamente:

**REVISION_TAG = $SHORT_SHA**, permitindo identificar exatamente qual commit está em execução em produção.

## Autora👩‍💻

Lucimara Rocha Silva

Projeto desenvolvido como laboratório prático educacional de DevOps e Google Cloud Platform para [ToolBox](https://github.com/toolbox-tech).

