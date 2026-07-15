# Data-Driven Mindset · Capítulo 3 — Adaptação para Jurídico & Auditoria Bancária

> Contexto da turma: profissionais da área de **Jurídico e Auditoria Interna** de um grande banco brasileiro (fictício, chamado aqui de **Banco Aurora S.A.**). Os mesmos 4 métodos de Advanced Analytics do capítulo original (Classificação, Séries Temporais, Clustering, Otimização) são recontextualizados com dados fictícios de contencioso, compliance e auditoria.

---

## Sumário

1. [Ciclo Prático 1 — Classificação: Risco de Perda em Ações Judiciais](#ciclo-1)
2. [Ciclo Prático 2 — Séries Temporais: Volume de Novos Processos e Reportes ao COAF](#ciclo-2)
3. [Ciclo Prático 3 — Clustering: Segmentação de Não Conformidades de Auditoria](#ciclo-3)
4. [Ciclo Prático 4 — Otimização: Roteirização de Visitas de Auditoria In Loco](#ciclo-4)
5. [Teste seus conhecimentos — versão Jurídico/Auditoria](#quiz)
6. [Métricas de avaliação no contexto jurídico](#metricas)

---

<a id="ciclo-1"></a>
## Ciclo Prático 1 · Classificação
### Case: Prever se uma ação judicial terá desfecho desfavorável ao banco

**Contexto de negócio:** o Contencioso Cível do Banco Aurora recebe milhares de novas ações por ano (revisão de contrato, cobrança indevida, negativação indevida, danos morais). Hoje a triagem de risco é manual e depende da experiência do advogado. A área quer um modelo que **classifique, no momento da citação, se o processo tem alta probabilidade de condenação (procedente)**, para priorizar acordo antecipado ou reforço de provisão contábil.

**O que já sabemos:** Classificação prevê uma categoria a partir de dados rotulados — aqui, "procedente" (sim/não), usando o histórico de milhares de processos já encerrados como base de treino.

**Desafios típicos deste case:**
- *Classes desbalanceadas*: poucos processos realmente graves (ex.: fraude comprovada) entre milhares de reclamações rotineiras.
- *Excesso de features*: comarca, vara, tipo de produto, valor da causa, tempo de relacionamento do cliente, histórico de reclamações — nem tudo é relevante.
- *Interpretabilidade*: o Jurídico precisa **explicar** por que o modelo classificou um processo como "alto risco", não apenas confiar numa caixa-preta (isso pesa em decisões de provisionamento contábil, sujeitas a auditoria externa).

#### Prompt para a LLM

```
Você é um cientista de dados iniciante estudando classificação, aplicado ao
contexto jurídico de um banco.

Gere um código em Python, para o Google Colab, que:
1. Crie um conjunto de dados fictício de 500 processos judiciais cíveis
   contra um banco (features: valor da causa, tipo de ação [revisão de
   contrato / cobrança indevida / negativação indevida / danos morais],
   tempo de relacionamento do cliente em meses, quantidade de reclamações
   anteriores do cliente, comarca [capital/interior], rótulo "procedente"
   sim/não).
2. Divida os dados em treino e teste (80/20).
3. Treine um modelo de classificação com Random Forest usando scikit-learn.
4. Mostre a matriz de confusão e o F1-score do modelo.
5. Mostre também a importância de cada feature (feature_importances_), para
   que um advogado sem conhecimento técnico entenda o que mais pesa na
   decisão do modelo.
6. Explique, em comentários simples no código, o que cada bloco faz —
   estou aprendendo.
```

**Entendendo o código (mapeamento):**

| Trecho | O que faz no contexto jurídico |
|---|---|
| `train_test_split(...)` | Separa processos já encerrados: 80% para o modelo "aprender" os padrões de condenação, 20% para testar se ele acerta em casos que nunca viu. |
| `RandomForestClassifier()` | Cria o modelo: várias "árvores de decisão" (como se fossem pareceres jurídicos independentes) que votam entre si para decidir se o processo tende a ser procedente. |
| `.fit(X_train, y_train)` | Treina o modelo com o histórico real de desfechos. |
| `confusion_matrix(...)` | Mostra os acertos e erros: quantos processos "vai perder" e "não vai perder" o modelo classificou certo — e onde errou. |
| `f1_score(...)` | Resume o desempenho em um número só, equilibrando falso positivo (provisionar sem necessidade) e falso negativo (não provisionar um caso que era, de fato, perda). |
| `feature_importances_` | Mostra ao Jurídico *por que* o modelo decidiu — essencial para auditoria e explicabilidade. |

**Discussão em sala:** o que pesa mais, um falso positivo (provisionar recursos para um processo que na verdade será ganho) ou um falso negativo (deixar de provisionar um processo que será perdido)? Como isso se conecta com Precision e Recall?

---

<a id="ciclo-2"></a>
## Ciclo Prático 2 · Séries Temporais
### Case: Prever o volume mensal de novos processos e de reportes ao COAF

**Contexto de negócio:** a área de Compliance/AML do Banco Aurora precisa dimensionar a equipe de analistas responsáveis por investigar operações suspeitas e enviar Comunicações de Operações Suspeitas (COS) ao COAF. O volume varia por sazonalidade (ex.: aumento no fim de ano, datas de vencimento de tributos, campanhas de crédito). Prever a demanda futura ajuda no planejamento de headcount e evita atraso no cumprimento de prazos regulatórios.

**O que já sabemos:** Séries temporais preveem valores futuros respeitando a ordem cronológica — aqui, vamos prever o número diário/semanal de novas comunicações de operações suspeitas geradas para os próximos 30 dias.

#### Prompt para a LLM

```
Você é um cientista de dados iniciante estudando séries temporais, aplicado
ao contexto de compliance bancário.

Gere um código em Python, para o Google Colab, que:
1. Crie uma série temporal fictícia do número diário de Comunicações de
   Operações Suspeitas (COS) geradas internamente por um banco ao longo de
   2 anos, com sazonalidade semanal (mais alertas em dias úteis) e um pico
   sazonal no fim de cada trimestre (fechamento fiscal).
2. Use a biblioteca Prophet para treinar um modelo de previsão.
3. Preveja o volume de comunicações para os próximos 30 dias.
4. Plote um gráfico com o histórico e a previsão.
5. Explique, em comentários simples no código, o que cada bloco faz —
   estou aprendendo, e traduza a conclusão em linguagem de negócio: o
   time de compliance vai precisar reforçar a equipe no próximo mês?
```

**Entendendo o código (mapeamento):**

| Trecho | O que faz no contexto de compliance |
|---|---|
| `Prophet()` | Cria o modelo de previsão, já preparado para capturar sazonalidade semanal (dias úteis) e de fechamento trimestral automaticamente. |
| `.fit(df)` | Treina o modelo com o histórico real de comunicações geradas. |
| `make_future_dataframe(periods=30)` | Cria as datas futuras — os próximos 30 dias que a área de compliance quer planejar. |
| `.predict(...) + plot(...)` | Gera e desenha a previsão — permite decidir, com antecedência, se será preciso reforçar a equipe de analistas de AML. |

**Discussão em sala:** por que prever apenas "a média histórica" não seria suficiente aqui? Que outros eventos externos (ex.: mudança regulatória do BACEN, operação policial de grande repercussão) poderiam quebrar o padrão sazonal do modelo?

---

<a id="ciclo-3"></a>
## Ciclo Prático 3 · Clustering
### Case: Segmentar não conformidades encontradas pela Auditoria Interna

**Contexto de negócio:** a Auditoria Interna do Banco Aurora registra, a cada ciclo de auditoria em agências e áreas de negócio, dezenas de "achados" (não conformidades) — desde falhas simples de documentação até indícios de fraude. Sem uma segmentação clara, é difícil identificar **padrões recorrentes** que indicam falha sistêmica de controle interno (e não um caso isolado).

**O que já sabemos:** Clustering agrupa dados semelhantes sem rótulos prévios — aqui, vamos segmentar achados de auditoria por características de gravidade e recorrência, sem dizer ao modelo de antemão quais são os grupos "certos".

#### Prompt para a LLM

```
Você é um cientista de dados iniciante estudando clustering, aplicado à
auditoria interna de um banco.

Gere um código em Python, para o Google Colab, que:
1. Crie um conjunto fictício de 300 achados de auditoria interna em
   agências bancárias (features: nota de severidade do achado de 1 a 5,
   número de recorrências do mesmo tipo de achado nos últimos 12 meses,
   tempo médio em dias para a área responsável corrigir o achado).
2. Padronize as features com StandardScaler.
3. Aplique o método Elbow para sugerir o número ideal de clusters (k).
4. Treine um modelo K-Means com esse k e plote os grupos.
5. Explique, em comentários simples no código, o que cada bloco faz — e
   sugira, em linguagem de negócio, um nome para cada cluster encontrado
   (ex.: "falhas pontuais de baixo risco" vs. "falhas recorrentes
   estruturais que exigem plano de ação corporativo").
```

**Entendendo o código (mapeamento):**

| Trecho | O que faz no contexto de auditoria |
|---|---|
| `StandardScaler()` | Coloca todas as features na mesma escala — sem isso, "tempo de correção em dias" dominaria a "nota de severidade" (que vai só de 1 a 5). |
| `for k in range(1,10): ...` | Testa vários números de grupos e guarda o erro de cada um — é a base do método Elbow. |
| `KMeans(n_clusters=k).fit(...)` | Treina o modelo final, agrupando cada achado de auditoria a um cluster. |
| `plt.scatter(..., c=labels)` | Visualiza os grupos formados — sem rótulo prévio, o próprio modelo descobre, por exemplo, um cluster de "falhas recorrentes de alto risco" que merece um plano de ação corporativo. |

**Discussão em sala:** como esse tipo de segmentação poderia embasar a priorização do plano anual de auditoria (risk-based audit plan)?

---

<a id="ciclo-4"></a>
## Ciclo Prático 4 · Otimização
### Case: Roteirização das visitas de auditoria in loco às agências

**Contexto de negócio:** a equipe de Auditoria Interna do Banco Aurora precisa visitar fisicamente um conjunto de agências numa determinada regional dentro de uma semana. Minimizar a distância total percorrida reduz custo de deslocamento e libera mais tempo dos auditores para o trabalho de campo em vez de trânsito.

**O que já sabemos:** Otimização busca a melhor combinação possível dentro de restrições — aqui, vamos otimizar a rota de visitas às agências para minimizar a distância total percorrida pela equipe de auditoria.

#### Prompt para a LLM

```
Você é um cientista de dados iniciante estudando otimização, aplicado ao
planejamento de auditoria in loco de um banco.

Gere um código em Python, para o Google Colab, que:
1. Crie 8 pontos fictícios representando agências bancárias a serem
   auditadas, com coordenadas (x, y).
2. Use a biblioteca OR-Tools (ou uma busca gulosa simples) para encontrar
   a rota de menor distância total que passa por todas as agências,
   partindo da sede regional do banco.
3. Plote o mapa com a rota escolhida.
4. Mostre a distância total da rota e uma estimativa de economia de tempo
   comparada a uma visita em ordem aleatória.
5. Explique, em comentários simples no código, o que cada bloco faz —
   estou aprendendo.
```

**Entendendo o código (mapeamento):**

| Trecho | O que faz no contexto de auditoria in loco |
|---|---|
| `distance_matrix(...)` | Calcula a distância entre cada par de agências — a "matéria-prima" do problema de roteirização. |
| `RoutingModel(...)` | Monta o problema: quantas agências, onde a equipe começa (sede regional) e qual objetivo minimizar (distância total). |
| `SolveWithParameters(...)` | Testa combinações de ordem de visita e converge para a rota mais curta. |
| `plt.plot(rota)` | Desenha a sequência de agências a visitar — diferente dos ciclos anteriores, aqui não há "treino" com dados históricos: é um problema de busca da melhor solução possível. |

**Discussão em sala:** que outras restrições reais um auditor enfrentaria que o modelo simplificado acima não capturou (ex.: horário de expediente da agência, tempo mínimo de auditoria por agência, disponibilidade de carro/motorista)?

---

<a id="quiz"></a>
## Teste seus conhecimentos — versão Jurídico/Auditoria

Para cada afirmação, identifique: **Classificação · Regressão · Séries Temporais · Clustering · Associação**

1. Prever se um processo judicial em curso será julgado procedente ou improcedente, com base no histórico de casos semelhantes.
2. Analisar o volume de reportes de operações suspeitas ao COAF ao longo dos últimos 3 anos para prever a demanda de analistas de compliance no próximo trimestre.
3. Agrupar agências bancárias com base no perfil de achados de auditoria para direcionar o plano de auditoria do próximo ano.
4. Identificar quais tipos de não conformidade costumam aparecer juntos numa mesma agência (ex.: falha de KYC costuma vir acompanhada de falha de custódia de documentos).
5. Estimar o valor provável de indenização a ser pago em uma ação de dano moral, com base no valor da causa, comarca e histórico de sentenças semelhantes.

**Gabarito**

1. Classificação
2. Séries Temporais
3. Clustering
4. Associação
5. Regressão

---

<a id="metricas"></a>
## Métricas de avaliação no contexto jurídico

**Matriz de confusão aplicada ao case de risco de perda judicial:**

| | Previsto: Procedente | Previsto: Improcedente |
|---|---|---|
| **Real: Procedente** | Verdadeiro Positivo | Falso Negativo |
| **Real: Improcedente** | Falso Positivo | Verdadeiro Negativo |

- **Precision:** das vezes que o modelo classificou um processo como "vai perder", quantas realmente eram perda? Um Precision baixo significa provisionar recursos desnecessariamente para casos que na verdade seriam ganhos.
- **Recall:** dos processos que realmente resultaram em perda, quantos o modelo conseguiu identificar antes? Um Recall baixo é mais grave aqui — significa deixar de provisionar (ou de buscar acordo) em casos que vão gerar prejuízo real, o que pode inclusive chamar atenção de auditoria externa e do BACEN.
- **F1-score:** equilibra as duas métricas — útil para comparar diferentes versões do modelo de triagem de risco processual.

> Nota pedagógica: no contexto jurídico/regulatório, geralmente vale mais priorizar **Recall** (não deixar passar um caso de risco real) do que Precision (evitar alarmes falsos), já que o custo de não provisionar um processo que será perdido tende a ser maior — inclusive reputacional — do que provisionar um pouco a mais por precaução.

---

### Notebooks de referência (a adaptar)

Os 4 notebooks originais do capítulo (Classificação, Séries Temporais, Clustering, Otimização) podem ser reaproveitados como base técnica — trocando apenas o dataset fictício pelos exemplos acima, mantendo toda a lógica de código (train_test_split, Prophet, KMeans, OR-Tools) inalterada.
