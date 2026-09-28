# 🏦 Otimização de Campanhas Bancárias: Previsão de Propensão de Compra (Term Deposit)

Um projeto de Ciência de Dados ponta a ponta construído sobre a base *Bank Marketing* (Kaggle). Utilizando a metodologia CRISP-DM, o objetivo deste repositório é resolver um problema clássico de ineficiência operacional em telemarketing: **substituir estratégias baseadas em volume de ligações por uma abordagem preditiva focada em conversão real.** Ao prever a probabilidade de um cliente assinar um produto de Depósito a Prazo, o modelo permite direcionar a energia da equipe de vendas para os *leads* corretos, reduzindo custos e maximizando o ROI da campanha.

---

## 📊 Resumo Executivo: Insights da Análise Exploratória (EDA)

Antes da construção dos modelos de Machine Learning, a base de dados foi rigorosamente explorada para extrair sinais de negócio e compreender o comportamento das variáveis. 

> 🔗 **[Acesse aqui a Apresentação Executiva Completa dos Insights (Google Slides)](https://canva.link/i7y0qbte0sxnb68)**

Abaixo estão as principais descobertas que guiaram as decisões de Feature Engineering e Data Prep:

### 1. O Paradoxo do Volume vs. Eficiência
* **A Ilusão de Maio:** O mês de maio concentrou quase 1/3 de todas as ligações do ano, mas entregou a **pior taxa de conversão (apenas 6,4%)**. Isso evidencia uma campanha massiva e mal segmentada, gerando puro desperdício operacional.
* **O Público Equivocado:** A classe operária (`blue-collar`) foi o segundo grupo mais contatado (cerca de 9.000 ligações), mas apresentou adesão pífia de **6,9%**. O produto claramente não possui *fit* com a realidade financeira desse nicho.

### 2. Perfis de Alta Propensão (A Mina de Ouro)
* **Extremos da Vida Financeira:** Estudantes e aposentados possuem as maiores taxas de conversão (31,4% e 25,2%, respectivamente). O produto de depósito a prazo demonstra alta aderência com públicos que estão iniciando a vida financeira ou buscando proteger seu patrimônio a longo prazo.
* **Sazonalidade Assertiva:** Meses com baixíssimo volume de contatos operacionais (março, setembro, outubro e dezembro) apresentaram taxas de conversão excepcionais, variando entre **43,9% e 50,5%**.

### 3. O Peso do Histórico e do Canal
* **O Fator Game-Changer (`poutcome`):** Clientes que aceitaram ofertas em campanhas passadas possuem uma probabilidade impressionante de **65,1%** de converter novamente no contato atual.
* **Resgate de Leads:** Mesmo clientes que rejeitaram campanhas anteriores (`failure`) possuem conversão de **14,2%** (acima da média global da base, que é de ~11,2%). Lead morno vende mais que lead totalmente frio (clientes nunca contactados convertem apenas 8,8%).
* **Meio de Contato:** Abordagens via celular (`cellular`) são quase 3 vezes mais eficientes em gerar vendas do que ligações para telefones fixos (`telephone`).

### 4. Contexto Macroeconômico e Risco
* **Economia Dita as Regras:** A conversão é estruturalmente sensível ao mercado. Períodos com taxas de juros (Euribor) e índices de emprego elevados reduzem drasticamente a disposição dos clientes em aplicar dinheiro. As variáveis econômicas apresentam **altíssima multicolinearidade**.
* **Sinal de Inadimplência:** Praticamente não há registros de calote confirmado. A categoria `unknown` em variáveis de crédito atua como um sinal genuíno de risco (o fato de o banco "não saber" a situação do cliente tem poder preditivo e não sofreu imputação forçada).

### 🚨 Prevenção de Data Leakage
A variável `duration` (tempo da chamada) possui a maior correlação com a conversão, mas **foi oficialmente descartada do escopo preditivo**. Como a duração de uma ligação só é conhecida *após* a sua realização, mantê-la no modelo criaria uma ilusão de previsão, tornando a solução inaplicável no mundo real da equipe de vendas.