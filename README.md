# Projeto de Infraestrutura de Dados com Azure

Este repositório contém a estrutura e os detalhes do projeto de infraestrutura de dados utilizando serviços do Azure, incluindo Azure Data Lake, Data Factory e Databricks. O objetivo principal é criar uma solução escalável e eficiente para gerenciar e processar grandes volumes de dados, garantindo redundância e segurança.
![image](https://github.com/user-attachments/assets/fe6244df-764e-4a3d-8b91-804d6605418c)

## Índice

- [Visão Geral](#visão-geral)
- [Recursos](#recursos)
- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Configuração](#configuração)
  - [Grupo de Recursos](#grupo-de-recursos)
  - [Conta de Armazenamento](#conta-de-armazenamento)
  - [Data Lake](#data-lake)
  - [Data Factory](#data-factory)
  - [Azure Databricks](#azure-databricks)
  - [Registro de Aplicativo](#registro-de-aplicativo)
  - [Key Vault](#key-vault)
- [Uso](#uso)
- [Contribuição](#contribuição)
- [Licença](#licença)

## Visão Geral

Este projeto configura uma infraestrutura de dados utilizando os serviços do Azure para garantir armazenamento eficiente, processamento de dados e segurança. A configuração inclui:

- **Azure Data Lake**: Armazenamento escalável e seguro para grandes volumes de dados.
- **Azure Data Factory**: Orquestração e automação de fluxos de dados.
- **Azure Databricks**: Ambiente de análise e processamento de dados baseado em Apache Spark.
- **Key Vault**: Gerenciamento seguro de segredos e chaves.

## Recursos

- **Localização**: West US (mais econômico)
- **Redundância**: LRS (Localmente Redundante) para reduzir custos
- **Namespace Hierárquico**: Transformação da Conta de Armazenamento em Data Lake
- **Containers no Data Lake**: `db`, `transient`, `bronze`, `silver`, `gold`

## Tecnologias Utilizadas

- **Azure Data Lake Storage**
- **Azure Data Factory**
- **Azure Databricks**
- **Azure Key Vault**
- **SQL**
- **Python**
- **Apache Spark**

## Configuração

### Grupo de Recursos

- **Nome**: `rg_bixtecnologia_dev`
- **Localização**: West US

### Conta de Armazenamento

- **Nome**: `dlsbixtecnologiadev`
- **Tipo de Performance**: Standard
- **Redundância**: Locally-redundant storage (LRS)
- **Namespace Hierárquico**: Habilitado
- **Tier de Acesso**: Hot
- **Segurança**:
  - Transferência segura: Habilitado
  - Acesso anônimo a blobs: Desabilitado
  - Chaves de conta de armazenamento: Habilitado
  - TLS mínimo: Versão 1.2

### Data Lake

Dentro do Data Lake, criamos os seguintes containers:

- **db**: Contém os dados principais do projeto.
- **transient**: Dados temporários para processamento intermediário.
- **bronze**: Dados brutos.
- **silver**: Dados processados e limpos.
- **gold**: Dados prontos para análise.

### Data Factory

Criamos um Data Factory para orquestrar o fluxo de dados entre os diferentes serviços. Ele é responsável por automatizar o movimento e a transformação dos dados entre as diferentes camadas do Data Lake.

### Azure Databricks

- **Workspace**: Criado para hospedar clusters e notebooks para o processamento de dados.
- **Cluster**: Configurado como Single Node para minimizar custos, utilizando Apache Spark.

### Registro de Aplicativo

Criamos um registro de aplicativo para permitir a comunicação segura entre os diferentes serviços, como Databricks e Data Factory. As credenciais e segredos gerados são armazenados com segurança no Azure Key Vault.

### Key Vault

Um Key Vault foi configurado para armazenar de forma segura todos os segredos e chaves utilizados pelos serviços no ambiente.

## Uso

Este projeto pode ser utilizado para gerenciar e processar grandes volumes de dados de forma eficiente, utilizando os serviços do Azure para garantir segurança, escalabilidade e redundância.

## Contribuição

Contribuições são bem-vindas! Sinta-se à vontade para abrir issues e pull requests para sugerir melhorias ou reportar problemas.

## Licença

Este projeto está licenciado sob os termos da licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

---

![pipelinebix](https://github.com/user-attachments/assets/f6db9b4d-f1a8-469c-bf18-7b20031d4006)

Este pipeline mostrado na imagem é uma estrutura de orquestração de dados do nosso projeto dentro do Azure Data Factory:

1. **Copy Data (copy_transient_pub_lic_venda)**: Este bloco realiza a cópia dos dados de uma origem externa para a camada "transient" do Data Lake, especificamente para a tabela de licenciamento de vendas públicas.

2. **Copy Data (copy_transient_nom_categoria)**: Similar ao anterior, esta atividade copia os dados da categoria de nome da origem externa para a camada "transient" do Data Lake.

3. **ForEach (forEach_transient_users_api)**: Este bloco executa uma iteração sobre um conjunto de dados. Para cada iteração, ele copia dados de usuários de uma API externa para a camada "transient" do Data Lake.

4. **Notebook (notebook_bronze_public_venda)**: Processa os dados copiados da atividade anterior para a camada "bronze", que geralmente representa a primeira camada de processamento, onde os dados brutos são preparados para etapas subsequentes.

5. **Notebook (notebook_bronze_nome_categoria)**: Processa os dados de categorias de nomes na camada "bronze", preparando-os para processamento adicional.

6. **Notebook (notebook_bronze_users)**: Processa os dados de usuários na camada "bronze".

7. **Notebook (notebook_silver_vendas)**: Toma os dados processados na camada "bronze" e realiza mais transformações para colocá-los na camada "silver", que normalmente contém dados mais refinados e prontos para análise.

8. **Notebook (notebook_gold_total_vendas)**: A última etapa do pipeline, onde os dados na camada "silver" são processados e refinados na camada "gold". Esta camada contém dados finais, prontos para consumo por ferramentas analíticas ou relatórios, como o total de vendas.
