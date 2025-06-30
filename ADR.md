# ADR 001 - Adoção de gRPC na API do Sistema Fonts

## Status

Decisão tomada e implementada.

## Data

2024-06-30

## Contexto

O sistema Fonts é responsável por receber, armazenar e expor dados referentes ao uso de fontes licenciadas em builds de aplicativos. Para isso, foi necessário definir um protocolo de comunicação entre os agentes produtores (scripts executados via Jenkins) e consumidores (notebooks Databricks) com a `Fonts API`.

As opções mais consideradas foram:

- REST com JSON
- gRPC com Protobuf
- Mensageria assíncrona (RabbitMQ, Kafka)

Alguns critérios de decisão incluíram:
- **Desempenho**
- **Facilidade de integração**
- **Padronização**
- **Serialização eficiente de dados simples**

## Decisão

Foi adotado o protocolo **gRPC** como meio de comunicação entre clientes e a `Fonts API`.

A escolha foi motivada por:

- **Baixa latência** e **serialização eficiente** (Protobuf) para alto volume de requisições.
- Compatibilidade nativa com o ambiente de execução escolhido (`Go`).
- Permite que aplicativos cliente e servidor se comuniquem de forma eficiente, como se fossem objetos locais.
- Código de alto desempenho.

## Consequências

**Positivas:**

- Ganho de performance no tráfego entre Jenkins e API.
- Interface bem definida e tipada por meio dos arquivos `.proto`.

**Negativas:**

- Necessidade de ferramentas adicionais para debug (em comparação com REST).
- Curva de aprendizado para consumidores menos familiarizados com gRPC.

## Decisores

- Gerentes do projeto

## Links Relacionados

- [RFC 001 - Sistema de Contagem de Fontes](./RFC.md)
