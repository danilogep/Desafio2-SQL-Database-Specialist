# 🔧 Sistema de Gerenciamento de Oficina Mecânica

Este projeto consiste na modelagem completa (Conceitual, Lógica e Física) de um banco de dados relacional para um sistema de controle e gerenciamento de ordens de serviço (OS) em uma oficina mecânica.

O esquema foi desenvolvido refinando requisitos de negócio para garantir integridade de dados, performance e escalabilidade, simulando um cenário real de mercado.

## 📋 Descrição do Desafio e Narrativa

O objetivo foi criar um esquema conceitual do zero a partir da seguinte narrativa de negócio:

* **Clientes e Veículos:** Clientes levam veículos à oficina para conserto ou revisão. Um cliente pode ter vários veículos.
* **Equipes e Mecânicos:** Os mecânicos são organizados em equipes (ex: Lataria, Motor). Uma equipe inteira é responsável por avaliar e executar os serviços em um veículo.
* **Ordem de Serviço (OS):** O documento central que consolida os trabalhos. A OS possui número, data de emissão, status, valor total e data de conclusão.
* **Composição de Custos:** O valor total da OS é calculado somando-se os **Serviços** (mão-de-obra baseada em tabela de referência) e as **Peças** utilizadas.
* **Tabelas de Referência:** Os serviços e peças possuem tabelas próprias com valores padrão.

## 🛠️ Tecnologias Utilizadas

* **Banco de Dados:** MySQL
* **Ferramenta de Modelagem:** MySQL Workbench
* **Linguagem:** SQL (DDL/DML)

## 📐 Estrutura do Modelo (Destaques Técnicos)

### 1. Histórico de Preços (Integridade Financeira)
Uma falha comum em sistemas iniciantes é vincular a OS diretamente ao preço atual da peça/serviço. Se o preço da peça mudar na tabela `Peca` daqui a 6 meses, todas as OS antigas teriam seus valores alterados incorretamente.
* **Solução:** Implementação de atributos `Valor_Unitario` e `Valor_Cobrado` nas tabelas associativas (`Detalhe_OS_Peca` e `Detalhe_OS_Servico`). Isso "congela" o preço praticado no momento da venda, garantindo que o histórico financeiro permaneça imutável.

### 2. Tipagem de Dados Adequada
* **Valores Monetários:** Uso estrito de `DECIMAL` em vez de `FLOAT` para garantir precisão nos cálculos financeiros e evitar erros de arredondamento de ponto flutuante.
* **Datas:** Uso de `YEAR` para o ano do veículo e `DATE` para emissões/conclusões.
* **Status:** Uso de `ENUM` para restringir o status da OS, garantindo consistência na máquina de estados (ex: 'Em Analise', 'Aprovado', 'Concluido').

## 📊 Diagrama Entidade-Relacionamento (EER)

![Diagrama EER da Oficina](/img/diagrama_oficina.png)

## 🗂️ Dicionário de Dados Simplificado

* `Cliente`: Dados cadastrais dos proprietários.
* `Veiculo`: Dados do automóvel, vinculado a um cliente.
* `Equipe`: Grupo de trabalho responsável pela execução.
* `Mecanico`: Especialistas vinculados a uma equipe específica.
* `OS`: Tabela central (Fato) que une Veículo e Equipe, contendo datas e status.
* `Servico` / `Peca`: Tabelas de dimensão (catálogo) com descrições e preços de referência.
* `Detalhe_OS_Servico`: Tabela associativa (N:N) que registra quais serviços foram feitos, quantidades e valores históricos.
* `Detalhe_OS_Peca`: Tabela associativa (N:N) que registra quais peças foram usadas, quantidades e valores históricos.

## 🚀 Como Executar

1.  Clone este repositório.
2.  Abra o arquivo `Oficina.mwb` no MySQL Workbench.
3.  Execute o script para criar o esquema e as tabelas.