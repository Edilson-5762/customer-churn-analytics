# 📊 Customer Churn Analytics: Preditividade & Inteligência de Retenção

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Estatística](https://img.shields.io/badge/Method-IV%20%26%20WoE-green?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-orange?style=for-the-badge)

Análise diagnóstica e preditiva desenvolvida para identificar os fatores críticos associados à evasão de clientes (*churn*) em uma instituição bancária. O projeto combina **tratamento rigoroso de outliers**, **engenharia de recursos (feature engineering)** e **mensuração de poder preditivo** utilizando as métricas **Information Value (IV)** e **Weight of Evidence (WoE)** na sua formulação estatística canônica.

---

## 🧭 Sumário
- [Visão Geral & Contexto de Negócio](#-visão-geral--contexto-de-negócio)
- [Arquitetura e Pipeline de Dados](#-arquitetura-e-pipeline-de-dados)
- [Metodologia Estatística (IV & WoE)](#-metodologia-estatística-iv--woe)
- [Principais Insights & Achados Preditivos](#-principais-insights--achados-preditivos)
- [Tratamento de Anomalias & Outliers](#-tratamento-de-anomalias--outliers)
- [Estrutura do Repositório](#-estrutura-do-repositório)
- [Tech Stack & Ferramentas](#-tech-stack--ferramentas)
- [Como Executar o Projeto Localmente](#-como-executar-o-projeto-localmente)
- [Responsividade & Visualização do Projeto](#-responsividade--visualização-do-projeto)
- [Conclusão & Próximos Passos](#-conclusão--próximos-passos)

---

## 📌 Visão Geral & Contexto de Negócio

A retenção de clientes é uma das alavancas mais críticas para a sustentabilidade financeira de serviços bancários, dado que o Custo de Aquisição de Clientes (CAC) supera frequentemente o custo de manutenção da base ativa.

Este projeto investiga os microdados de uma carteira de clientes com o objetivo de responder a três perguntas centrais de negócio:
1. **Quais variáveis demográficas e operacionais de fato segregam clientes propensos ao churn?**
2. **Qual é o peso real de cada atributo no risco final de cancelamento?**
3. **Como orientar campanhas de retenção direcionadas para maximizar o ROI do time de CRM?**

---

## 🏗️ Arquitetura e Pipeline de Dados

O projeto segue a arquitetura de pipeline modular de ciência de dados:

```text
[ Base Bruta (data/raw) ] 
          │
          ▼
[ Pipeline de Limpeza & Outliers ] ──► (Tratamento ITR / Z-Score)
          │
          ▼
[ Eng. de Recursos & Discretização ] ──► (Binning por Faixa Etária, Score, etc.)
          │
          ▼
[ Cálculo de IV & WoE (Canônico) ] ──► (Logaritmo Natural e Separação de Classes)
          │
          ▼
[ Matriz de Insights & Relatório ] ──► (Geração de Artefatos em reports/figures)
📐 Metodologia Estatística (IV & WoE)Para avaliar o poder de separação de cada variável individual em relação à variável resposta (Churn: 0 = Ativo, 1 = Cancelado), foi aplicada a abordagem clássica de Weight of Evidence (WoE) e Information Value (IV).Formulações Aplicadas:Weight of Evidence (WoE):$$WoE_i = \ln \left( \frac{\% \text{Ativos}_i}{\% \text{Cancelados}_i} \right)$$Information Value (IV - Canônico):$$IV = \sum_{i=1}^{k} \left( \% \text{Ativos}_i - \% \text{Cancelados}_i \right) \times WoE_i$$Régua de Interpretação Preditiva do IV:$< 0{,}02$: Inútil / Sem poder preditivo$0{,}02 \text{ a } 0{,}10$: Poder preditivo fraco$0{,}10 \text{ a } 0{,}30$: Poder preditivo médio$0{,}30 \text{ a } 0{,}50$: Poder preditivo forte$> 0{,}50$: Poder preditivo muito alto (atenção para possível data leakage)📊 Principais Insights & Achados PreditivosAbaixo estão os resultados consolidados do poder de discriminação das variáveis analisadas:VariávelIV Canônico (ln)Classificação PreditivaDiagnóstico de NegócioFaixa Idade0.67Muito AltaO grupo de 46 a 60 anos concentra 41,3% dos cancelamentos (contra 10,1% dos ativos). Ponto focal primário para retenção.Geografia0.17MédiaClientes da Alemanha registram taxa desproporcional de churn (39,9% dos cancelados vs. 21,3% dos ativos).Gênero0.07FracaPúblico feminino representa 55,9% do total de cancelamentos, indicando ruído ou fricção na jornada desse segmento.Faixa Score0.00Sem PreditividadeO Score de Crédito isolado não diferencia clientes ativos de cancelados nesta carteira.⚠️ Nota Metodológica: Embora o IV aponte forte associação estatística em variáveis como Idade e Geografia, a análise reforça a importância de diferenciar correlação/associação de causalidade direta, utilizando esses atributos como direcionadores de risco e segmentação estratégica.🧹 Tratamento de Anomalias & OutliersPara garantir a confiabilidade estatística e evitar distorções nas métricas agregadas:Mantivemos a rastreabilidade total salvando o dataset bruto original e a versão com tratamento de limites operacionais na pasta data/raw/.Foram analisadas variáveis numéricas contínuas para remoção/atenuamento de valores discrepantes que pudessem enviesar os cálculos de proporção e bins de agrupamento.📂 Estrutura do RepositórioPlaintextcustomer-churn-analytics/
├── data/
│   ├── raw/                  # Datasets brutos (com e sem tratamento de outliers)
│   └── processed/            # Datasets tratados e enriquecidos
├── notebooks/
│   └── churn_analysis.ipynb  # Notebook com EDA, eng. de atributos e cálculo de IV/WoE
├── reports/
│   └── figures/              # Gráficos exportados para documentação e apresentações
├── .gitignore                # Regras de exclusão do Git (ignora caches e temporários)
├── README.md                 # Documentação executiva e técnica do projeto
└── requirements.txt          # Dependências e bibliotecas Python do projeto
🛠️ Tech Stack & FerramentasLinguagem: Python 3.10+Manipulação & Análise de Dados: pandas, numpyVisualização de Dados: matplotlib, seabornI/O & Leitura de Planilhas: openpyxlAmbiente de Desenvolvimento: Visual Studio Code / Jupyter NotebooksControle de Versão: Git & GitHub🚀 Como Executar o Projeto LocalmentePré-requisitosPython 3.10 ou superior instalado.Git configurado na máquina.Passo a PassoClonar o repositório:Bashgit clone [https://github.com/Edilson-5762/customer-churn-analytics.git](https://github.com/Edilson-5762/customer-churn-analytics.git)
cd customer-churn-analytics
Criar e ativar um ambiente virtual (recomendado):Bash# Linux/macOS
python3 -m venv .venv
source .venv/bin/activate

# Windows (PowerShell)
python -m venv .venv
.venv\Scripts\Activate.ps1
Instalar as dependências:Bashpip install -r requirements.txt
Executar a análise:Abra o projeto no VS Code ou inicie o Jupyter Server:Bashjupyter notebook
Navegue até a pasta notebooks/ e execute o notebook churn_analysis.ipynb.📱 Responsividade & Visualização do ProjetoA documentação em Markdown deste repositório foi estruturada seguindo as diretrizes de design responsivo do GitHub:Tabelas Adaptáveis: Renderizam perfeitamente em telas de smartphones, tablets e monitores Ultrawide.Blocos de Código & Fórmulas: Formatados com sintaxe destacada (Syntax Highlighting) para rápida leitura técnica.Badges Dinâmicos: Facilitam a identificação imediata da stack utilizada e status do projeto.💡 Conclusão & Próximos PassosAções Recomendadas de Negócio:Criar um plano de onboarding e relacionamento específico para clientes na faixa de 46 a 60 anos.Realizar uma auditoria operacional na operação da Alemanha para entender fricções no produto local.Evolução Técnica:Implementação de modelos preditivos supervisionados (XGBoost, Random Forest e Regressão Logística).Uso das pontuações WoE como features diretas para a Regressão Logística (Scorecard de Churn).✉️ Desenvolvido por Edilson Moraes — Sinta-se à vontade para conectar e enviar feedbacks no LinkedIn.[https://www.linkedin.com/in/edilson-moraes-047128408/]