# Estrutura do Projeto

**Fase 1: Fundação e Dados (SQLite)**

Onde os dados ganham estrutura de banco de dados real.

- Preparação: Criar o banco database.db usando a biblioteca sqlite3.
- Ingestão: Criar uma tabela de clientes e importar o CSV inicial.
- Consulta: Praticar alguns comandos SQL para garantir que os dados estão acessíveis (ex: selecionar clientes com renda > 5000).

**Fase 2: Inteligência e ML (Jupyter Notebook)**

Como as empresas decidem o limite?

- Análise (EDA): Identificar correlações (ex: "Idade influencia na inadimplência?").
- Treinamento: Testar modelos simples (Regressão Logística ou Random Forest).
- Exportação: Salvar o modelo final treinado usando pickle ou joblib (isso será usado pela API depois).

**Fase 3: Visualização (Power BI)**

A parte visual do projeto

- Conexão: Conectar o Power BI ao arquivo .db do SQLite.
- Modelagem: Criar medidas simples (DAX), como Taxa de Aprovação e Média de Score.
- Storytelling: Montar o dashboard focando em quem é o "bom pagador" e quem é o "risco".

**Fase 4: Disponibilização (FastAPI)**

Transformando o modelo em um produto real.

- Endpoint: Criar uma rota /predict que recebe dados do cliente (JSON).
- Lógica: Fazer a API carregar o modelo salvo na Fase 2 e devolver o resultado (Ex: "Score: 850 - Crédito Aprovado").
- Documentação: Usar o Swagger automático da FastAPI (/docs) para testar as previsões.

# To-Do

1. Cria banco SQLite ( )
2. Integra no Jupyter para análise ( )
3. Faz modelos de treino/teste para prever precisão ( )
4. Sobe um Dashboard em PowerBI ( )
5. Adiciona API (FastAPI) ( )
6. Deploy apenas do dashboard ( )
