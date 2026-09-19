# Relatório Técnico: Predição de Turnover com PySpark (Metodologia KDD)

**Disciplina:** Ciencia de dados II  
**Aluna:** Yasmin Pitanga Silva  
**Matrícula:** 72501459  
**Repositório:** [yaspitanga/Sistematizacao_CD2_HR_Analytics](https://github.com/yaspitanga/Sistematizacao_CD2_HR_Analytics)

---

## 1. Problema de Negócio e Seleção do Dataset

### Contexto e Objetivo
A rotatividade de funcionários (*Employee Attrition*) representa um dos maiores desafios na gestão de recursos humanos, resultando em custos operacionais com recrutamento, perda de conhecimento técnico e queda de produtividade. O objetivo deste trabalho é aplicar o processo de Descoberta de Conhecimento em Bases de Dados (*Knowledge Discovery in Databases* — KDD) utilizando **PySpark** para identificar os fatores determinantes do desligamento voluntário e construir modelos preditivos e descritivos.

### Dataset Escolhido
* **Fonte Original:** [Kaggle - HR Analytics Dataset](https://www.kaggle.com/datasets/anshika2301/hr-analytics-dataset)
* **Arquivo:** `HR_Analytics.csv`
* **Volume de Dados:** 1.470 registros e 35 atributos (variáveis demográficas, financeiras, contratuais e de satisfação).

---

## 2. Pré-Processamento e Limpeza dos Dados

A etapa de preparação dos dados foi conduzida em ambiente PySpark distribuído, garantindo reprodutibilidade e tratamento de inconsistências de ingestão:

1. **Tratamento do Cabeçalho:** Remoção do caractere invisível BOM (`\ufeff`) e eliminação de espaços em branco nas chaves de colunas.
2. **Remoção de Colunas Invariantes:** Exclusão dos atributos `EmployeeCount`, `StandardHours` e `Over18`, pois apresentavam valor único para toda a base e zero variância.
3. **Mapeamento da Variável Alvo:** A variável categórica `Attrition` (`Yes`/`No`) foi convertida para a numérica `Attrition_Num` (`1`/`0`) para fins de modelagem.
4. **Tratamento de Nulos e Tipagem:** Filtragem de registros nulos nas variáveis do modelo e conversão explícita de atributos numéricos para `DoubleType`.

---

## 3. Análise Exploratória de Dados (Spark SQL)

Foram formuladas e respondidas 5 perguntas de negócio através do motor Spark SQL sobre a view temporária `tabela_rh`:

* **1. Média de Idade e Salário por Nível de Cargo:**  
  Identificou-se forte progressão salarial conforme o `JobLevel`. Cargos de nível 1 possuem média salarial de R\$ 2.789,08 (idade média de 30,1 anos), enquanto o nível 5 atinge R\$ 19.191,83 (idade média de 47,8 anos).

* **2. Tempo Médio de Empresa por Status de Saída:**  
  Funcionários que permaneceram na empresa possuem tempo médio de casa de **7,4 anos**, enquanto aqueles que pediram demissão possuem média de **5,1 anos**, demonstrando maior vulnerabilidade nos anos iniciais do contrato.

* **3. Média Salarial por Nível de Escolaridade:**  
  Doutores (Nível 5) e pós-graduados (Nível 4) possuem as maiores médias salariais (R\$ 8.277,64 e R\$ 6.881,00, respectivamente), confirmando o impacto da formação acadêmica na remuneração.

* **4. Relação entre Estado Civil e Hora Extra:**  
  Colaboradores solteiros (*Single*) apresentaram a maior proporção relativa de realização de horas extras (*OverTime = Yes*) em relação ao seu total do grupo.

* **5. Saídas por Nível de Satisfação com o Ambiente:**  
  O nível 1 de satisfação (*Low*) concentrou o maior volume proporcional de saídas voluntárias (`Attrition = Yes`), confirmando o clima organizacional como pilar crítico de retenção.

---

## 4. Modelagem Preditiva e Comparação de Métricas

Para a tarefa de classificação binária (`Attrition`), foi estruturado um pipeline do PySpark MLlib com `StringIndexer` e `VectorAssembler`. A base foi dividida em **80% para treino** e **20% para teste** (seed = 42).

### Algoritmos Testados:
1. **Decision Tree Classifier (Árvore de Decisão)**
2. **Random Forest Classifier (Ensemble)**

### Quadro Comparativo de Desempenho

| Algoritmo | Acurácia (%) | Área sob a Curva ROC (AUC) |
| :--- | :---: | :---: |
| **Decision Tree** | 86.53% | 0.73 |
| **Random Forest** | **86.53%** | **0.83** |

* **Análise dos Resultados:** Embora ambos os modelos tenham atingido a mesma acurácia geral, o **Random Forest** obteve um salto expressivo na **AUC (0.83)**. Isso demonstra uma capacidade superior de separabilidade das classes e ranqueamento de probabilidade do risco de saída.

---

## 5. Modelagem Descritiva (Clusterização K-Means)

Foi aplicado o algoritmo K-Means ($K=3$) utilizando os atributos de carreira e renda (`Age`, `MonthlyIncome`, `TotalWorkingYears`, `YearsAtCompany`). O modelo atingiu um **Score Silhueta de 0.65**, validando a coesão dos grupos.

### Perfis de Funcionários Identificados:
* **Cluster 0 (Início de Carreira):** Média de 34 anos de idade, salário médio de R\$ 3.938, 8 anos de experiência total e 5 anos de empresa. Representa a camada operacional com maior probabilidade de transição de mercado.
* **Cluster 1 (Plenos e Seniores):** Média de 40 anos de idade, salário médio de R\$ 10.055, 15 anos de experiência e 9 anos de empresa. Perfil estável e consolidado na operação.
* **Cluster 2 (Liderança e Alta Senioridade):** Média de 47 anos de idade, maior salário médio (R\$ 17.722), 25 anos de carreira e 14 anos de empresa. Perfil com taxa de retenção máxima devido ao pacote de benefícios e posição.

---

## 6. Conclusões, Recomendações e Limitações

### Ações Recomendadas para a Gestão
1. **Planos de Retenção Focados no Cluster 0:** Estabelecer planos de carreira claros e revisões salariais periódicas nos primeiros 5 anos de empresa para mitigar a evasão do nível operacional.
2. **Monitoramento do Modelo Random Forest:** Utilizar as probabilidades calculadas pelo modelo para gerar alertas preventivos aos gestores de RH quando um colaborador apresentar alto risco de saída.

### Limitações do Estudo
* **Corte Transversal:** Os dados representam um retrato estático temporal, sem considerar variáveis macroeconômicas externas ou taxas de mercado.
* **Desbalanceamento da Classe Alvo:** A classe `Attrition = Yes` é minoritária (~16%), o que torna a métrica AUC mais representativa do que a acurácia pura.

### Próximos Passos
* Implementar técnicas de reamostragem (como SMOTE ou undersampling) para balanceamento de classes.
* Testar algoritmos avançados de *Gradient Boosting* via PySpark.
