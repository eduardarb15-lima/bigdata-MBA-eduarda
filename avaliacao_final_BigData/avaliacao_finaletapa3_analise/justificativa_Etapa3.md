**Etapa 3** 

Base: 30.000 transações, taxa geral de fraude de 2,5%, volume total transacionado de R$ 5.207.843,50.

**Análise 1 — Risco por canal**

* **Finding:** o canal app concentra taxa de fraude de 3,79%, mais que o dobro do canal mais seguro (pos, 1,42%), apesar de representar 40% do volume de transações (12.021 de 30.000).  
* **Insight:** o app não é apenas o canal mais usado é desproporcionalmente o mais explorado por fraude. Isso sugere que a superfície de ataque em mobile é maior do que nos canais presenciais como POS.  
* **Ação:** priorizar regras de autenticação adicional especificamente para transações via app, em vez de aplicar a mesma fricção a todos os canais igualmente o que penalizaria desnecessariamente quem usa POS ou ATM, canais já mais seguros.

**Análise 2 — Risco por categoria de estabelecimento**

* **Finding:** a categoria viagem tem taxa de fraude de 5,32%, mais que o dobro da média geral (2,5%) e a maior entre todas as categorias, mesmo representando apenas 10% do volume de transações.  
* **Insight:** compras de viagem têm características que facilitam fraude: valores mais altos, menor frequência por cliente e, tipicamente, uso em locais diferentes do habitual do cliente, o que também confunde regras de geolocalização.  
* **Ação:** criar uma régua de risco específica para a categoria viagem, com verificação adicional acima de um valor limite, em vez de aplicar o mesmo threshold de risco usado para categorias de baixo valor como alimentação.

**Análise 3 — Padrão temporal**

* **Finding:** transações na madrugada têm taxa de fraude de 3,27%, a mais alta entre os períodos do dia, contra 2,17% à tarde.  
* **Insight:** o horário de madrugada concentra menor volume de atividade legítima do cliente, então uma transação nesse período tem menor probabilidade de ser o próprio cliente agindo normalmente e maior probabilidade de ser automação ou uso não autorizado da conta.  
* **Ação:** aplicar um multiplicador de risco ao risk\_score para transações fora do horário comercial, principalmente quando combinado com os sinais das análises 1 e 2\.

---

**Descrição da ideia do BI**

1. **Para quem é o dashboard:** para a área de risco/antifraude da TechPay analistas que monitoram padrões diariamente e decidem quando escalar uma regra de bloqueio.  
2. **Que decisão ele ajuda a tomar:** onde concentrar esforço de mitigação de fraude se vale a pena endurecer autenticação por canal, criar regras específicas por categoria de estabelecimento, ou ajustar o risk\_score por horário.  
3. **Por que esses KPIs, e o que foi descartado:** os KPIs escolhidos foram os que mostraram maior variação entre grupos, ou seja, onde o dado realmente discrimina risco. Descartei um KPI genérico de "volume total por dia" como destaque principal, porque volume sozinho não indica risco; ele aparece no dashboard como contexto, não como decisão.  
4. **Quem seria o dono do painel:** um analista sênior de risco/fraude, que revisaria o painel semanalmente e teria autonomia para propor ajuste de regras de bloqueio ou escalar para o time de produto quando um padrão novo aparecer.