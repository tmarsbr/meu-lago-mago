# Meu Lago Mago 🪄

## Visão Geral

Este projeto constrói um **Data Lakehouse** do zero no Databricks, seguindo as melhores práticas de engenharia de dados. Inspirado na playlist **"Lago do Mago"** de Téo Me Why, implementamos uma arquitetura em camadas (Bronze, Silver, Gold) com integração entre **CDC (Change Data Capture)** e **Full Load**, utilizando o ecossistema Databricks para processamento de dados em larga escala.

O foco atual está na **camada Bronze**, responsável pela ingestão e armazenamento bruto dos dados, preparando o terreno para transformações nas camadas superiores.

## Arquitetura do Lakehouse

A arquitetura segue o modelo tradicional de Lakehouse:

- **Bronze Layer**: Dados brutos ingeridos diretamente das fontes, com mínimo processamento. Armazenamento em Delta Lake para versionamento e ACID transactions.
- **Silver Layer**: Dados limpos e transformados, prontos para análise.
- **Gold Layer**: Dados agregados e otimizados para consumo por aplicações e dashboards.

Fluxo atual (até Aula 02): `clientes` → `transacao` (ingestão sequencial com dependências via Lakeflow Jobs).

## Etapas Concluídas (Até Aula 02)

✅ **Configuração do Ambiente**
- Setup do workspace Databricks com Unity Catalog para governança de dados.
- Integração com repositório GitHub (`meu-lago-mago`) para versionamento de código.

✅ **Estrutura de Pastas**
- Organização inicial: `src/bronze/ingestao/`.
- Notebooks desenvolvidos: `bronze_clientes.ipynb` e `bronze_transacao.ipynb`.

✅ **Desenvolvimento dos Notebooks Bronze**
- `bronze_clientes`: Ingestão de dados de clientes via Full Load.
- `bronze_transacao`: Ingestão de dados de transações via CDC, com merge incremental.

✅ **Orquestração com Lakeflow Jobs**
- Criação de jobs no Databricks para execução automatizada.
- Dependência sequencial: `bronze_clientes` executa antes de `bronze_transacao` para evitar conflitos de concorrência.

✅ **Tratamento de Erros e Otimizações**
- Resolução de `ConcurrentAppendException` através de execução sequencial.
- Implementação de controle transacional Delta para merges seguros.

## Pipeline de Execução

![Execução do Lakeflow Job](job%20e%20pipeline.png)

A imagem mostra a execução do Lakeflow Job no Databricks, responsável pela orquestração das tabelas da camada Bronze do projeto meu-lago-mago. O fluxo de tarefas é sequencial, representando o processo de ingestão e consolidação de dados com controle de dependência entre tabelas.

- **bronze_clientes** → Primeira tarefa do pipeline, encarregada de processar e aplicar o merge CDC dos dados de clientes na camada Bronze.
- **bronze_transacao** → Executada automaticamente após a conclusão da etapa anterior, realiza o merge CDC das transações, garantindo a consistência e integridade referencial entre entidades.

Cada bloco verde indica execução bem-sucedida no Cluster de Tiago Silva, e a seta entre as tarefas evidencia a dependência de execução: a segunda inicia apenas quando a primeira finaliza com sucesso. Esse mecanismo previne conflitos de escrita no Delta Lake (como o ConcurrentAppendException) e assegura que os merges ocorram em ordem controlada.

📈 Essa execução confirma a estabilidade do pipeline de ingestão e valida a estrutura transacional da camada Bronze dentro do Lakehouse.

## Tecnologias e Ferramentas

- 🏗️ **Databricks**: Plataforma para processamento de big data e machine learning.
- 🏞️ **Delta Lake**: Formato de armazenamento com ACID transactions e versionamento.
- ⚡ **PySpark**: API Python para Apache Spark, usada para processamento distribuído.
- 📚 **Unity Catalog**: Sistema de governança para metadados e controle de acesso.
- 🐙 **GitHub**: Controle de versão e colaboração.
- 🔄 **Lakeflow Jobs**: Orquestração de pipelines no Databricks.

## Próximos Passos

🚀 **Aula 03 em diante**: Expansão para camadas Silver e Gold.
- Transformações e limpeza de dados na Silver Layer.
- Agregações e modelagem dimensional na Gold Layer.
- Integração avançada de CDC e otimização de performance.
- Implementação de dashboards e relatórios finais.

## Créditos e Referências

Este projeto é baseado na playlist **"Lago do Mago"** de **Téo Me Why**, disponível no YouTube. Agradecimentos especiais ao criador por compartilhar conhecimentos valiosos sobre engenharia de dados no Databricks.

🔗 [Playlist Lago do Mago - YouTube](https://www.youtube.com/playlist?list=PLvlkVRRKOYFTcLehYZ2Bd5hGIcLH0dJHE)

GitHub: [@TeoMeWhy](https://github.com/TeoMeWhy)

---

*Projeto desenvolvido com dedicação para aprendizado e aplicação prática de conceitos de Data Lakehouse. 🧙‍♂️*
