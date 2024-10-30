# Projeto Airflow: Extração de Dados Climáticos e de Ações

Este repositório contém duas DAGs para extração automatizada de dados utilizando o Apache Airflow. As DAGs foram desenvolvidas para extrair dados climáticos históricos de Boston e dados de ações de empresas selecionadas, permitindo armazenamento e análise desses dados em arquivos `.csv`.

## Índice

- [Descrição das DAGs](#descrição-das-dags)
  - [DAG 1: Extração de Dados Climáticos em Boston](#dag-1-extração-de-dados-climáticos-em-boston)
  - [DAG 2: Extração de Dados de Ações](#dag-2-extração-de-dados-de-ações)
- [Configuração do Ambiente](#configuração-do-ambiente)
- [Como Executar](#como-executar)
- [Estrutura de Arquivos](#estrutura-de-arquivos)

---

## Descrição das DAGs

### DAG 1: Extração de Dados Climáticos em Boston

Esta DAG é executada semanalmente e coleta as condições climáticas da semana anterior para a cidade de Boston. Os dados são salvos em uma pasta específica com a data da semana.

- **Nome da DAG**: `dados_climaticos`
- **Objetivo**: Extrair dados climáticos de Boston e armazenar semanalmente para fins de análise.
- **Intervalo de execução**: Semanal, toda segunda-feira (`0 0 * * 1`)
- **Data de início**: 22 de agosto de 2022
- **Timezone**: UTC

#### Estrutura das Tarefas
1. **Tarefa 1 - Criação de Pasta (`cria_pasta`)**
   - Cria uma pasta com a data correspondente para armazenar os dados extraídos.
   - Comando: `mkdir -p "/home/thiago/Documents/airflowalura/tempo_em_boston/semana={{data_interval_end.strftime("%Y-%m-%d")}}"`

2. **Tarefa 2 - Extração de Dados (`extrai_dados`)**
   - Extrai dados climáticos de Boston da semana anterior via API e salva em três arquivos `.csv`: `dados_brutos.csv` (dados completos), `temperaturas.csv` (dados de temperatura), e `condicoes.csv` (condições climáticas e ícones).

---

### DAG 2: Extração de Dados de Ações

Esta DAG é executada diariamente, de terça a sexta-feira, e coleta dados horários de preços das ações das empresas AAPL, MSFT, GOOG e TSLA. Cada conjunto de dados é salvo em uma pasta específica para cada ticker, permitindo acompanhamento regular.

- **Nome da DAG**: `get_stocks_dag`
- **Objetivo**: Extrair dados horários de preços de ações para análise.
- **Intervalo de execução**: Diária, de terça a sexta-feira (`0 0 * * 2-6`)
- **Data de início**: 1 de janeiro de 2024
- **Timezone**: UTC

#### Estrutura das Tarefas
1. **Tarefa de Extração por Ticker (`get_history`)**
   - Para cada ticker em `TICKERS`, extrai dados horários via `yfinance` e salva um arquivo `.csv` com o preço das ações no caminho correspondente ao ticker.

---

## Configuração do Ambiente

1. **Instale o Apache Airflow**: Siga a [documentação oficial](https://airflow.apache.org/docs/apache-airflow/stable/installation/index.html) para instalar o Airflow em um ambiente adequado.
2. **Bibliotecas Python**:
   - `pandas` e `yfinance` para manipulação e extração de dados.
   - Instale com: 
     ```bash
     pip install pandas yfinance
     ```
3. **Configurações de Diretório**:
   - Certifique-se de que os caminhos definidos nas DAGs estão acessíveis no ambiente de execução do Airflow (`/home/thiago/Documents/airflowalura` ou ajuste conforme necessário).

## Como Executar

1. Clone este repositório em sua máquina:
   ```bash
   git clone https://github.com/seu_usuario/seu_repositorio.git
   cd seu_repositorio
2. Mova as DAGs para o diretório de DAGs do Airflow.

3. Inicie o scheduler e a interface web do Airflow:
    ```bash
    airflow scheduler
    airflow webserver
4. Monitore a execução das DAGs na interface do Airflow.

## Estrutura de Arquivos
`/tempo_em_boston/semana=YYYY-MM-DD/`

**dados_brutos.csv**: Dados completos do clima.

**temperaturas.csv**: Dados de temperatura mínimos, médios e máximos.

**condicoes.csv**: Descrição das condições e ícones do clima.

`/stocks/{ticker}/{ticker}_YYYYMMDD.csv`

`Arquivo contendo os preços históricos das ações por hora.`

_______________________________________________________________________________________________

## Essas DAGs permitem a automação das extrações de dados, com armazenamento organizado para facilitar o acesso e análise.
