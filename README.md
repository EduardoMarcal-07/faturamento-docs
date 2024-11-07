# Faturamento RB

> Faturamento RB é uma solução projetada para centralizar todas as cobranças que um cliente que utiliza a Rede Básica (RB) precisa pagar. A aplicação simplifica o gerenciamento de débitos pendentes para os clientes, consolidando as faturas emitidas por mais de 300 transmissoras, que participam da Rede Básica e fornecem serviços de energia a esse cliente.

## Sumário

1. [Introdução](#introdução)
2. [Arquitetura](#arquitetura)
3. [Tecnologias Utilizadas](#tecnologias-utilizadas)
4. [Configuração e Instalação](#configuração-e-instalação)
5. [Estrutura de Pastas](#estrutura-de-pastas)
6. [Guia de Funcionalidades](#guia-de-funcionalidades)
7. [Configuração de Ambiente](#configuração-de-ambiente)
8. [Execução de Testes](#execução-de-testes)
9. [Deploy e Build](#deploy-e-build)
10. [Monitoramento e Logs](#monitoramento-e-logs)
11. [Erros Conhecidos e Soluções](#erros-conhecidos-e-soluções)

---

## Introdução

**Resumo**:  

Cada transmissora realiza a cobrança individualmente, gerando faturas para os clientes que utilizam sua infraestrutura. As cobranças podem ter até três datas de vencimento: 15, 25 e 05 do mês seguinte. Em teoria, cada transmissora deve disponibilizar a nota fiscal cinco dias antes da data de vencimento, permitindo que o cliente tenha acesso ao documento em seu portal.

O módulo Spider do Faturamento RB automatiza a coleta das notas fiscais emitidas por cada transmissora. Esse módulo recebe uma mensagem através do Kafka contendo informações como site da transmissora, login, senha e outros dados necessários para autenticação. A partir dessas informações, o módulo tenta acessar o portal da transmissora e verificar se o documento já está disponível.

Após a coleta, as informações são enviadas de volta ao módulo Server por meio do Kafka. O módulo Server reúne os dados e os organiza em dashboards no frontend, oferecendo ao cliente uma visão consolidada e de fácil acesso de todos os débitos pendentes na Rede Básica. Dessa forma, o Faturamento RB melhora a experiência dos clientes ao permitir um acompanhamento mais eficiente de suas faturas e evitando o trabalho manual de acessar individualmente o portal de cada transmissora.

---

## Arquitetura

Microservices:

- **Padrão Arquitetural** (ex: Hexagonal, MVC, Microservices, Monolítico, Event-Driven).
- **Componentes e Módulos**:
  - Liste os principais módulos e descreva a função de cada um.
  - Exemplo: `service`: responsável por lógica de transformação de DTOs e responses HTTP.
- **Dependências**:
  - Explique dependências externas como bancos de dados, filas de mensagens (ex: RabbitMQ), serviços externos, etc.
- **Diagrama da Arquitetura** (se possível):
  - Um diagrama simples pode ajudar a ilustrar como os componentes interagem.

---

## Tecnologias Utilizadas

Liste todas as principais tecnologias, bibliotecas e frameworks utilizados:

- Linguagens: Java.
- Frameworks: Quarkus.
- Bancos de Dados: MySQL.
- Mensageria: Kafka.
- Ferramentas de Build e Deploy: Docker, Maven.
- Outras Dependências: Graphql.

---

## Configuração e Instalação

### Pré-Requisitos

Pré-requisitos para rodar a aplicação:

- Java 17.
- Configuração de variáveis de ambiente.
- Maven.
- Docker

### Passo a Passo de Instalação

1. Clone o repositório:
   ```bash
   git clone <URL-do-repositório>
   cd <pasta-do-repositório>

## Estrutura de Pastas

### Server

```plaintext
/src
  └── main
      ├── java/br/com/bestdeal/
      │   ├── cdi   # CDI
      |   |   ├── qualifier # Contém classes e configurações que utilizam o CDI (Contexts and Dependency Injection) com Qualifiers para a injeção de dependências específicas
      │   ├── converter # Contém classes responsáveis por converter ou mapear dados, como a conversão entre diferentes tipos de armazenamento (S3, LOCAL)
      │   ├── dao   # Objetos de Acesso a Dados, responsáveis por interagir diretamente com o banco de dados
      │   ├── export   # 
      │   ├── graphql   # Graphql
      |   |   ├── mutation ## Rest Api endpoints
      └── resources
          ├── application.yml  # Configurações da aplicação
          └── static           # Arquivos estáticos
```


## Guia de Funcionalidades

Descreva as principais funcionalidades do sistema. Para cada uma, detalhe:

- **Nome da funcionalidade**
- **Descrição**
- **Arquivos e classes responsáveis**
- **Fluxo de trabalho básico** (ex: sequência de chamadas entre métodos ou serviços)
- **Pontos de integração** com outros sistemas ou módulos

---

## Configuração de Ambiente

### Variáveis de Ambiente

Liste todas as variáveis de ambiente e explique o propósito de cada uma.

Exemplo:

- `DB_HOST`: Endereço do banco de dados.
- `RABBITMQ_HOST`: Endereço do servidor RabbitMQ.

### Configuração de Bancos de Dados

1. Configurações para cada ambiente (dev, stage, production).
2. Scripts de inicialização do banco, se necessário.
3. Comandos de criação de índices ou outras configurações específicas.

---

## Execução de Testes

### Testes Unitários

```bash
mvn test
```

## Deploy e Build 

1. Build da Aplicação:

  ```bash
  mvn clean install
  ```

2. Deploy em Produção:

- Processo de deploy e ferramentas (ex: Jenkins, Docker)
- Variáveis ou configurações específicas para produção.

3. Rollback:

- Como proceder em caso de falha de deploy.

## Monitoramento e Logs


1. Configuração de Logs:

- Localização dos arquivos de log e nível de logging.)
- Como alterar o nível de log.

2. Deploy em Produção:

- Ferramentas utilizadas (ex: Prometheus, Grafana).
- Endpoints para health check e métricas.

## Erros Conhecidos e Soluções

### Erros

1. **Falha na Extração de Informações de NFe e Boletos**  
   - **Descrição:** O processo de extração de dados de notas fiscais e boletos pode falhar devido a alterações no modelo dos documentos, uma vez que não existe um padrão unificado.

2. **Erro de Acesso na Spider-Message por Expiração de Login**  
   - **Descrição:** O erro ocorre quando a tentativa de acesso falha devido à expiração do login armazenado.

