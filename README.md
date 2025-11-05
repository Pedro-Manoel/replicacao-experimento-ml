# Replicação de Experimento — Comparação entre Modelos Deep Learning e Shallow Learning

Este repositório contém os artefatos desenvolvidos para o projeto da disciplina de Aprendizagem de Máquina, cujo objetivo foi **replicar um experimento reportado em artigo científico**, seguindo a **Opção 1** das diretrizes do projeto:  
> O artigo original utiliza modelos de *Deep Learning*, e nesta replicação foram implementados modelos de *Shallow Learning* para comparação dos resultados.


## 📘 Estrutura do Projeto

```

├── analysis/                  # Arquivos LaTeX e análises complementares geradas automaticamente
├── article_code/              # Código do artigo original
├── data/                      # Conjunto de dados utilizados (arquivos CSV por cidade)
├── plots/                     # Gráficos e figuras resultantes dos experimentos
├── results/                   # Resultados salvos em CSV (métricas de desempenho)
├── README.md                  # Este arquivo
├── analise_resultados.ipynb   # Notebook de análise comparativa entre abordagens
├── replicacao_fiel.ipynb      # Replicação fiel do artigo (modelos shallow com setup original)
└── replicacao_otimizada.ipynb # Replicação otimizada com engenharia de features e tuning

```

## 🧩 Descrição do Projeto

O trabalho propõe uma **replicação experimental** de um artigo focado em previsão com **modelos de Deep Learning**.  
Nesta replicação, foram implementados **modelos de Shallow Learning** utilizando os mesmos dados, horizonte de previsão e protocolo experimental do estudo original, a fim de **avaliar o impacto da complexidade do modelo sobre o desempenho preditivo**.

Foram conduzidas **duas abordagens experimentais**:

### 🔹 Abordagem 1 — Replicação Fiel
Objetivo: reproduzir o desenho experimental do artigo original da forma mais fiel possível, alterando apenas o tipo de modelo.

**Características principais:**
- Mesmas *features* exógenas: `ALLSKY_KT`, `T2M`, `RH2M`, `Month`, `WS10M`;
- Defasagem (lag) de 1 dia para previsão de `t` a partir de `t−1`;
- Divisão *holdout*: últimos 30 dias para teste;
- Modelos avaliados:
  1. **Ridge Regression**
  2. **Support Vector Regressor (SVR)**
  3. **Random Forest Regressor**
  4. **XGBoost Regressor**
- Normalização com `MinMaxScaler` (ajustado apenas no treino).

### 🔹 Abordagem 2 — Replicação Otimizada
Objetivo: explorar o potencial máximo dos modelos *shallow*, incorporando engenharia de *features* e otimização de hiperparâmetros.

**Principais diferenças:**
- **Engenharia de Features Avançada:**
  - Features de calendário (`Year`, `Month`, `Day`, `DayOfWeek`, `DayOfYear`, `WeekOfYear`);
  - Representação cíclica de variáveis sazonais (`Month`, `DayOfYear`, `DayOfWeek`);
  - Lags de 1, 2, 3, 7, 14 e 30 dias para variáveis meteorológicas e alvo;
  - Estatísticas de janelas móveis (médias, desvios, máximos e mínimos) em janelas de 7, 14 e 30 dias.
- **Otimização de Hiperparâmetros:**  
  - `RandomizedSearchCV` com `TimeSeriesSplit (5 splits)` para busca eficiente;
- **Validação Cruzada (Backtesting):**  
  - *Rolling origin* (5 divisões) em todo o conjunto temporal para estimar a generalização.

## ⚙️ Requisitos

Para executar os notebooks, é necessário ter instalado:

```bash
Python >= 3.9
seaborn 
scikit-posthocs
scikit-learn 
matplotlib 
pandas 
numpy 
statsmodels 
xgboost 
scipy
```

## 🚀 Execução

1. **Clonar o repositório:**

   ```bash
   git clone https://github.com/Pedro-Manoel/replicacao-experimento-ml.git
   cd replicacao-experimento-ml
   ```

2. **Organizar os dados:**
   Os arquivos CSV das cidades devem estar na pasta `data/`.
   Cada arquivo representa uma cidade diferente usada nos experimentos.

3. **Selecionar a cidade para o experimento:**

   * Os notebooks (`replicacao_fiel.ipynb` e `replicacao_otimizada.ipynb`) operam **com uma cidade por vez**.
   * A cidade é definida por meio de uma **flag** no código (`STATION_SELECTED`).
   * Para rodar outra cidade, basta alterar o valor dessa flag no início do notebook e reexecutar.

4. **Executar os experimentos:**

   * Replicação fiel:

     ```bash
     jupyter notebook replicacao_fiel.ipynb
     ```
   * Replicação otimizada:

     ```bash
     jupyter notebook replicacao_otimizada.ipynb
     ```

5. **Análise comparativa:**

   * Execute `analise_resultados.ipynb` para gerar gráficos e tabelas de comparação entre abordagens e cidades.

---

## 📊 Resultados

* Resultados numéricos (Métricas) estão em:

  ```
  results/
  ```
* Gráficos comparativos e visualizações estão em:

  ```
  plots/
  ```
* Arquivos LaTeX auxiliares (usados no relatório final) estão em:

  ```
  analysis/
  ```

---

Perfeito 👍 Aqui está uma versão **mais enxuta e direta** das principais conclusões, mantendo o rigor técnico e clareza:

---

## 🧠 Principais Conclusões

O projeto replicou o artigo *“Neural Networks Forecast Models Comparison for the Solar Energy Generation in Amazon Basin”*, originalmente baseado em *Deep Learning*, aplicando modelos de *Shallow Learning* (SVR, Ridge, Random Forest e XGBoost) sobre as mesmas 12 cidades da Bacia Amazônica.

1. **Formulação do Problema:**
   A análise mostrou que o artigo realiza previsões *one-step-ahead* (de *t−1* para *t*), o que caracteriza um problema de regressão supervisionada — e não uma previsão *multi-step* contínua.

2. **Desempenho dos Modelos Shallow:**
   Nessa formulação, os modelos *shallow* (especialmente o SVR) superaram significativamente os modelos *deep*, demonstrando maior estabilidade e menor erro.

3. **Simplicidade Eficiente:**
   A “Replicação Fiel” teve melhor desempenho que a “Otimizada”, indicando que o sinal *lag-1* é suficientemente informativo e que *features* adicionais podem introduzir ruído.

4. **Robustez:**
   Os resultados de *backtesting* confirmaram a consistência e robustez dos modelos *shallow* para previsões de irradiação solar diária.

Em síntese, a replicação mostrou que, para este problema, **modelos de regressão simples superam arquiteturas profundas**, devido à natureza do conjunto de dados e da formulação experimental do artigo original.


