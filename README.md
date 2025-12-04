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
  estão associadas a um **maior valor de revenda**?
- Como o mercado de veículos elétricos tem se comportado ao longo dos anos,
  em termos de valor de revenda, custos e economia de CO₂?
- É possível construir um **modelo preditivo** que estime o valor de revenda a partir
  das características do veículo e do seu histórico de uso?

As respostas são organizadas em três entregas, todas implementadas no notebook
[`notebooks/ev_resale_analysis.ipynb`](notebooks/ev_resale_analysis.ipynb):

1. **Parte #1 — Análise Exploratória de Dados (EDA) e Comparação de Grupos**  
2. **Parte #2 — Tendências de Mercado ao Longo do Tempo**  
3. **Parte #3 — Modelo Preditivo para o Valor de Revenda**

---
## 2. Dados utilizados

O projeto utiliza um dataset de veículos elétricos com as seguintes informações
(principais colunas):

- **Identificação e contexto**
  - `Vehicle_ID`, `Make`, `Model`, `Year`, `Region`, `Vehicle_Type`, `Usage_Type`
- **Bateria e desempenho**
  - `Battery_Capacity_kWh`, `Battery_Health_%`, `Range_km`,
    `Charging_Power_kW`, `Charging_Time_hr`, `Charge_Cycles`
- **Uso e performance**
  - `Energy_Consumption_kWh_per_100km`, `Mileage_km`,
    `Avg_Speed_kmh`, `Max_Speed_kmh`,
    `Acceleration_0_100_kmh_sec`, `Temperature_C`
- **Custos e impacto ambiental**
  - `Maintenance_Cost_USD`, `Insurance_Cost_USD`,
    `Electricity_Cost_USD_per_kWh`, `Monthly_Charging_Cost_USD`,
    `CO2_Saved_tons`
- **Variável-alvo**
  - `Resale_Value_USD` – valor estimado de revenda (target do modelo).

### Localização do arquivo

Por padrão, o notebook espera encontrar o arquivo em um destes caminhos:

- `data/inputs/electric_vehicle_analytics(in).csv` **(recomendado)**  
- ou, em fallback, `data/electric_vehicle_analytics(in).csv`

Se o seu arquivo tiver outro nome (por exemplo `electric_vehicle_analytics.csv`),
renomeie-o para `electric_vehicle_analytics(in).csv` e coloque na pasta correta, ou
ajuste a constante `DATA_INPUT_PATH` no notebook.

---
## 3. Estrutura do repositório

Estrutura recomendada:

```text
electric-vehicle-resale-analytics/
├─ data/
│  ├─ inputs/
│  │  └─ electric_vehicle_analytics(in).csv
│  └─ (outros arquivos brutos, se necessário)
├─ data_outputs/
│  ├─ figures/      # gráficos gerados
│  ├─ reports/      # CSVs de indicadores, correlações, etc.
│  └─ models/       # modelo treinado (.joblib)
├─ notebooks/
│  └─ ev_resale_analysis.ipynb
├─ README.md
├─ requirements.txt
└─ .gitignore
```

Se não quiser a subpasta `inputs/`, basta manter o arquivo em `data/electric_vehicle_analytics(in).csv`
e ajustar o caminho no notebook.

---
## 4. Como executar o projeto

### 4.1. Clonar o repositório

```bash
git clone https://github.com/abraaopinto/electric-vehicle-resale-analytics.git
cd electric-vehicle-resale-analytics
```

> Ajuste a URL se o repositório estiver em outra conta ou organização.

### 4.2. Criar e ativar ambiente virtual (opcional, mas recomendado)

```bash
python -m venv venv

# Windows
.\venv\Scripts\activate

# Linux / macOS
source venv/bin/activate
```

### 4.3. Instalar dependências

Na raiz do repositório:

```bash
pip install -q -r requirements.txt
```

O arquivo `requirements.txt` contém as bibliotecas necessárias para rodar o notebook
(pandas, numpy, matplotlib, seaborn, scikit-learn, statsmodels, jupyter, joblib, etc.).

### 4.4. Garantir que o dataset está no local correto

Coloque o arquivo `electric_vehicle_analytics(in).csv` em:

```text
data/inputs/electric_vehicle_analytics(in).csv
```

ou, alternativamente, em:

```text
data/electric_vehicle_analytics(in).csv
```

Se o nome/caminho forem diferentes, ajuste a constante `DATA_INPUT_PATH`
na célula de configuração do notebook.

### 4.5. Rodar o Jupyter Notebook

```bash
jupyter notebook
```

Depois, abra o arquivo:

```text
notebooks/ev_resale_analysis.ipynb
```

e execute as células **na ordem**, do topo até o final.

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
- Testes ANOVA (`Resale_Value_USD ~ C(grupo)`);
- Correlações de Pearson entre a variável-alvo e as demais variáveis numéricas;
- Seção textual com **insights da Parte 1**.

### Passo 3 — Parte #2: Tendências de Mercado ao Longo do Tempo

- Agregação anual por `Year`:
  - `avg_resale_value`, `avg_co2_saved`,
  - custos médios (`maintenance`, `insurance`, `charging`),
  - `avg_battery_health`, `avg_mileage`, `vehicle_count`.
- Gráficos de linha mostrando a evolução dos indicadores;
- Regressões lineares simples de cada indicador em função do ano;
- Tabela com inclinação, p-valor e R² de cada tendência;
- Seção textual com **insights de tendências de mercado**.

### Passo 4 — Parte #3: Modelo Preditivo

- Preparação de features (numéricas + categóricas) e variável alvo (`Resale_Value_USD`);
- Pré-processador com:
  - imputação de medianas + `StandardScaler` para numéricas;
  - imputação da moda + `OneHotEncoder` para categóricas.
- Treinamento e avaliação de:
  - `LinearRegression`
  - `RandomForestRegressor`
  - `GradientBoostingRegressor`
- Métricas:
  - RMSE (teste + cross-validation),
  - MAE,
  - R².
- Ensemble simples com média das previsões dos modelos;
- Importância de variáveis (feature importance) do melhor modelo de árvore;
- Seção final com **conclusões e recomendações para o negócio**.

---

## 6. Ferramentas e bibliotecas principais

- **Python 3.8+**
- Análise e manipulação de dados:
  - `pandas`, `numpy`
- Visualização:
  - `matplotlib`, `seaborn`
- Estatística e regressão:
  - `statsmodels`
- Machine Learning:
  - `scikit-learn`
- Ambiente:
  - `jupyter`, `joblib`

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