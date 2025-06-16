# RFC 001 - Sistema de Contagem e Controle de Uso de Fontes em Builds

**Data:** 2025-06-16  
**Autor:** Roberto Pereira dos Santos Filho  
**Status:** Finalizado

---

## 1. Título

**Sistema de Monitoramento e Controle de Uso de Pacotes de Fontes em Pipelines de Build**

---

## 2. Contexto

Empresas que utilizam fontes licenciadas em seus produtos digitais (como Arial, Times, etc.) frequentemente contratam pacotes com número limitado de usos. A ausência de um mecanismo automatizado de monitoramento pode causar **uso excessivo** não intencional, expondo a organização a riscos legais e financeiros.

Antes da implementação deste sistema, o processo de build dos aplicativos não contabilizava ou validava o uso desses pacotes de fontes, dificultando auditorias e o cumprimento de contratos de licenciamento.

---

## 3. Proposta

Desenvolver um sistema automatizado que contabiliza e armazena informações sobre o uso de fontes durante o processo de build dos aplicativos. Esse sistema foi implementado com um fluxo de coleta, armazenamento e visualização dos dados de uso de fontes.

---

## 4. Objetivos

- Automatizar a contagem de fontes utilizadas em builds.  
- Persistir os dados de uso em banco de dados relacional.  
- Disponibilizar APIs para leitura, gravação e exclusão de registros.  
- Integrar com notebooks Databricks para atualização continua da planilha.  
- Exibir os dados consolidados em Google Planilhas para consulta de stakeholders.

---

## 5. Fluxo de Execução

1. Um engenheiro dispara um **build no Jenkins**.
2. Durante o build, um **script executa a contagem de arquivos de fontes utilizados**.
3. O script envia os dados para o **Fonts API**, um serviço `gRPC` escrito em Go.
4. O serviço armazena os dados em um **banco de dados PostgreSQL**.
5. Um notebook no **Databricks** consulta periodicamente esses dados para gerar relatórios.
6. O relatório é exportado e salvo em uma **planilha do Google Sheets**, onde o usuário final visualiza a quantidade de usos acumulados por fonte/licença.

---

## 6. Arquitetura

O sistema foi modelado utilizando o modelo C4. Os diagramas completos podem ser visualizados abaixo:

![Diagrama C4 - System Landscape](bPJFRjD04CRl-nH3n26LAd7f4Qf24WfL1L4Zu5XDx8apodhMtPsDIjy6SU09U8AyM7UTVuRIfbpisP7t-tc-R-speEWrrjO4KWZKe4Tr7iG96MMr19FlGQc6IvGo5DYGCPPc2kh0SpLNADbJeUp4c1SiXOqbmUl1oQl1oUdVZUk14wLRFJLcJ3uuwNy9SVN3ipk6thRi7SFBaaBXt_HogLY5TMLP5gawskuTIKKo9_Qkf7UH.png)

![Diagrama C4 - Containers](XPJ1RXCn48RlVefXnI4H0Iuzagfj4WgeWf0SE4QJzNHZnMklx76LWdWQ3Zm1Jv0NmtOsP4aQkEpEUCV__Fx6sYlFw3ZKMdXYyDIWGxOEZ3KaMB4cU6iDUtW9e_X6PSXv8JJTCx05fweLWrIEIbRM2F5CcL87IV1cTF5wT75vlB18AhJXfpza-KiXve-UgUvz8BDMnW-WQADY0Cyb-T8DYxbA9GXH00FR6hS_jpziIfT1QPbB.png)

---

## 7. Considerações de Segurança

- A API utiliza autenticação baseada em token.
- O banco de dados é acessado apenas via serviços internos autenticados.
- O Databricks possui permissão somente leitura.
- O acesso ao Google Sheets é realizado por uma conta de serviço restrita.

---

## 8. Impacto

- Redução de riscos legais por uso indevido de licenças.  
- Monitoramento contínuo do uso de fontes adquiridas.  
- Agilidade na produção de relatórios para áreas de compliance e jurídico.  
- Suporte a auditorias internas e externas com rastreabilidade total.

---

## 9. Status de Implantação

- Scripts Jenkins implementados nos projetos relevantes.  
- API Fonts em produção com monitoramento ativo.  
- Banco de dados populado com dados históricos.  
- Notebooks Databricks configurados com execuções programadas.  
- Google Planilhas em uso pelos times responsáveis.