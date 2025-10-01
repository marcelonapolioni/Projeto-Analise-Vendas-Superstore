# Análise de Vendas - Dashboard Executivo em Power BI

## 🎯 Objetivo do Projeto
Este projeto foi desenvolvido como um portfólio para demonstrar habilidades em Business Intelligence utilizando o Microsoft Power BI. O objetivo principal é transformar um conjunto de dados brutos de vendas em um dashboard interativo e coeso, que forneça insights acionáveis sobre a performance de vendas, eficiência operacional e comportamento do cliente.

O processo abrange desde a extração e transformação dos dados (ETL), passando pela modelagem em esquema estrela, até o desenvolvimento de cálculos em DAX e a criação de um design visual.

## 📊 Dashboard Interativo
Uma versão interativa do relatório final foi publicada no Power BI Service e pode ser acessada através do link abaixo:

**[Clique aqui para acessar o relatório online](https://app.powerbi.com/groups/93a3faff-0bbb-4aff-a698-387b58466ad4/reports/cfc87b9f-8ea8-4292-a420-c1e7b948313d/dafbbe7f002dc7bc8001?experience=power-bi)**

## 🖼️ Preview do Dashboard
![Preview do Dashboard de Vendas](![alt text](image.png))

## 🛠️ Ferramentas e Tecnologias
* **Microsoft Power BI Desktop:** Ferramenta principal para todo o desenvolvimento.
* **Linguagem M (Power Query):** Utilizada para o processo de Extração, Transformação e Carga (ETL).
* **DAX (Data Analysis Expressions):** Utilizada para a criação de medidas, colunas calculadas e otimização do modelo de dados.

## 🏛️ Arquitetura do Projeto
O projeto foi estruturado utilizando o **Esquema Estrela (Star Schema)**.

* **Tabela Fato:**
  * `fPedidos`: Contém os dados quantitativos e as chaves de relacionamento (IDs) de cada transação de venda.

* **Tabelas Dimensão:**
  * `dCalendario`: Tabela de datas criada em DAX para permitir análises de inteligência de tempo.
  * `dProduto`: Contém os atributos únicos dos produtos (ID, Categoria, etc.).
  * `dCliente`: Contém os atributos únicos dos clientes.
  * `dLocalizacao`: Contém os atributos geográficos únicos (CEP, Cidade, Estado, Região).

## 🔄 Processo de ETL (Extração, Transformação e Carga)
A fase de ETL foi realizada inteiramente no **Power Query Editor**, utilizando a **Linguagem M**.

#### 1. Extração
* Os dados foram extraídos de um único arquivo Excel (`.xlsx`) contendo o dataset "Superstore".

#### 2. Transformação
* **Criação das Tabelas Dimensão:** A partir da consulta original, foram criadas duplicatas que deram origem às dimensões `dProduto`, `dCliente` e `dLocalizacao`. Em cada uma, foram selecionadas as colunas pertinentes e removidas as duplicatas para garantir uma chave primária única.
* **Limpeza e Otimização da Tabela Fato:** Da tabela `fPedidos` original, foram removidas as colunas de texto que já estavam presentes nas dimensões, mantendo apenas as chaves (IDs) e os valores numéricos.
* **Tratamento de Dados:** Foram aplicadas correções de tipos de dados e a filtragem de valores nulos nas chaves das tabelas dimensão para garantir a integridade dos relacionamentos.

## 🧠 Desenvolvimento DAX (Data Analysis Expressions)
A lógica de negócio e os principais indicadores foram desenvolvidos em DAX.

#### Tabela Calendário
Foi criada uma tabela `dCalendario` robusta e dinâmica para suportar todas as análises temporais.
```dax
dCalendario = 
    ADDCOLUMNS (
        CALENDAR (
            MIN ( 'fPedidos'[Order Date] ),
            MAX ( 'fPedidos'[Order Date] )
        ),
        "Ano", YEAR ( [Date] ),
        "MesNum", MONTH ( [Date] ),
        "MesAno", FORMAT ( [Date], "mmm/yyyy", "pt-BR" ),
        "AnoMes", YEAR ( [Date] ) * 100 + MONTH ( [Date] )
    )
```

#### Tabela de Medidas
Para organização e boas práticas, todos os cálculos foram centralizados em uma tabela `_Medidas`.

#### Colunas Calculadas
Foram criadas colunas para enriquecer a análise, como o tempo de despacho de cada pedido.
```dax
-- Na tabela fPedidos
Tempo para Envio = DATEDIFF('fPedidos'[Order Date], 'fPedidos'[Ship Date], DAY)
```

#### Medidas Principais (KPIs)
Exemplos de medidas criadas para o dashboard:
```dax
-- Total de receita
Total Vendas = SUM('fPedidos'[Sales])

-- Contagem de pedidos únicos
Número de Pedidos Únicos = DISTINCTCOUNT('fPedidos'[Order ID])

-- Valor médio por transação
Ticket Médio = DIVIDE([Total de Vendas], [Número de Pedidos Únicos])

-- Média de dias para despachar um pedido
Tempo Médio para Envio = AVERAGE('fPedidos'[Tempo para Envio])
```

## 🚀 Como Executar o Projeto Localmente
1.  É necessário ter o [Microsoft Power BI Desktop](https://powerbi.microsoft.com/pt-br/desktop/) instalado.
2.  Faça o download ou clone este repositório.
3.  Abra o arquivo `.pbix` no Power BI Desktop.

## 👤 Autor

**Marcelo Napoloni**

* **LinkedIn:** [Link para o seu perfil no LinkedIn]