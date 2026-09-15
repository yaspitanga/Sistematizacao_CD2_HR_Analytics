
## Identificação
- **Nome Completo:** YASMIN PITANGA SILVA
- **Matrícula:**: 72501459
- **Disciplina:** Ciência de Dados II

  
# Projeto Data Science II — HR Analytics (KDD & PySpark)

Este repositório contém a implementação do pipeline completo da metodologia **KDD (Knowledge Discovery in Databases)** para análise e predição de turnover de funcionários (*HR Analytics*).

## Tecnologias e Bibliotecas
* **Linguagem:** Python
* **Motor Big Data:** PySpark (Spark SQL, DataFrames, MLlib)
* **Visualização:** Matplotlib, Seaborn
* **Ambiente de Execução:** Google Colab

## Estrutura do Pipeline (Metodologia KDD)
1. **Seleção e Ingestão:** Carga do dataset `HR_Analytics.csv` em Spark DataFrames.
2. **Pré-processamento:** Tratamento de valores nulos, conversão de tipos de dados e remoção de colunas invariantes.
3. **Análise Exploratória (EDA):** Execução de 5 consultas em Spark SQL e gráficos comparativos.
4. **Modelagem Preditiva:** Comparativo de classificação entre **Decision Tree** (AUC 0.73) e **Random Forest** (AUC 0.83).
5. **Modelagem Descritiva:** Clusterização via **K-Means** (Silhueta 0.82) dividida em 3 perfis organizacionais.
6. **Conclusões KDD:** Interpretação dos resultados para tomada de decisão no setor de RH.

## Como Executar o Notebook
1. Abra o arquivo `Sistematizacao_CD2_Pipeline_KDD.ipynb` no **Google Colab**.
2. Garanta que o arquivo de dados `HR_Analytics.csv` esteja carregado no ambiente de execução do Colab.
3. Execute todas as células em ordem sequencial (**Ambiente de Execução ➔ Executar tudo**).
