# 📦 Estrutura do Repositório — Todo o código do curso num só lugar

**Por quê:** em vez de entregar arquivos soltos por lab, você organiza **um único repositório Git** com tudo — os 12 labs e o projeto final. Facilita sua vida (não perde nada) e a do professor (corrige um link só).

---

## 🗂️ Estrutura de pastas

```
bigdata-curso-<seu-nome>/
├── README.md                        ← visão geral do repositório (template abaixo)
├── .gitignore                       ← exclui datasets grandes, cache, etc.
│
├── dia1_fundamentos/
│   ├── lab01_hdfs/
│   │   └── comandos.sh              ← os comandos que você rodou (rota A ou B)
│   └── lab02_sqoop/
│       └── import.sh
│
├── dia2_transformacao/
│   ├── lab03_hive_tabelas/
│   │   └── create_tables.sql
│   ├── lab04_particoes/
│   │   └── particionamento.sql
│   ├── lab05_bronze/
│   │   └── bronze.sql               ← ou .py se rota DuckDB
│   ├── lab06_silver/
│   │   └── silver.sql
│   ├── lab07_gold/
│   │   └── gold.sql
│   └── lab08_eda/
│       └── eda_queries.sql
│
├── dia3_insights_bi/
│   ├── lab09_export/
│   │   └── export.sh
│   ├── lab10_spark/
│   │   └── spark_queries.py
│   ├── lab11_dashboard/
│   │   └── dashboard.py             ← gera o dashboard_fraude.html
│   └── lab12_ml_preview/
│       └── modelo_fraude.py
│
└── avaliacao_final/
    ├── etapa1_arquitetura/
    │   ├── diagrama.png             ← export do draw.io/Excalidraw
    │   └── justificativa.md
    ├── etapa2_bigdata/
    │   ├── pipeline.sql             ← ou .py, o pipeline completo executado
    │   └── explicacao.md            ← relatório em prosa, sem código colado
    ├── etapa3_analise/
    │   ├── analises.sql
    │   ├── dashboard.html           ← ou print do Metabase
    │   └── ideia_bi.md
    └── etapa4_ml/
        ├── modelo_fraude.py         ← ou .ipynb, pipeline de ML completo
        └── explicacao.md            ← relatório em prosa, interpretação dos coeficientes
```

> Não precisa ser exatamente esses nomes de arquivo — o que importa é **a pasta bater com o lab/etapa correspondente**, pra quem for corrigir achar na hora.

---

## 🚀 Criando o repositório (passo a passo)

### 1. Criar localmente

```bash
mkdir bigdata-curso-seunome
cd bigdata-curso-seunome
git init
```

### 2. Criar o `.gitignore` — não versione datasets grandes nem lixo de execução

```bash
cat > .gitignore << 'EOF'
# datasets grandes — quem for rodar já tem os CSVs do curso, não precisa duplicar
*.csv
*.parquet

# metastore local do Hive (se você rodou fora de /opt/hive, isso aparece na sua pasta)
metastore_db/
derby.log

# cache Python
__pycache__/
*.pyc
.ipynb_checkpoints/

# ambientes virtuais
venv/
bigdata-env/
EOF
```

> ⚠️ **Sobre o `metastore_db/`**: se você passou pela mesma dor de cabeça que apareceu durante o curso (Hive criando um metastore Derby diferente a cada diretório), é bem provável que sobrem pastas `metastore_db` espalhadas pelo seu ambiente. Não versione isso — é estado local, não código.

### 3. Criar o README principal

```bash
cat > README.md << 'EOF'
# Big Data para Negócios — [Seu Nome]

Repositório com todo o código dos 12 labs e do projeto final do curso.

## Estrutura
- `dia1_fundamentos/` a `dia3_insights_bi/` — labs 1 a 12
- `avaliacao_final/` — projeto final (arquitetura, pipeline, análise/BI, machine learning)

## Ambiente usado
[Descreva aqui: cluster real ou rota sem admin, e por quê]

## Como rodar
[Instruções mínimas — ex.: "scripts .sql rodam no Hive; .py rodam com `python3 arquivo.py`"]
EOF
```

### 4. Primeiro commit

```bash
git add .
git commit -m "estrutura inicial do repositório"
```

### 5. Subir para o GitHub (ou GitLab)

```bash
# crie o repositório vazio no GitHub primeiro, depois:
git remote add origin https://github.com/seu-usuario/bigdata-curso-seunome.git
git branch -M main
git push -u origin main
```

---

## ✍️ Convenção de commits (sugestão, não obrigatório)

Um commit por lab concluído facilita mostrar progresso:

```bash
git add dia1_fundamentos/lab01_hdfs/
git commit -m "lab 01: HDFS - estrutura e replicação"

git add dia1_fundamentos/lab02_sqoop/
git commit -m "lab 02: Sqoop - import de clientes e transações"

# ... e assim por diante
```

Isso também vira histórico visível de que o trabalho foi feito ao longo do curso, não numa noite antes da entrega.

---

## 📤 O que entregar no final

Em vez de anexar arquivos soltos, entregue **o link do repositório** (GitHub/GitLab, público ou com acesso liberado para o professor):

```
https://github.com/seu-usuario/bigdata-curso-seunome
```

Isso vale tanto para os **Labs 1-12** (30% da nota) quanto para as **3 Etapas da Avaliação Final** (70% da nota) — tudo no mesmo repositório, em pastas separadas.

---

## 🔧 Troubleshooting rápido

| Problema | Solução |
|----------|---------|
| `git: command not found` | `sudo apt install git -y` |
| CSV grande foi commitado sem querer | `git rm --cached arquivo.csv` (depois de já ter no `.gitignore`) |
| Esqueceu de criar `.gitignore` antes do 1º commit | `git rm -r --cached .` e recomece o `git add` — remove tudo do índice sem apagar do disco, depois re-adiciona respeitando o `.gitignore` |
| Não tem conta no GitHub | GitLab.com também funciona, mesmos comandos, só troca a URL do `remote add` |

---

**Uma pasta, um link, tudo rastreável.** Isso facilita tanto sua organização quanto a correção.
