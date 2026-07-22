# AdventureWorks DW2019 — SQL e visualização

Projeto de análise de vendas com o banco dimensional **AdventureWorksDW2019**, utilizando consultas T-SQL, integração entre Python e SQL Server e visualizações com Matplotlib.

O repositório demonstra um fluxo inicial e reproduzível:

```text
SQL Server → consulta analítica → DataFrame Pandas → gráfico
```

---

## Objetivo

Responder perguntas comerciais a partir do modelo dimensional da Microsoft:

- quais produtos geram maior quantidade vendida e receita?
- quais clientes acumulam maior receita?
- como a receita varia ao longo dos meses de 2013?

---

## Conteúdo implementado

### Produtos mais vendidos

O arquivo [`sql/01_analise_produtos_mais_vendidos.sql`](sql/01_analise_produtos_mais_vendidos.sql) combina:

- `FactInternetSales`;
- `DimProduct`;
- `DimProductSubcategory`.

A consulta agrega quantidade e receita, retornando os cinco produtos com maior receita total.

### Clientes com maior receita

O arquivo [`sql/02_analise_clientes_valiosos.sql`](sql/02_analise_clientes_valiosos.sql) combina:

- `FactInternetSales`;
- `DimCustomer`.

A consulta retorna os dez clientes com maior receita agregada, incluindo atributos de educação e ocupação.

### Visualização com Python

O script [`data_viz/analise_viz.py`](data_viz/analise_viz.py):

1. conecta ao SQL Server com `pyodbc`;
2. executa consultas com `pandas.read_sql`;
3. cria gráficos de produtos, clientes e vendas mensais;
4. salva as visualizações na pasta `images/`.

---

## Estrutura real do repositório

```text
adventureworks-sql/
├── data_viz/
│   └── analise_viz.py
├── images/
├── sql/
│   ├── 01_analise_produtos_mais_vendidos.sql
│   └── 02_analise_clientes_valiosos.sql
└── README.md
```

---

## Tecnologias

| Camada | Tecnologia |
|---|---|
| Banco de dados | SQL Server |
| Linguagem SQL | T-SQL |
| Conexão | PyODBC / ODBC Driver 17 |
| Análise | Python e Pandas |
| Visualização | Matplotlib |
| Modelo de dados | AdventureWorksDW2019 |

---

## Conceitos demonstrados

- consultas em modelo dimensional;
- relacionamento entre fatos e dimensões;
- `JOIN`, `GROUP BY`, `SUM`, `TOP` e `ORDER BY`;
- agregação de métricas de vendas;
- conexão Python–SQL Server;
- transformação do resultado SQL em DataFrame;
- visualização de indicadores comerciais.

---

## Como executar

### 1. Preparar o banco

Restaure o banco **AdventureWorksDW2019** em uma instância do SQL Server.

### 2. Clonar o projeto

```bash
git clone https://github.com/Jk-Pascoal/adventureworks-sql.git
cd adventureworks-sql
```

### 3. Instalar dependências

```bash
pip install pandas matplotlib pyodbc
```

Também é necessário instalar o **ODBC Driver 17 for SQL Server**.

### 4. Configurar a conexão

No arquivo `data_viz/analise_viz.py`, substitua os valores locais:

```python
server = "SEU_SERVIDOR"
database = "AdventureWorksDW2019"
```

A configuração atual utiliza autenticação integrada do Windows:

```text
Trusted_Connection=yes
```

### 5. Criar a pasta de saída e executar

```bash
mkdir images
python data_viz/analise_viz.py
```

---

## Limitações atuais

- servidor e banco ainda são configurados diretamente no script;
- não há arquivo `requirements.txt`;
- as consultas não possuem testes automatizados;
- a ordenação mensal usa o nome do mês e deve evoluir para uma chave numérica;
- resultados dependem da versão e do conteúdo local do banco restaurado;
- análise RFM, segmentação e CLV ainda não estão implementadas neste repositório.

As limitações são apresentadas explicitamente para separar o que já está reproduzível do roadmap futuro.

---

## Próximas evoluções

- mover a configuração de conexão para variáveis de ambiente;
- adicionar `requirements.txt`;
- ordenar a série mensal por número do mês;
- ampliar a biblioteca de consultas;
- implementar análise RFM como etapa independente e reproduzível;
- criar testes básicos para as consultas e transformações;
- publicar um painel com os resultados.

---

## Autor

**Jakson Pascoal** — [GitHub](https://github.com/Jk-Pascoal)
