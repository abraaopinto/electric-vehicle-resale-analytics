# Electric Vehicle Resale Analytics

Análise exploratória, tendências de mercado e modelos preditivos
para entender como diferentes fatores influenciam o **valor de revenda de veículos elétricos**
(`Resale_Value_USD`).

Repositório do projeto de nível 2, estruturado para avaliação de habilidades
em análise de dados, modelagem estatística e comunicação de resultados de negócio.

---

## 1. Objetivo do projeto

Responder às seguintes perguntas de negócio:

- Quais características técnicas e de uso (bateria, autonomia, uso urbano/rodoviário, região, etc.)
  estão mais associadas a um **maior valor de revenda**?
- Como o **mercado de veículos elétricos** se comportou ao longo do tempo em termos de:
  - preço médio de revenda,
  - custos de manutenção,
  - economia de CO₂,
  - custos de seguro e recarga?
- É possível construir um **modelo preditivo** que estime com boa precisão
  o valor de revenda de um veículo elétrico, a partir de suas características?

---

## 2. Dataset utilizado

O projeto utiliza o arquivo:

- `electric_vehicle_analytics(in).csv`

Cada linha representa um **veículo elétrico** em um determinado contexto de uso,
com colunas que incluem, por exemplo:

- `Resale_Value_USD` — variável alvo (valor de revenda em dólares);
- `Year` — ano do veículo;
- `Make`, `Model` — fabricante e modelo;
- `Region` — região de uso;
- `Vehicle_Type` — tipo de veículo;
- `Usage_Type` — tipo de uso (por exemplo, uso urbano, rodoviário, misto);
- `Battery_Capacity_kWh`, `Range_miles`, `Battery_Health_%` — características técnicas da bateria;
- `Monthly_Charging_Cost_USD`, `Maintenance_Cost_USD`, `Insurance_Cost_USD` — custos associados;
- `CO2_Saved_tons` — economia de CO₂;
- `Mileage_miles` — quilometragem.

> **Importante:** este é um dataset **sintético/simulado** para fins de estudo;
> não representa dados reais de uma montadora ou locadora específica.

---

## 3. Estrutura do repositório

Estrutura recomendada:

```text
electric-vehicle-resale-analytics/
├─ data/
│  ├─ inputs/
│  │  └─ electric_vehicle_analytics(in).csv
│  ├─ outputs/
│  │  ├─ figures/      # gráficos gerados
│  │  ├─ reports/      # CSVs de indicadores, correlações, etc.
│  │  └─ models/       # modelo treinado (.joblib)
│  └─ (outros arquivos brutos, se necessário)
├─ notebooks/
│  └─ ev_resale_analysis.ipynb
├─ README.md
├─ requirements.txt
└─ .gitignore
```

Se não quiser a subpasta `inputs/`, basta manter o arquivo em `data/electric_vehicle_analytics(in).csv`
e ajustar o caminho no
notebook (`DATA_INPUT_PATH`).

---

## 4. Como executar o projeto

### 4.1. Clonar o repositório

Na pasta onde você mantém seus projetos:

```bash
git clone https://github.com/<seu-usuario>/electric-vehicle-resale-analytics.git
cd electric-vehicle-resale-analytics
```

### 4.2. Criar ambiente virtual (opcional, mas recomendado)

```bash
python -m venv venv
source .\venv/bin/activate    # Linux/Mac
# ou
.\venv\Scripts\activate     # Windows (PowerShell ou CMD)
```

### 4.3. Instalar dependências

Na raiz do repositório:

```bash
pip install -q -r requirements.txt
```

O arquivo `requirements.txt` contém as bibliotecas necessárias para rodar o notebook
(pandas, numpy, matplotlib, seaborn, scikit-learn, statsmodels, jupyter, joblib, etc.).

Se estiver utilizando o **Google Colab**, você também pode instalar as
dependências a partir do próprio notebook, descomentando a linha no início
de `ev_resale_analysis.ipynb`:

```python
# !pip install -q -r "../requirements.txt"
```

Em ambiente local, recomenda-se instalar as dependências pelo terminal
(`pip install -r requirements.txt`) antes de abrir o Jupyter.

### 4.4. Garantir que o dataset está no local correto

Coloque o arquivo `electric_vehicle_analytics(in).csv` em:

```text
data/inputs/electric_vehicle_analytics(in).csv
```

ou, se preferir sem a subpasta `inputs/`:

```text
data/electric_vehicle_analytics(in).csv
```

Nesse caso, não esqueça de ajustar o caminho no notebook.

### 4.5. Abrir o Jupyter Lab/Notebook

Na raiz do projeto:

```bash
jupyter lab
# ou
jupyter notebook
```

Em seguida, navegue até `notebooks/ev_resale_analysis.ipynb` e execute as células na ordem
(`Run All` ou passo a passo).

---

## 5. Organização lógica do notebook

O notebook `ev_resale_analysis.ipynb` está organizado em **Passos**, seguindo
o fluxo natural de um projeto de ciência de dados aplicado ao negócio:

### Passo 1 — Preparação do Ambiente

- Instalação de dependências (quando necessário);
- Imports das bibliotecas;
- Definição de constantes (caminhos, semente aleatória, parâmetros gerais);
- Funções utilitárias com docstrings.

### Passo 2 — Parte #1: EDA e Comparação de Grupos

- Carregamento do dataset e resumo inicial (`summarize_dataframe`);
- Análise de valores ausentes;
- Distribuição de variáveis numéricas (histogramas + boxplots);
- Distribuição de variáveis categóricas (gráficos de barras);
- Comparação de grupos em relação a `Resale_Value_USD`:
  - Fabricante (`Make`)
  - Região (`Region`)
  - Tipo de veículo (`Vehicle_Type`)
  - Tipo de uso (`Usage_Type`)
- Testes ANOVA (`Resale_Value_USD ~ fator`) para cada um desses fatores;
- Geração de texto de **insights** focados em:
  - diferenças de valor de revenda por grupo;
  - possíveis explicações de negócio para essas diferenças.

### Passo 3 — Parte #2: Tendências de Mercado ao Longo do Tempo

- Agregação anual dos principais indicadores:
  - valor médio de revenda;
  - custos médios de manutenção, seguro, recarga;
  - economia de CO₂;
  - quilometragem e saúde da bateria.
- Gráficos de **série temporal** para cada indicador;
- Ajuste de regressões lineares simples
  (por exemplo, `avg_resale_value ~ Year`) para:
  - estimar tendência (inclinação e significância);
  - gerar um resumo tabular (coeficientes, p-valor, R²);
- Projeção simplificada do valor médio de revenda para alguns anos à frente,
  com base na regressão `avg_resale_value ~ Year`;
- Geração de texto de **insights** sobre:
  - como o mercado vem evoluindo;
  - implicações para fabricantes, locadoras e consumidores.

### Passo 4 — Parte #3: Modelagem Preditiva do Valor de Revenda

- Seleção da variável alvo: `Resale_Value_USD`;
- Separação entre `X` (features) e `y` (target), removendo IDs;
- Identificação de variáveis numéricas e categóricas;
- Construção de um **pipeline** de pré-processamento:
  - numéricas: imputação (mediana) + padronização (`StandardScaler`);
  - categóricas: imputação (moda) + one-hot encoding;
- Divisão treino/teste (por exemplo, 80%/20%);
- Treinamento de diferentes modelos de regressão:
  - `LinearRegression`;
  - `RandomForestRegressor`;
  - `GradientBoostingRegressor`;
- Avaliação com:
  - RMSE (root mean squared error);
  - MAE (mean absolute error);
  - R² (coeficiente de determinação);
  - Validação cruzada (CV) para avaliar estabilidade;
- Comparação dos modelos em uma tabela consolidada;
- Construção de um **ensemble simples** (média das previsões)
  e avaliação da sua performance.

### Passo 5 — Interpretação e Importância das Variáveis

- Seleção do melhor modelo baseado na métrica (menor RMSE em teste);
- Extração de **importância de variáveis** (no caso de modelos baseados em árvore);
- Reconstrução dos nomes das features após o one-hot encoding;
- Geração de um gráfico com as principais variáveis mais relevantes;
- Interpretação de quais características mais influenciam
  o valor de revenda (ex.: saúde da bateria, quilometragem, tipo de uso, etc.).

### Passo 6 — Conclusões e Recomendações de Negócio

- Síntese dos achados da EDA;
- Síntese das tendências temporais (valor de revenda, custos, CO₂);
- Comentário sobre a qualidade dos modelos (RMSE, MAE, R²);
- Interpretação de quais fatores são **alavancas de valor** para o mercado
  de veículos elétricos;
- Recomendações para:
  - empresas que operam frotas ou locadoras;
  - consumidores interessados em valor de revenda;
  - potenciais investidores ou analistas de mercado.

### 5.1. Artefatos gerados durante a execução

Ao executar o notebook `notebooks/ev_resale_analysis.ipynb`, são criados
automaticamente alguns artefatos dentro de `data/outputs/`:

- `data/outputs/reports/`  
  Relatórios em CSV e Markdown, incluindo:
  - resumo de valores ausentes;
  - correlações com `Resale_Value_USD`;
  - resultados dos testes de ANOVA;
  - indicadores anuais e regressões de tendência;
  - métricas dos modelos preditivos e do ensemble.

- `data/outputs/figures/`  
  Figuras em PNG geradas na análise exploratória, nas tendências de mercado
  e na importância das variáveis do modelo.

- `data/outputs/models/`  
  Arquivo `.joblib` com o melhor modelo treinado para previsão de
  `Resale_Value_USD`, pronto para ser carregado em outros scripts ou APIs.

Esses artefatos permitem que o projeto seja reutilizado em relatórios,
apresentações e experimentos sem necessidade de reexecutar todas as análises.

---

## 6. Ferramentas e bibliotecas principais

- **Linguagem**: Python 3.10+
- **Manipulação de dados**: `pandas`, `numpy`
- **Visualização**: `matplotlib`, `seaborn`
- **Modelagem estatística**: `statsmodels`
- **Modelagem preditiva**: `scikit-learn`
- **Serialização de modelos**: `joblib`
- **Ambiente interativo**: `Jupyter Lab` / `Jupyter Notebook`

---

## 7. Como este projeto atende aos critérios de avaliação

- ✅ Repositório estruturado, com `README`, `requirements` e notebook principal;
- ✅ Notebook único que cobre:
  - EDA robusta e comparação de grupos;
  - Análise temporal de indicadores e tendências;
  - Modelos preditivos com avaliação quantitativa e interpretação;
- ✅ Uso de visualizações claras, comentários e docstrings;
- ✅ Seções finais dedicadas a **insights** e **recomendações de negócio**, conectando
  os resultados técnicos às decisões estratégicas relacionadas ao valor de revenda
  de veículos elétricos.

---

Autor: **Abraão dos Santos Pinto**