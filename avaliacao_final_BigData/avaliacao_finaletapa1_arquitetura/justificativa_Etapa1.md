**1\. Por que essa ferramenta de ingestão, e qual seria a alternativa?**

A ingestão foi feita via upload direto do arquivo avaliacao\_transactions.csv no Google Colab, que assume o papel de camada de ingestão nesse desenho. Essa escolha se justifica pelo cenário: não há um banco de origem real da TechPay para conectar, apenas um arquivo já extraído. Em um ambiente de produção real, a alternativa seria uma ferramenta como o Sqoop para importar diretamente de um banco transacional, ou um conector de streaming se a origem fosse eventos em tempo real. Optei por não usar essas ferramentas aqui porque exigiria um banco de dados de origem que o cenário não fornece  usá-las seria simular uma conexão que não existe.

**2\. Qual a estratégia de particionamento, e por quê?**

Os dados são particionados por mês do timestamp. A justificativa é o padrão de consulta esperado: a área de risco da TechPay provavelmente vai analisar tendências e comparar períodos, então particionar por mês evita que uma consulta precise varrer o dataset inteiro para responder uma pergunta sobre um recorte de tempo. Como segunda camada de organização, dentro de cada partição de mês, os dados também podem ser organizados por channel, já que essa coluna aparece repetidamente nas perguntas de negócio da Etapa 3\.

**3\. Onde estão os pontos de falha, e o que quebraria primeiro se o volume multiplicasse por 100?**

Rodando localmente no Colab, o ponto de falha mais imediato é a memória RAM da sessão. Hoje o dataset tem 30 mil linhas e cabe inteiro em memória sem problema. Multiplicado por 100, operações que hoje rodam em pandas/DuckDB sem esforço começaram a estourar a RAM disponível do ambiente gratuito do Colab, especialmente nas etapas de agregação da camada Gold. A mitigação seria migrar de pandas para processamento em chunks ou para um motor que processa fora da memória, como Spark local ou DuckDB com leitura direta de Parquet particionado.

4\. O que mudaria se a detecção de fraude precisasse ser em tempo real, não em lote?

O desenho atual é em batch: os dados chegam, passam pelas camadas Bronze/Silver/Gold e só depois viram análise. Para tempo real, a ingestão mudaria de um upload de arquivo para um stream contínuo de eventos, e a camada de processamento precisaria rodar regras de decisão durante a ingestão, não depois. O dashboard também deixaria de ser um painel para a diretoria revisar semanalmente e passaria a ser um sistema de alerta, disparado notificação no momento em que uma transação suspeita ocorre, em vez de aparecer numa análise agregada do dia seguinte.  
