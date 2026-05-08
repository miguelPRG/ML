# LinkedIn Discoverability Score — Guião de Apresentação

## Objetivo do projeto

Este projeto tem como objetivo desenvolver um modelo de **Machine Learning supervisionado** capaz de atribuir um **score de discoverability**, de **0 a 100**, a:

- perfis do LinkedIn;
- posts/publicações do LinkedIn.

Discoverability é o grau de facilidade com que um perfil ou publicação pode ser encontrado, recomendado ou ganhar visibilidade dentro de uma plataforma digital como o LinkedIn. Como o algoritmo real do LinkedIn não é público, este conceito foi tratado como um score académico de 0 a 100, construído a partir dos dados disponíveis, permitindo treinar modelos de Machine Learning para prever esse nível de visibilidade.

> Nota importante: este projeto **não replica o algoritmo real do LinkedIn**. O score criado é um **proxy académico**, construído a partir dos dados disponíveis, para permitir treinar e avaliar modelos de regressão.

---

## Problema de Machine Learning

O problema foi tratado como **aprendizagem supervisionada**, porque existe uma variável-alvo que o modelo aprende a prever:

- `profile_discoverability_score`
- `post_discoverability_score`

Foi usada **regressão**, porque o objetivo não é classificar em categorias como “baixo”, “médio” ou “alto”, mas prever um valor contínuo entre **0 e 100**.

---

## Estrutura do projeto

```text
ML/
├── data/
│   ├── processed/       # Dados limpos/processados
│   ├── train/           # Dados de treino
│   ├── validation/      # Dados de validação
│   └── test/            # Dados de teste
│
├── models/              # Modelos treinados e guardados
│
├── notebooks/
│   ├── dataProcessing.ipynb
│   ├── featureEngeneering.ipynb
│   └── modelTraining.ipynb
│
├── pyproject.toml       # Dependências do projeto
├── uv.lock
└── README.md
```

---

## Ordem dos notebooks

### 1. `dataProcessing.ipynb`

Neste notebook faço a primeira fase do projeto:

- carregamento dos datasets;
- análise inicial dos dados;
- limpeza de valores nulos;
- tratamento de campos em formato JSON/listas;
- criação dos scores de discoverability;
- análise exploratória dos dados.

Aqui explico o **problema**, os **dados usados** e como foram criados os targets.

---

### 2. `featureEngeneering.ipynb`

Neste notebook preparo os dados para o treino dos modelos:

- criação de features numéricas;
- criação de features derivadas;
- tratamento de variáveis categóricas com `OneHotEncoder`;
- normalização de variáveis numéricas com `StandardScaler`;
- criação de embeddings textuais com `SentenceTransformer`;
- divisão dos dados em treino, validação e teste.

A utilização de embeddings permite transformar texto, como `headline`, `about` e `content`, em vetores numéricos que capturam informação semântica.

---

### 3. `modelTraining.ipynb`

Neste notebook treino e avalio os modelos:

- carregamento dos dados finais;
- treino de modelos simples;
- treino de modelos otimizados;
- utilização de `XGBRegressor`;
- otimização com `RandomizedSearchCV`;
- avaliação em validação e teste;
- comparação de métricas;
- gravação dos modelos finais.

---

## Justificação das escolhas técnicas

### Aprendizagem supervisionada

Usei aprendizagem supervisionada porque o modelo aprende a partir de exemplos onde já existe um score conhecido.  
Cada linha tem features de entrada e um target que o modelo tenta prever.

### Regressão

Usei regressão porque o output esperado é numérico e contínuo.  
O modelo deve prever um valor entre **0 e 100**, não uma classe fixa.

### XGBoost

O XGBoost foi escolhido porque é adequado para dados tabulares e consegue capturar relações não-lineares entre variáveis.

Neste projeto isso é importante porque a discoverability não depende apenas de uma variável isolada. Por exemplo:

- muitos seguidores não garantem alta discoverability;
- comentários podem ser mais relevantes do que reações;
- o tipo de media pode influenciar o alcance;
- texto, experiência, skills e engagement podem interagir entre si.

O XGBoost também permite regularização e otimização de hiperparâmetros, reduzindo o risco de overfitting.

### Cross-validation e RandomizedSearchCV

Usei validação cruzada com `RandomizedSearchCV` para testar várias combinações de hiperparâmetros sem depender de uma única configuração manual.

Foram testados parâmetros como:

- `n_estimators`;
- `learning_rate`;
- `max_depth`;
- `subsample`;
- `colsample_bytree`;
- `reg_alpha`;
- `reg_lambda`.

Isto ajuda a encontrar um modelo mais robusto e com melhor capacidade de generalização.

---

## Métricas de avaliação

Foram usadas métricas de regressão:

- **MAE** — erro absoluto médio;
- **RMSE** — penaliza erros maiores;
- **R²** — indica quanta variância do target é explicada pelo modelo.

Estas métricas permitem perceber se o modelo está apenas a decorar os dados ou se consegue generalizar para o conjunto de teste.

---

## Pontos importantes para defender oralmente

- O objetivo não é copiar o LinkedIn, mas criar um proxy académico de discoverability.
- O problema é supervisionado porque existe um target.
- É regressão porque o score é contínuo entre 0 e 100.
- Os notebooks seguem uma pipeline lógica: processamento → feature engineering → treino/avaliação.
- O XGBoost foi escolhido por ser forte em dados tabulares e relações não-lineares.
- A validação cruzada ajuda a tornar a escolha dos hiperparâmetros mais robusta.
- A avaliação final deve ser feita com dados de teste, não apenas com treino.

---

## Como executar

Instalar dependências:

```bash
uv sync
```

Abrir o Jupyter:

```bash
uv run jupyter lab
```

Executar os notebooks pela ordem:

```text
1. notebooks/dataProcessing.ipynb
2. notebooks/featureEngeneering.ipynb
3. notebooks/modelTraining.ipynb
```

---

## Resumo para apresentação de 10 minutos

1. Apresentar o objetivo do projeto.
2. Mostrar a estrutura do repositório.
3. Abrir `dataProcessing.ipynb` e mostrar os dados + criação dos scores.
4. Abrir `featureEngeneering.ipynb` e explicar as features e embeddings.
5. Abrir `modelTraining.ipynb` e mostrar treino, XGBoost, cross-validation e métricas.
6. Terminar com limitações: o score é um proxy académico e não o algoritmo real do LinkedIn.
