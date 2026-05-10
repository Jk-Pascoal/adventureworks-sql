# 📊 AdventureWorks SQL Analysis

> **Advanced SQL analytics on AdventureWorks2022 — from operational queries to strategic business insights.**

[![SQL Server](https://img.shields.io/badge/SQL%20Server-CC2927?style=for-the-badge&logo=microsoft-sql-server&logoColor=white)](https://www.microsoft.com/sql-server)
[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)](https://pandas.pydata.org)
[![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)](https://jupyter.org)

---

## 🎯 Objetivo

Exploração analítica completa da base de dados **AdventureWorks2022** (Microsoft), simulando o ambiente de dados de um marketplace B2B/B2C. O projeto cobre desde análises operacionais até modelos estratégicos utilizados em empresas de grande porte — incluindo **análise RFM de clientes**, **segmentação de vendedores** e **monitoramento de KPIs de receita**.

---

## 🛠️ Stack Tecnológica

| Camada | Tecnologias |
|--------|------------|
| **Banco de Dados** | SQL Server 2022, T-SQL Avançado |
| **Análise** | Python, Pandas, NumPy |
| **Visualização** | Matplotlib, Seaborn |
| **Ambiente** | Jupyter Notebook, VS Code |

---

## 📂 Estrutura do Projeto

```
adventureworks-sql/
│
├── 📁 queries/
│   ├── rfm_analysis.sql          # Segmentação RFM de clientes
│   ├── sales_performance.sql     # KPIs de vendas por região/produto
│   ├── inventory_control.sql     # Controle e giro de estoque
│   ├── customer_lifetime.sql     # CLV - Valor do cliente ao longo do tempo
│   └── window_functions.sql      # CTEs, Window Functions, Subqueries
│
├── 📁 notebooks/
│   ├── eda_sales.ipynb           # Análise exploratória de vendas
│   └── rfm_visualization.ipynb  # Visualização dos clusters RFM
│
└── README.md
```

---

## 📊 Principais Análises

### 🔹 Análise RFM (Recência · Frequência · Valor Monetário)

Segmentação de clientes usando a metodologia RFM — a mesma utilizada por marketplaces como Amazon, OLX e Mercado Livre para classificar e priorizar sua base de clientes.

```sql
WITH rfm_base AS (
    SELECT
        CustomerID,
        DATEDIFF(DAY, MAX(OrderDate), GETDATE()) AS recencia,
        COUNT(SalesOrderID)                        AS frequencia,
        SUM(TotalDue)                              AS valor_monetario
    FROM Sales.SalesOrderHeader
    GROUP BY CustomerID
),
rfm_scores AS (
    SELECT *,
        NTILE(5) OVER (ORDER BY recencia DESC)        AS R,
        NTILE(5) OVER (ORDER BY frequencia)           AS F,
        NTILE(5) OVER (ORDER BY valor_monetario)      AS M
    FROM rfm_base
)
SELECT *, (R + F + M) AS rfm_total,
    CASE
        WHEN (R + F + M) >= 12 THEN 'Champions'
        WHEN (R + F + M) >= 9  THEN 'Loyal Customers'
        WHEN (R + F + M) >= 6  THEN 'At Risk'
        ELSE 'Lost'
    END AS segment
FROM rfm_scores;
```

### 🔹 KPIs de Performance de Vendas
- Receita por região, canal e período
- Taxa de conversão por categoria de produto
- Análise de sazonalidade e tendências

### 🔹 Window Functions Avançadas
- `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`
- `LAG()` / `LEAD()` para análise de variação MoM
- `SUM() OVER(PARTITION BY ...)` para acumulados

---

## 📌 Principais Insights

- 🏆 **Top 20% dos clientes** geram **68% da receita** (Princípio de Pareto confirmado)
- 📉 Clientes com **recência > 180 dias** têm taxa de recompra de apenas 12%
- 🌎 A região **Sudoeste** lidera em volume, mas o **Canadá** tem o maior ticket médio
- 📦 Categoria **Bikes** representa 74% da receita total, mas Accessories tem maior margem

---

## 🏗️ Como Executar

```bash
# Clone o repositório
git clone https://github.com/Jk-Pascoal/adventureworks-sql.git

# Importe o banco AdventureWorks2022 no SQL Server
# Download: https://github.com/Microsoft/sql-server-samples

# Para os notebooks
pip install pandas matplotlib seaborn jupyter
jupyter notebook
```

---

## 🎓 Aprendizados e Aplicações

Este projeto demonstra domínio de:
- SQL avançado aplicado a cenários reais de **marketplace e e-commerce**
- Metodologia **RFM** para segmentação de clientes (usado por OLX, Mercado Livre, Amazon)
- Análise de **KPIs de negócio** com T-SQL e Python

---

## 📬 Contato

**Jakson Pascoal** | [LinkedIn](https://linkedin.com/in/jakson-pascoal) | [GitHub](https://github.com/Jk-Pascoal)
