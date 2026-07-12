# Prompts do Curso — Data-Driven Mindset

Este documento reúne **todos os prompts** que devem ser colados no Gemini (ou em outra LLM de sua preferência) ao longo dos três capítulos do curso. Estão organizados na ordem exata em que aparecem nas aulas, para que você copie e cole diretamente no Google Colab / Gemini durante a prática.

> 💡 Todos os prompts usam o Google Colab (colab.research.google.com) como ambiente de execução.

---

## Capítulo 1 — Fundamentos de Dados e Análise

### Prompt 1 — Classificar Tipos de Dados (Titanic)

**Contexto:** exercício prático 1, sobre classificação de colunas em estruturadas / semiestruturadas / não estruturadas.

```
Aja como um cientista de dados. Vou te passar as colunas do dataset Titanic
(survived, pclass, sex, age, fare, embarked, class, who, deck).

Para cada coluna, classifique o tipo de dado como estruturado, não
estruturado ou semiestruturado, explique o motivo em uma frase, e gere um
código Python usando pandas com sns.load_dataset('titanic') para exibir o
tipo (dtype) de cada coluna.
```

---

### Prompt 2 — Análise Exploratória (Titanic)

**Contexto:** exercício prático 2, primeira análise exploratória de dados (EDA).

```
Estou no Google Colab explorando o dataset Titanic, já embutido na
biblioteca seaborn.

Gere um código Python que carregue o dataset com sns.load_dataset('titanic'),
mostre as 5 primeiras linhas, um resumo estatístico das colunas numéricas e
a contagem de valores nulos por coluna, com comentários curtos explicando
cada parte.
```

---

### Prompt 3 — Primeiro Gráfico (Titanic)

**Contexto:** exercício prático 3, visualização de dados.

```
Usando o dataset Titanic (sns.load_dataset('titanic')) no Google Colab,
gere um código Python com matplotlib ou seaborn que crie um gráfico de
barras mostrando a taxa de sobrevivência por classe (pclass).

Inclua título e rótulos nos eixos, e comente o que cada linha do código faz.
```

---

## Capítulo 2 — Fundamentos da Análise de Dados

### Prompt 1 — Criando o Dataset TelecomX + Primeiras Visualizações

**Contexto:** Ciclo Prático 1 — distribuição de variáveis (histograma e gráfico de barras).

```
Tenho um dataset fictício de uma operadora de telecom chamada TelecomX.

Gere um código Python que:
1. Crie um DataFrame df com colunas: idade, salario, gastos_mensais,
   satisfacao (1-5), chamadas_suporte, meses_ultima_compra,
   duracao_contrato_meses e churn (0/1), com valores fictícios realistas
   para 500 linhas.
2. Plote um histograma da distribuição de idade.
3. Plote um gráfico de barras com a proporção de clientes por churn (0 vs 1).

Use pandas, matplotlib e seaborn, e comente cada bloco do código.
```

---

### Prompt 2 — Boxplot e Detecção de Outliers

**Contexto:** Ciclo Prático 2 — boxplot e regra do IQR.

```
Usando o mesmo DataFrame df do TelecomX criado anteriormente (com as
colunas salario e churn):
1. Gere um boxplot da variável salario, segmentado por churn (0 vs 1).
2. Calcule os outliers de salario com a regra do IQR (1.5x o intervalo
   interquartil) e informe quantos foram encontrados.

Comente cada bloco do código.
```

---

### Prompt 3 — Correlação e Pairplot

**Contexto:** Ciclo Prático 3 — heatmap de correlação e pairplot.

```
Usando o mesmo DataFrame df do TelecomX:
1. Gere um heatmap da matriz de correlação de todas as variáveis numéricas,
   com anotações dos valores.
2. Gere um pairplot das variáveis numéricas, colorido pela variável churn
   (hue='churn').

Comente cada bloco do código.
```

---

### Prompt 4 — Correlação vs. Causalidade

**Contexto:** Ciclo Prático 4 — análise das correlações mais fortes e diagrama causal com networkx.

```
Com base na matriz de correlação do df do TelecomX:
1. Liste as 3 correlações mais fortes com a variável churn.
2. Para cada uma, avalie se sugere uma relação causal plausível ou apenas
   uma variável de confusão.
3. Gere um código Python com networkx que desenhe um diagrama causal com
   duracao_contrato_meses, satisfacao e churn como hipótese a ser testada.
```

---

## Capítulo 3 — Principais Métodos de Advanced Analytics

### Prompt 1 — Classificação (Random Forest)

**Contexto:** Ciclo Prático 1 — classificação de churn com Random Forest.

```
Você é um cientista de dados iniciante estudando classificação.

Gere um código em Python, para o Google Colab, que:
1. Crie um conjunto de dados fictício de clientes de telecom
   (features: tempo de contrato, valor da fatura, nº de chamados
   ao suporte, rótulo "cancelou" sim/não).
2. Divida os dados em treino e teste (80/20).
3. Treine um modelo de classificação com Random Forest
   usando scikit-learn.
4. Mostre a matriz de confusão e o F1-score do modelo.
5. Explique, em comentários simples no código, o que cada
   bloco faz — estou aprendendo.
```

---

### Prompt 2 — Séries Temporais (Prophet)

**Contexto:** Ciclo Prático 2 — previsão de demanda com Prophet.

```
Você é um cientista de dados iniciante estudando séries temporais.

Gere um código em Python, para o Google Colab, que:
1. Crie uma série temporal fictícia de vendas diárias de um
   produto ao longo de 2 anos, com sazonalidade semanal.
2. Use a biblioteca Prophet para treinar um modelo de previsão.
3. Preveja a demanda para os próximos 30 dias.
4. Plote um gráfico com o histórico e a previsão.
5. Explique, em comentários simples no código, o que cada
   bloco faz — estou aprendendo.
```

---

### Prompt 3 — Clustering (K-Means)

**Contexto:** Ciclo Prático 3 — segmentação de clientes de e-commerce.

```
Você é um cientista de dados iniciante estudando clustering.

Gere um código em Python, para o Google Colab, que:
1. Crie um conjunto fictício de clientes de e-commerce
   (features: frequência de compra, ticket médio, dias desde
   a última compra).
2. Padronize as features com StandardScaler.
3. Aplique o método Elbow para sugerir o número ideal de
   clusters (k).
4. Treine um modelo K-Means com esse k e plote os grupos.
5. Explique, em comentários simples no código, o que cada
   bloco faz — estou aprendendo.
```

---

### Prompt 4 — Otimização (Roteirização)

**Contexto:** Ciclo Prático 4 — otimização de rotas de entrega com OR-Tools.

```
Você é um cientista de dados iniciante estudando otimização.

Gere um código em Python, para o Google Colab, que:
1. Crie 8 pontos fictícios de entrega com coordenadas (x, y).
2. Use a biblioteca OR-Tools (ou uma busca gulosa simples)
   para encontrar a rota de menor distância total que passa
   por todos os pontos, partindo de um depósito.
3. Plote o mapa com a rota escolhida.
4. Mostre a distância total da rota.
5. Explique, em comentários simples no código, o que cada
   bloco faz — estou aprendendo.
```

---

## Resumo — Total de Prompts

| Capítulo | Nº de prompts | Temas |
|---|---|---|
| 1 — Fundamentos de Dados e Análise | 3 | Classificação de tipos de dados, EDA, visualização |
| 2 — Fundamentos da Análise de Dados | 4 | Distribuição, outliers/IQR, correlação/pairplot, causalidade |
| 3 — Advanced Analytics | 4 | Classificação, séries temporais, clustering, otimização |
| **Total** | **11** | |
