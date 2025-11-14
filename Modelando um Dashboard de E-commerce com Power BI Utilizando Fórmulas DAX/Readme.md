# 📊 Desafio de Projeto: Modelagem Dimensional no Power BI (Financial Sample)

## 📌 Visão Geral do Projeto

Este projeto demonstra a capacidade de transformar uma base de dados transacional ou "flat" (a tabela `Financial Sample`) em um **Modelo Dimensional (Star Schema)** utilizando o **Power Query** e **DAX** no Power BI.

O objetivo principal é otimizar a performance analítica e facilitar a criação de relatórios, separando as informações contextuais (Dimensões) das informações mensuráveis (Fatos).

### 🎯 Objetivos de Aprendizado
* Aplicar o conceito de *Star Schema* (Esquema em Estrela).
* Utilizar o **Power Query** para limpeza, agrupamento e criação de Chaves de Negócio.
* Criar Chaves Substitutas (SKs) e associá-las para construir o modelo.
* Utilizar funções **DAX** para a criação de tabelas dinâmicas de tempo.

---

## 🛠️ Tecnologias e Base de Dados

| Categoria | Detalhe |
| :--- | :--- |
| **Ferramenta Principal** | Microsoft Power BI Desktop (Power Query e DAX) |
| **Base de Dados** | Tabela única `Financial Sample` (Fonte de dados do desafio)  |
| **Modelo Final** | Star Schema (Esquema em Estrela) |

---

## ⚙️ Processo de Construção do Diagrama

A modelagem seguiu as diretrizes do desafio, partindo de uma única tabela (`Financial Sample`) e utilizando o Power Query para criar múltiplas consultas, limpando e transformando cada uma delas em uma Dimensão ou Tabela Fato.

### 1. Preparação da Base
* **`Financials_origem`**: A consulta original foi **duplicada** e mantida em modo **oculto** (Não carregar no relatório) para servir como *backup*.
* **Tabela Base**: Uma segunda cópia da tabela original foi utilizada como base para as transformações subsequentes.

### 2. Criação das Dimensões (Power Query)

As tabelas de dimensão foram criadas a partir de cópias da Tabela Base. O foco foi garantir a **unicidade** dos registros e criar as **Chaves de Negócio** (`ID_Produto`).

#### A. D\_Produtos (Dimensão Agregada)
Esta dimensão foi criada através da funcionalidade **Agrupar Por** no Power Query.
* **Agrupamento:** A consulta foi agrupada pela coluna `Product`.
* **Cálculos Agregados:** Foram aplicadas diversas agregações para fins analíticos, como:
    * `Valor máximo de venda` (Máx. da `Sale Price`) 
    * `Valor mínimo de venda` (Mín. da `Sale Price`) 
    * `Média do valor de vendas` (Média da `Sale Price`) 
    * Mediana do valor de vendas` (Mediana da `Sale Price`) 
* **ID de Produto (Chave de Negócio):** Foi criada uma coluna **Condicional** ou **Índice** para gerar o `ID_Produto` , que servirá como chave de negócio na tabela Fato.

#### B. D\_Descontos e D\_Produtos\_Detalhes
Estas dimensões foram criadas através da duplicação da Tabela Base, seleção de colunas relevantes (`Discount Band`, `Sale Price`, etc.) e **remoção de duplicatas** nas colunas de contexto para garantir registros únicos.

#### C. D\_Detalhes
Esta dimensão contempla informações de contexto que não foram absorvidas nas demais dimensões (Ex: `Segment`, `Country`), com foco na unicidade de cada atributo.

### 3. Tabela Fato (`F_Vendas`)
A Tabela Fato representa a granularidade das vendas (cada linha é uma transação ou registro de venda).

* **Conteúdo:** Contém as chaves para as dimensões (`ID_Produto`) e os fatos (medidas) da transação, como `Units Sold`, `Sales`, `Profit`, e `Date`.
* **Limpeza:** Colunas de texto duplicadas (que foram movidas para as dimensões, como `Product`) foram removidas, e as chaves de negócio (`ID_Produto`) foram introduzidas através da função **Mesclar Consultas** (Merge Queries) no Power Query.

### 4. Tabela de Data (`D_Calendário`)
A dimensão de tempo foi criada utilizando a linguagem **DAX** no Power BI Desktop.

* **Função DAX Utilizada:** `CALENDAR(MIN('F_Vendas'[Date]), MAX('F_Vendas'[Date]))` (ou similar, como `CALENDARAUTO()`).
* **Enriquecimento:** Colunas adicionais (`Ano`, `Mês`, `Trimestre`, etc.) foram criadas usando `ADDCOLUMNS` e funções DAX de data (`YEAR`, `MONTH`, etc.) para permitir a análise temporal em diferentes níveis de granularidade.

---

## 🖼️ Esquema em Estrela (Star Schema)

Após todas as transformações, o modelo de dados foi finalizado, onde a tabela `F_Vendas` está no centro, ligada diretamente às suas dimensões.

**(Inclua aqui a imagem do seu modelo de dados no Power BI)**

---

## 🚀 Entregáveis Finais

* Arquivo do projeto Power BI (`.pbix`).
* Imagem do modelo de dados (Esquema em Estrela).
* Este arquivo `README.md` detalhando o processo.
