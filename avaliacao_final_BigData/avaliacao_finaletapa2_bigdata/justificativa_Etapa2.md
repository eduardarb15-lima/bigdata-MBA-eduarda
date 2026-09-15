**Etapa 2** 

**1\. Ingestão.** O arquivo avaliacao\_transactions.csv, com 30.000 transações, foi carregado para o ambiente usando DuckDB, que lê o CSV diretamente e infere os tipos de cada coluna. Essa etapa apenas traz o dado bruto para dentro do ambiente de processamento, sem nenhuma transformação — é o ponto de partida do pipeline.

**2\. Criação da tabela raw.** A partir da tabela inferida automaticamente, foi criada uma segunda tabela com o schema definido explicitamente: cada coluna recebeu seu tipo correto . Isso evita que inferências automáticas que podem errar em casos de borda causem inconsistência mais adiante no pipeline.

**3\. Particionamento.** Os dados foram particionados por mês, extraído do campo timestamp. A decisão segue a estratégia definida na Etapa 1: como a área de risco da TechPay provavelmente consulta os dados por período, particionar por mês evita que uma consulta sobre um recorte de tempo precise varrer o dataset inteiro.

**4\. Bronze.** Nessa camada foram aplicados filtros de qualidade: remoção de duplicatas exatas, exclusão de transações com valor menor ou igual a zero, credit\_score fora da faixa 300-900, e risk\_score fora de 0-100. Das 30.000 linhas originais, todas as 30.000 sobreviveram nenhuma foi descartada, o que indica que a base fornecida já estava consistente dentro das regras de negócio esperadas. Ainda assim, os filtros foram mantidos porque, num cenário real de produção, esse tipo de inconsistência é comum e a etapa Bronze existe justamente para pegá-la antes que ela contamine as camadas seguintes.

**5\. Silver.** Foram adicionadas três colunas derivadas: faixa\_valor, periodo\_dia e dia\_semana. As colunas channel e merchant\_category que já vinham da base original — foram mantidas sem alteração, porque a exploração inicial dos dados mostrou que a fraude se concentra de forma desproporcional em transações via app, na categoria viagem, e durante a madrugada. Descartar essas colunas nessa etapa inviabilizaria a análise mais importante do trabalho.

**6\. Gold.** Foram criadas duas tabelas agregadas. A primeira cruza canal, categoria de estabelecimento e período do dia, calculando total de transações, total de fraudes, taxa de fraude percentual e ticket médio essa tabela alimenta diretamente a análise de risco por canal da Etapa 3\. A segunda resume o volume transacionado e o risco médio por dia e por segmento de cliente, servindo de base para a análise de tendência temporal.

**Respostas às perguntas do enunciado:**

* **Quantas linhas sobreviveram da Bronze em diante? Alguma foi descartada?** Todas as 30.000 linhas sobreviveram; nenhuma foi descartada, pois a base já estava dentro das faixas de validade esperadas.  
* **A Silver usa channel e merchant\_category? Por quê?** Sim, ambas foram mantidas, porque são as colunas que carregam o sinal mais forte de fraude nesse dataset descartá-las eliminaria a possibilidade de detectar o padrão que a avaliação pede para ser descoberto.  
* **Que agregações foram colocadas na Gold, e por quê?** Risco por canal/categoria/período do dia e resumo diário por segmento.

