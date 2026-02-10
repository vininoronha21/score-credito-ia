# 🏦 Sistema de Credit Scoring com Machine Learning

Pipeline completo de Machine Learning aplicado a Credit Scoring, cobrindo desde a geração de dados até visualização executiva em Power BI, com foco em decisão orientada a risco.

## 📊 Visão Geral

Este projeto implementa um sistema de predição de inadimplência utilizando Machine Learning, simulando um cenário realista de crédito com dados sintéticos e lógica de negócio consistente.

O pipeline contempla:

* Geração de 100.000 registros sintéticos com fatores de risco e proteção
* Análise exploratória de dados (EDA)
* Treinamento e comparação de modelos supervisionados
* Análise de threshold e trade-offs de negócio
* Exportação de artefatos para visualização em Power BI

## 🎯 Objetivos

**Modelo de triagem inicial** para credit scoring, otimizado para **maximizar recall** (detectar inadimplentes).

**Aplicação prática:** Clientes classificados como alto risco são encaminhados para análise manual detalhada, enquanto clientes de baixo risco seguem no fluxo automatizado.

## 📈 Resultados do Modelo

### Logistic Regression (Modelo Selecionado)

> Métricas considerando threshold otimizado para recall

| Métrica            | Valor  | Interpretação                    |
| ------------------- | ------ | ---------------------------------- |
| **Recall**    | 66.95% | Detecta ~2 em cada 3 inadimplentes |
| **AUC-ROC**   | 0.667  | Poder discriminativo moderado      |
| **Precision** | 27.65% | Modelo de triagem (alto recall)    |
| **Accuracy**  | 60.01% | Acurácia geral                    |

### Comparação de Modelos (Threshold Padrão = 0.5)

| Modelo                        | AUC-ROC | Recall | Precision |
| ----------------------------- | ------- | ------ | --------- |
| **Logistic Regression** | 0.775   | 15.7%  | 57.3%     |
| **Random Forest**       | 0.856   | 77.8%  | 43.5%     |

**Decisão:** Apesar do Random Forest apresentar métricas superiores em alguns cenários, a Logistic Regression foi priorizada por:

* Interpretabilidade
* Estabilidade
* Facilidade de auditoria e compliance
* Menor custo computacional
* Melhor adequação ao contexto regulado de crédito

## 🔬 Principais Insights

### Top 3 Features Mais Importantes

1. **Histórico de Atrasos**  — 50.6%
2. **Ocupação** — 23.1%
3. **Número de Dependentes** — 15.7%

### Perfis de Maior Risco

* **Desempregados:** ~64% inadimplência
* **Score Serasa < 500:** ~37% inadimplência
* **Renda < R$ 30k/ano:** ~29% inadimplência

### Feature Engineering

* Renda Per Capita: Criada para capturar melhor a capacidade financeira real do cliente

`renda_per_capita = renda_anual / (numero_dependentes + 1)` que captura melhor o contexto familiar do que renda bruta.

## 📁 Estrutura do Projeto

```
score-credito-ia/
│
├── assets/                              # Recursos visuais
│
├── data/                                # Dados brutos
│   ├── database.db                      # Banco SQL
│   └── clientes_backup.csv              # Backup em CSV
│
├── insights/                            # Resultados das análises
│   ├── feature_importance.csv           # Importância das features
│   ├── insights_eda.txt                 # Resumo da EDA
│   └── matriz_correlacao.csv            # Matriz de correlação
│
├── models/                              # Modelos treinados
│   ├── encoder_escolaridade.pkl
│   ├── encoder_estado.pkl
│   ├── encoder_estado_civil.pkl
│   ├── encoder_genero.pkl
│   ├── encoder_ocupacao.pkl
│   ├── metadados_modelo.json            # Métricas e configurações
│   ├── modelo_final.pkl                 # Modelo Logistic Regression
│   └── scaler.pkl                       # Normalizador StandardScaler
│
├── notebooks/                           # Jupyter Notebooks
│   ├── 01_criar_database.ipynb          # Geração de dados sintéticos
│   ├── 02_analise_exploratoria.ipynb    # EDA e correlações
│   ├── 03_modelagem_ml.ipynb            # Treinamento e validação
│   └── 04_exportar_powerbi.ipynb        # Exportação para Power BI
│
├── powerbi/                             # Datasets para visualização
│   ├── confusion_matrix.csv
│   ├── dataset_powerbi.csv              # Dataset principal (100k linhas)
│   ├── feature_importance_comparison.csv
│   ├── inadimplencia_ocupacao.csv
│   ├── inadimplencia_renda.csv
│   ├── inadimplencia_score.csv
│   ├── model_comparison.csv             # Comparação LR vs RF
│   ├── precision_recall_curve.csv
│   ├── roc_curve.csv
│   ├── threshold_analysis.csv           # Análise de thresholds
│   └── threshold_analysis_visual.csv
│
├── .gitignore                           # Arquivos ignorados pelo Git
├── LICENSE                              # Licença do projeto
├── README.md                            # Documentação principal
├── README-EN.md                         # Documentação em EN-US
├── ROADMAP.md                           # Roadmap
└── requirements.txt                     # Dependências
```

## 🛠️ Tecnologias Utilizadas

* **Python 3.12+**
* **Pandas**
* **NumPy**
* **Scikit-learn**
* **SQLite**
* **Power BI**

## 🚀 Como Executar

### 1. Instalar Dependências

```bash
pip install -r requirements.txt
```

### 2. Executar os Notebooks (em ordem)

```bash
# 1. Criar banco de dados com 100k registros
jupyter notebook notebooks/01_criar_database.ipynb

# 2. Análise exploratória
jupyter notebook notebooks/02_analise_exploratoria.ipynb

# 3. Treinar modelos
jupyter notebook notebooks/03_modelagem_ml.ipynb

# 4. Exportar para Power BI
jupyter notebook notebooks/04_exportar_powerbi.ipynb
```

### 3. Dashboard Power BI

Abrir o Power BI Desktop e importar os CSVs da pasta `powerbi/`:

1. **Obter Dados** → **Texto/CSV**
2. Importar os 10 arquivos CSV
3. Criar visualizações conforme guia (ver seção abaixo)

## 📊 Guia do Dashboard Power BI

> O dashboard permite análise executiva e operacional:

### Página 1: Visão Executiva

* **Cards KPI:** Taxa inadimplência, Recall, AUC-ROC, Total clientes
* **Gráfico:** Threshold vs Precision/Recall (linha dupla)
* **Slicer:** Threshold interativo (0.3, 0.4, 0.5, 0.6, 0.7)

### Página 2: Performance do Modelo

* **Curva ROC** (comparando LR vs RF)
* **Matriz de Confusão** (filtrada por threshold)
* **Tabela Comparativa** (LR vs RF)

### Página 3: Feature Importance

* **Gráfico de Barras:** Top 10 features mais importantes
* **Cards:** Top 3 features com destaque

### Página 4: Análise de Negócio

* **Gráfico:** Inadimplência por Ocupação
* **Gráfico:** Inadimplência por Faixa de Score
* **Gráfico:** Inadimplência por Faixa de Renda

### Página 5: Drill-Down Individual

* **Tabela:** Clientes com probabilidade de inadimplência
* **Filtros:** Estado, Ocupação, Idade, Renda
* **Scatter Plot:** Renda vs Score (colorido por inadimplência)

## 🎓 Metodologia

### 1. Criação de Dados

* 100.000 clientes
* 16 variáveis explicativas
* Target gerado com lógica de negócio realista

### 2. Análise Exploratória

* Estatísticas descritivas
* Análise de correlações
* Identificação de fatores de risco e proteção

### 3. Modelagem

* Train/Test: 70/30
* Encoding e normalização
* Validação cruzada (5-fold)
* Análise de threshold (17 thresholds testados)
* Comparação entre modelos: Logistic Regression vs Random Forest

### 4. Exportação

* Dados prontos para Power BI
* Sem necessidade de processamento adicional

## 📋 Trade-offs e Decisões de Négocio

### Por que priorizar Recall?

✅ Custo de falso negativo ≈ **R$ 5.000**

✅ Custo de falso positivo ≈ **R$ 1.000**

✅ Falsos positivos podem ser mitigados via análise manual

### Por que Logistic Regression?

✅ Interpretável - Coeficientes = Regras de negócio

✅ Rápido - Latência < 10ms

✅ Auditável - Fácil de explicar para compliance

✅ Atende requisitos - Recall >65% se ajustarmos threshold

### Trade-off de Threshold

| Threshold | Perfil      | Precision | Recall |
| --------- | ----------- | --------- | ------ |
| 0.30      | Agressivo   | 44.7%     | 48.8%  |
| 0.50      | Balanceado  | 57.3%     | 15.7%  |
| 0.70      | Conservador | 62.7%     | 2.6%   |

## 🔄 Roadmap

* [ ] API (FastAPI)
* [ ] Monitoramento de data drift
* [ ] Sistema de retreinamento automático
* [ ] A/B testing de thresholds
* [ ] Integração com CRM

## ⚠️ Observações Finais

Este projeto foi desenvolvido com foco em estudo e aplicação prática de Machine Learning em Credit Scoring, utilizando dados sintéticos e hipóteses de negócio realistas.

Embora siga boas práticas de modelagem, avaliação e tomada de decisão, trata-se de um projeto educacional/portfólio, não representando um sistema de crédito em produção real. Algumas simplificações foram adotadas intencionalmente para fins didáticos e de aprendizado.

O objetivo principal é demonstrar:

- Estruturação de pipelines de ML
- Análise orientada a risco
- Tomada de decisão baseada em trade-offs
- Comunicação técnica e de negócio

## 📝 Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

## 👨‍💻 Autor

Desenvolvido por **Vinícius Forte**

- 🐙 GitHub: [vininoronha21](https://github.com/vininoronha21)
- 💼 LinkedIn: [Vinícius Noronha](https://linkedin.com/in/viniciusnoronha)
- 📧 Email: contatovininoronha@gmail.com

---

**Nota:** Para a versão em Inglês, consulte [README-EN.md](README-EN.md)
