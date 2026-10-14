
# 📊 Dashboard Executivo de Vendas - Contoso (Power BI + SQL Server + Docker)

![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![SQL Server](https://img.shields.io/badge/SQL_Server-CC292B?style=for-the-badge&logo=microsoftsqlserver&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)

---

## 📌 Visão Geral do Projeto

Este projeto consiste no desenvolvimento end-to-end de uma solução analítica executiva de vendas para a empresa fictícia **Contoso**. O objetivo é fornecer à alta gestão visibilidade sobre o Faturamento Total, Custo, Lucro e Desempenho por Categoria de Produtos e Lojas.

A solução abrange desde a **subida de infraestrutura em container Docker (SQL Server no Linux Ubuntu)** até a **modelagem multidimensional (Star Schema)** e a **construção do Dashboard interativo no Power BI Desktop**.

---

## 🏗️ Arquitetura da Solução


```

+-----------------------------------------------------------------------+
|                         LINUX UBUNTU (HOST)                           |
|                                                                       |
|  +---------------------------------+  +----------------------------+  |
|  |     Container Docker            |  |    Azure Data Studio       |  |
|  |   (SQL Server 2022)             |  |  (Validação & Consultas)   |  |
|  |   - Database: ContosoVendasDB   |  +----------------------------+  |
|  |   - Port: 1433                  |                                  |
|  +---------------------------------+                                  |
|                  ^                                                    |
|                  | (Conexão via Rede NAT / Port 1433)                 |
|                  v                                                    |
|  +-----------------------------------------------------------------+  |
|  |                     MÁQUINA VIRTUAL WINDOWS 11                  |  |
|  |                                                                 |  |
|  |   +---------------------------------------------------------+   |  |
|  |   |                  Power BI Desktop                       |   |  |
|  |   |   - Modelo Star Schema (FactVendas, DimCliente, etc.)   |   |  |
|  |   |   - Dashboard Executivo Interativo                      |   |  |
|  |   +---------------------------------------------------------+   |  |
|  +-----------------------------------------------------------------+  |
+-----------------------------------------------------------------------+

```

---

## 🗄️ Modelo de Dados (Star Schema)

A base de dados `ContosoVendasDB` foi estruturada seguindo as melhores práticas de modelagem dimensional (Esquema em Estrela):

* **FactVendas:** Tabela fato contendo as transações de vendas, quantidades e valores totais.
* **DimCliente:** Cadastro de clientes e localização geográfica.
* **DimProduto:** Cadastro de produtos, categorias e custos unitários.
* **DimLoja:** Mapeamento de lojas físicas e canal E-commerce.

```sql
-- Estrutura principal do banco
ContosoVendasDB
├── FactVendas (ID_Venda, DataVenda, ID_Cliente, ID_Produto, ID_Loja, Quantidade, ValorTotal)
├── DimCliente (ID_Cliente, NomeCliente, Estado)
├── DimProduto (ID_Produto, NomeProduto, Categoria, PrecoUnitario, CustoUnitario)
└── DimLoja (ID_Loja, NomeLoja, Cidade, Estado)

```

---

## 📐 Métricas & Regras de Negócio (DAX)

Para suportar as análises executivas, foram desenvolvidas medidas utilizando **DAX (Data Analysis Expressions)**:

* **Faturamento Total:**
```dax
Faturamento Total = SUM(FactVendas[ValorTotal])

```


* **Custo Total:**
```dax
Custo Total = 
SUMX(
    FactVendas,
    FactVendas[Quantidade] * RELATED(DimProduto[CustoUnitario])
)

```


* **Lucro Total:**
```dax
Lucro Total = [Faturamento Total] - [Custo Total]

```


* **Total de Unidades Vendidas:**
```dax
Total Unidades = SUM(FactVendas[Quantidade])

```



---

## 📈 Destaques do Dashboard Executivo

* **Cartões de KPI:** Leitura imediata de Faturamento, Custo e Total de Unidades.
* **Filtros Interativos (Tile/Botões):** Segmentação temporal por Mês de forma limpa.
* **Evolução Mensal:** Gráfico de linhas comparando a trajetória de Faturamento vs. Lucro.
* **Análise por Categoria:** Gráfico de barras horizontais detalhando a margem de lucro por segmento de produtos.
* **Desempenho por Loja:** Colunas informando o volume faturado em cada ponto de venda / e-commerce.

---

## 🛠️ Tecnologias Utilizadas

* **Power BI Desktop:** Modelagem de dados, DAX e UI/UX Dashboard Design.
* **SQL Server 2022:** Armazenamento e estruturação relacional.
* **Docker / Docker Compose:** Conteinerização do banco de dados no ambiente Linux.
* **Azure Data Studio:** Gerenciamento de scripts T-SQL e validação das tabelas.
* **Ubuntu Linux & Windows 11 (VM):** Infraestrutura hospedeira e ambiente de homologação.

---

## 👤 Autor

**Mário Rossatti Junior**

*Desenvolvedor & Analista de Dados / BI*

* GitHub: [@rossatti-dataworker](https://github.com/rossatti-dataworker)
* Gmail:  [@rossatti.dataworker](rossatti.dataworker@gmail.com)
