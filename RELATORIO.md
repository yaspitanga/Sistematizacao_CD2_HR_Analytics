# Relatório do Projeto — Data Science II (HR Analytics)

## 1. Problema e Dados
O objetivo deste trabalho é entender o motivo da saída de funcionários (*turnover*) na empresa e criar modelos para prever desligamentos futuros, ajudando a equipe de Recursos Humanos nas tomadas de decisão.
* **Dataset utilizado:** `HR_Analytics.csv`
* **Informações analisadas:** Idade, salário, tempo de empresa, nível do cargo e satisfação no trabalho.

---

## 2. Limpeza e Preparação dos Dados
* Leitura do arquivo usando **PySpark DataFrames**.
* Remoção de linhas com dados ausentes ou incompletos.
* Padronização das colunas numéricas e conversão de textos (como "Sim" e "Não") em formatos numéricos para o treinamento dos modelos.

---

## 3. Análise Exploratória (Spark SQL)
Foram feitas 5 consultas em **Spark SQL** combinadas com gráficos (Seaborn/Matplotlib) para identificar tendências de saída por departamento, faixa salarial e tempo de casa.

---

## 4. Modelos Preditivos (Resultados)
Comparamos dois algoritmos para prever se um funcionário vai sair ou continuar na empresa:

1. **Árvore de Decisão:** 
   * Acurácia: 86,53%
   * AUC: 0,73
2. **Random Forest (Ensemble):** 
   * Acurácia: 86,53%
   * AUC: 0,83

**Conclusão da comparação:** O **Random Forest** foi o melhor modelo, pois atingiu a mesma acurácia com uma capacidade muito maior de ranquear e identificar os funcionários com maior risco real de demissão (AUC 0,83).

---

## 5. Grupos de Funcionários (Clusterização)
Usando o algoritmo **K-Means**, dividimos a base em 3 perfis claros de funcionários:

* **Grupo 0 (Início de Carreira):** Funcionários mais jovens (média de 34 anos), salário médio de $3.938 e menor tempo de empresa. É o grupo com maior risco de saída por busca de crescimento.
* **Grupo 1 (Profissionais Plenos/Seniores):** Média de 40 anos, salário médio de $10.055 e estabilidade intermediária.
* **Grupo 2 (Liderança):** Média de 47 anos, alto salário médio ($17.722) e mais de 14 anos de empresa. Grupo com raros desligamentos.

---

## 6. Conclusões e Próximos Passos
* **Ação recomendada:** Criar planos de retenção e carreira focados no **Grupo 0**, além de usar o modelo Random Forest no RH para receber alertas preventivos de saída.
* **Limitações:** O dataset representa apenas um momento específico da empresa, sem avaliar fatores de mercado externo.
