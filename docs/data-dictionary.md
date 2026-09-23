# 📖 Dicionário de Dados — Bank Marketing Dataset

## 📋 Visão Geral do Projeto
Este documento contém a especificação técnica e funcional do dicionário de dados do dataset **Bank Marketing** (UCI Machine Learning Repository / Kaggle). O objetivo do modelo preditivo é estimar a propensão de adesão de um cliente bancário a um produto de depósito a prazo (*term deposit*).

---

## 🏛️ 1. Dados Cadastrais do Cliente (*Bank Client Data*)

| Nome da Variável | Tipo de Dado | Descrição Funcional | Valores Possíveis / Domínio |
| :--- | :--- | :--- | :--- |
| `age` | Numérico (Inteiro) | Idade do cliente em anos. | Valores inteiros positivos (ex: $18$ a $95+$) |
| `job` | Categórico (Nominal) | Tipo de ocupação / profissão do cliente. | `'admin.'`, `'blue-collar'`, `'entrepreneur'`, `'housemaid'`, `'management'`, `'retired'`, `'self-employed'`, `'services'`, `'student'`, `'technician'`, `'unemployed'`, `'unknown'` |
| `marital` | Categórico (Nominal) | Estado civil do cliente. | `'divorced'` (inclui viúvos/divorciados), `'married'`, `'single'`, `'unknown'` |
| `education` | Categórico (Ordinal) | Nível de escolaridade concluído pelo cliente. | `'illiterate'`, `'basic.4y'`, `'basic.6y'`, `'basic.9y'`, `'high.school'`, `'professional.course'`, `'university.degree'`, `'unknown'` |
| `default` | Categórico (Binário/Nominal) | Indica se o cliente possui histórico de crédito em inadimplência (*default*). | `'no'`, `'yes'`, `'unknown'` |
| `housing` | Categórico (Binário/Nominal) | Indica se o cliente possui empréstimo imobiliário (*housing loan*) ativo. | `'no'`, `'yes'`, `'unknown'` |
| `loan` | Categórico (Binário/Nominal) | Indica se o cliente possui empréstimo pessoal (*personal loan*) ativo. | `'no'`, `'yes'`, `'unknown'` |

---

## 📞 2. Atributos da Campanha Atual (*Current Campaign*)

| Nome da Variável | Tipo de Dado | Descrição Funcional | Valores Possíveis / Domínio |
| :--- | :--- | :--- | :--- |
| `contact` | Categórico (Nominal) | Tipo do meio de comunicação utilizado no último contato. | `'cellular'`, `'telephone'` |
| `month` | Categórico (Nominal) | Mês do ano em que ocorreu o último contato. | `'jan'`, `'feb'`, `'mar'`, ..., `'nov'`, `'dec'` |
| `day_of_week` | Categórico (Nominal) | Dia da semana do último contato. | `'mon'`, `'tue'`, `'wed'`, `'thu'`, `'fri'` |
| `duration` | Numérico (Contínuo) | Duração da última chamada telefônica em segundos. | Numérico real ($\ge 0$). **[⚠️ Alerta Crítico: Data Leakage]** |

> ⚠️ **Alerta de Negócio — Data Leakage (`duration`):** 
> A duração da chamada é um forte preditor da conversão (se `duration = 0`, o target obrigatoriamente será `y = 'no'`). No entanto, o tempo de conversa **só é conhecido APÓS a ligação ter sido encerrada**. Para modelos preditivos cujo objetivo seja selecionar previamente quais clientes contactar, a coluna `duration` **deve ser removida do pipeline de treino** para evitar vazamento de dados.

---

## 📊 3. Atributos de Histórico e Recência (*Campaign History*)

| Nome da Variável | Tipo de Dado | Descrição Funcional | Valores Possíveis / Domínio |
| :--- | :--- | :--- | :--- |
| `campaign` | Numérico (Inteiro) | Número total de contatos realizados para este cliente durante a campanha atual. | Inteiro ($\ge 1$, incluindo o último contato) |
| `pdays` | Numérico (Inteiro) | Número de dias decorridos desde o último contato de uma campanha anterior. | Inteiro. O valor `999` significa que o cliente **nunca foi contactado** anteriormente. |
| `previous` | Numérico (Inteiro) | Número de contatos realizados com este cliente antes da campanha atual. | Inteiro ($\ge 0$) |
| `poutcome` | Categórico (Nominal) | Resultado/desfecho da campanha de marketing anterior. | `'failure'`, `'nonexistent'`, `'success'` |

---

## 📈 4. Contexto Socioeconômico (*Economic Indicators*)

| Nome da Variável | Tipo de Dado | Descrição Funcional | Valores Possíveis / Domínio |
| :--- | :--- | :--- | :--- |
| `emp.var.rate` | Numérico (Contínuo) | Taxa de variação do emprego (*Employment Variation Rate*) — Indicador Trimestral. | Decimal (ex: $-3.4$ a $1.4$) |
| `cons.price.idx` | Numérico (Contínuo) | Índice de Preços ao Consumidor (*Consumer Price Index*) — Indicador Mensal. | Decimal (ex: $92.201$ a $94.767$) |
| `cons.conf.idx` | Numérico (Contínuo) | Índice de Confiança do Consumidor (*Consumer Confidence Index*) — Indicador Mensal. | Decimal (ex: $-50.8$ a $-26.9$) |
| `euribor3m` | Numérico (Contínuo) | Taxa Euribor de 3 meses (*Euribor 3 Month Rate*) — Indicador Diário. | Decimal (ex: $0.634$ a $5.045$) |
| `nr.employed` | Numérico (Contínuo) | Número médio de empregados no mercado (*Number of Employees*) — Indicador Trimestral. | Decimal (ex: $4963.6$ a $5228.1$) |

---

## 🎯 5. Variável Alvo (*Target Variable*)

| Nome da Variável | Tipo de Dado | Descrição Funcional | Valores Possíveis |
| :--- | :--- | :--- | :--- |
| `y` | Categórico (Binário) | Indica se o cliente aceitou e contratou o depósito a prazo (*term deposit*). | `'yes'` (Classe Positiva), `'no'` (Classe Negativa) |

---

## 🛠️ 6. Recomendações para Feature Engineering e Data Prep

1. **Tratamento de Categoria `'unknown'`:**
   - Presente nas colunas `job`, `marital`, `education`, `default`, `housing` e `loan`. Pode ser tratada como uma categoria própria (já que a falta da informação pode conter sinal preditivo) ou imputada via moda/KNN no Módulo de Data Prep.
2. **Tratamento de `pdays`:**
   - Como `999` representa descontinuidade (cliente não contactado), sugere-se criar uma flag binária `flg_contacted_prev` ($1$ se `pdays` $\neq 999$, caso contrário $0$) e reajustar `pdays` para evitar impactos em algoritmos lineares.
3. **Tratamento de Multicolinearidade:**
   - As variáveis macroeconômicas (`euribor3m`, `emp.var.rate`, `nr.employed`) apresentam elevadas correlações entre si. A avaliação via matriz de correlação e cálculo do VIF (*Variance Inflation Factor*) é recomendada na etapa de Feature Selection.