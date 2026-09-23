Entendimento do Negócio (Business Understanding)
1. Contexto do Problema
O banco realizou campanhas de marketing direto (ligações telefônicas) para oferecer um produto financeiro: o Depósito a Prazo (Term Deposit).

2. Objetivo de Negócio
Objetivo Principal: Aumentar a taxa de conversão da campanha de marketing bancário, identificando quais clientes têm maior propensão a assinar o depósito a prazo.

Objetivo Secundário (Eficiência Operacional): Reduzir os custos de telemarketing, evitando ligar para clientes com baixíssima probabilidade de conversão.

3. Variável Target
y: Indica se o cliente contratou o depósito a prazo ('yes' ou 'no').

4. Ponto Crítico de Integridade de Negócio (Data Leakage)
⚠️ A Coluna duration: A duração da chamada telefônica possui alta correlação com a conversão, porém só é conhecida APÓS a ligação acontecer. Para criar um modelo preditivo realista (capaz de decidir para quem ligar antes de discar), a variável duration deve ser descartada.

❓ Perguntas de Negócio que Vamos Responder
Para orientar nossa futura Análise Exploratória (EDA) e a Engenharia de Features, formularemos hipóteses baseadas nas seguintes perguntas:

Perfil Sociodemográfico: Qual faixa etária, profissão, nível educacional e estado civil apresentam a maior taxa de conversão?

Histórico Financeiro/Risco: Clientes com empréstimo habitacional (housing), empréstimo pessoal (loan) ou histórico de inadimplência (default) têm menor disposição para investir em depósitos a prazo?

Esforço de Campanha: Existe um ponto de saturação no número de contatos (campaign) a partir do qual insistir na ligação reduz a chance de conversão?

Recência e Histórico de Campanhas Anteriores: Clientes que já foram contactados em campanhas passadas (pdays, previous, poutcome) têm maior propensão de conversão?

Contexto Macroeconômico: Como variáveis de mercado (taxa Euribor a 3 meses, índice de confiança do consumidor, variação no emprego) influenciam a tomada de decisão do cliente?