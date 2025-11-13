# Desafio de Projeto: Processando e Transformando Dados com Power BI Analyst

## Visão Geral do Projeto

Este projeto demonstra o fluxo completo de um Analista de Power BI, desde o setup da infraestrutura de dados até a transformação e preparação para a modelagem analítica. O objetivo é processar e limpar uma base de dados corporativa (`Company`) hospedada no **Azure MySQL**, preparando-a para um futuro modelo estrela e relatórios gerenciais.

### Objetivos de Aprendizado
* Configurar o setup de banco de dados na **Azure**.
* Popular o servidor com script fornecido.
* Integrar o **MySQL** com **Power BI**.
* Realizar as transformações indicadas no Power Query.

---

## Arquitetura de Dados e Setup

O projeto segue o fluxo de dados no Power BI, onde os dados são extraídos de fontes heterogêneas (neste caso, MySQL Azure) e processados no Power Query.

### 1. Configuração da Fonte de Dados
* **Plataforma Cloud:** Microsoft Azure.
* **Servidor de BD:** Azure Database for MySQL.
* **Base de Dados:** `Company` (Criada e populada via scripts SQL).

### 2. Conexão e Ingestão no Power BI
O Power BI foi integrado ao MySQL no Azure. Todas as tabelas foram carregadas no **Power Query Editor** para o processo de transformação.

---

## Etapas de Transformação Realizadas (Power Query)

As transformações seguiram as diretrizes do desafio para limpar, enriquecer e validar a integridade dos dados.

### 3. Limpeza e Validação Inicial
* **Ajuste de Tipos de Dados:** Tipos de dados e cabeçalhos foram verificados. Valores monetários (`Salary`, etc.) foram convertidos para o tipo **Double Preciso**.
* **Tratamento de Nulos:**
    * A existência de nulos foi verificada.
    * Nulos em `Super_ssn` foram analisados, podendo indicar os gerentes
    * Foi verificado se há algum departamento sem gerente. Se houvesse, as lacunas foram preenchidas com dados supostos.
* **Verificação de Dados:** O número de horas (`Hours`) dos projetos foi verificado.

### 4. Enriquecimento e Mesclagem de Dados

| Passo | Ação no Power Query | Justificativa |
| :--- | :--- | :--- |
| **Mesclagem Depto.** | Mesclagem das consultas `employee` e `departament`. A junção teve como base a tabela `employee`. | Criar uma tabela `employee` completa com o **nome do departamento** (`Dname`) de cada colaborador. Colunas desnecessárias foram eliminadas. |
| **Mesclagem Gerente (Self-Join)** | Realizada a junção dos colaboradores e respectivos nomes dos gerentes. | Associar o nome do gerente ao colaborador. |
| **Mesclar Nome Completo** | Mesclagem das colunas de Nome e Sobrenome (`Fname` e `Lname`) em uma única coluna. | Simplificação e padronização dos dados para visualização. |
| **Mesclagem Depto + Local** | Mesclagem dos nomes de departamentos e localização. | Cria um identificador único para cada combinação de Departamento-Local, auxiliando na criação do **Modelo Estrela**. |

### 5. Resposta: Mesclar vs. Atribuir (Mesclar Depto/Local)

> **Por que utilizamos apenas o Mesclar e não o Atribuir (Append) para a combinação Departamento/Localização?** 
>
> A operação **Mesclar (Merge)** é utilizada para combinar **colunas** (dados lateralmente), gerando uma nova coluna composta por valores de duas colunas existentes. A operação **Atribuir (Append/Anexar)** combina **linhas** (dados verticalmente), empilhando duas tabelas com estruturas de coluna semelhantes. Como o objetivo era combinar **Dname** e **Dlocation** em uma única coluna para criar um identificador único, a operação **Mesclar** é a correta.

### 6. Agrupamento e Finalização

* **Agrupamento:** Os dados foram agrupados para contar o número de colaboradores por gerente.
* **Eliminação de Colunas:** Colunas desnecessárias (que não seriam usadas no relatório) foram eliminadas de cada tabela.

