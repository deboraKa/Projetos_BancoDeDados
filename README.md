# Integração e Processamento de Dados de Câmbio com Azure

## Visão geral

Este projeto foi desenvolvido em um laboratório com o objetivo de estruturar um processo de extração, transformação e armazenamento de dados de cotação do dólar, disponibilizados pela API do Banco Central.

A necessidade apresentada era obter a cotação e suas variações ao longo da semana, preparando os dados para utilização em relatórios semanais.

## Objetivo

Construir um fluxo de dados utilizando serviços Azure para:

- Consumir dados de cotação disponibilizados pela API do Banco Central.
- Carregar os dados inicialmente em uma tabela de estágio (*staging*) no Azure SQL.
- Padronizar os tipos e formatos dos campos por meio de um procedimento armazenado.
- Armazenar os dados processados no Azure Blob Storage em formato Parquet.

## Tecnologias e serviços

- **Azure Data Factory:** orquestração do pipeline e movimentação dos dados.
- **Azure SQL Database:** armazenamento relacional com tabelas de estágio e final.
- **Azure Blob Storage:** armazenamento dos arquivos processados.
- **API do Banco Central:** origem dos dados de cotação.
- **Parquet:** formato utilizado para armazenar os dados processados.

## Arquitetura e fluxo de dados

O processo documentado no laboratório segue estas etapas:

1. **Preparação no Azure SQL**  
   Foi criado um schema, uma tabela de estágio (*Stage*) e uma tabela final. Um arquivo CSV também foi utilizado para popular as tabelas durante a preparação do ambiente.

2. **Extração com Azure Data Factory**  
   Uma atividade *Copy Data* consome os dados disponibilizados pela API do Banco Central e carrega os registros na tabela Stage do Azure SQL.

3. **Transformação dos dados**  
   Um procedimento armazenado realiza a conversão dos tipos de dados, incluindo campos numéricos para Float e campos de data para Datetime. Também padroniza o separador decimal, substituindo vírgula por ponto.

4. **Carga no armazenamento**  
   Uma segunda atividade *Copy Data* utiliza os dados da tabela final e os grava no Azure Blob Storage em formato Parquet.

5. **Armazenamento**  
   Os arquivos Parquet ficam armazenados em um container, disponibilizando os dados processados para usos posteriores.

## Fluxo resumido

```text
API do Banco Central
        |
        v
Azure Data Factory
  Copy Data (extração)
        |
        v
Azure SQL - Tabela Stage
        |
        v
Procedimento armazenado
(conversão e padronização)
        |
        v
Azure SQL - Tabela Final
        |
        v
Azure Data Factory
  Copy Data (carga)
        |
        v
Azure Blob Storage
   Arquivo Parquet
```

## Conceitos aplicados

- Integração de dados por API.
- Organização de dados em tabelas de estágio e final.
- Orquestração de atividades de cópia no Azure Data Factory.
- Transformação e padronização de tipos de dados.
- Persistência de dados em formato colunar Parquet.

## Observações

Este repositório documenta um laboratório/projeto de integração de dados. As informações apresentadas descrevem o fluxo registrado no material do projeto; não são publicados aqui endpoints privados, credenciais ou configurações de acesso ao ambiente.

## Possíveis melhorias futuras

- Adicionar diagrama visual da arquitetura implementada.
- Incluir capturas de tela das configurações do pipeline, sem expor credenciais.
- Documentar o schema e os campos das tabelas Stage e final.
- Descrever a frequência de execução e a estratégia de atualização dos dados, caso essas definições sejam formalizadas no projeto.
