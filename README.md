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