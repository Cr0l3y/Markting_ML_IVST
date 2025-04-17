***EN***

# Project

# Marketing Investment Classifier - Machine Learning Project (ENGLISH VERSION)

## 📊 Objective
This project aims to classify customers who are likely to join a bank's marketing investment campaign using supervised machine learning algorithms.

---

## 🔍 Initial Data Analysis

- **Missing Values**: No null values found in the dataset.
- **Data Consistency**: Some inconsistencies found in text format (e.g., uppercase/lowercase).
- **Variable Types**:
  - The target variable `aderencia_investimento` is categorical: **classification problem**.
  - `inadimplencia` and `fez_emprestimo` only have "sim" (yes) and "nao" (no) values.

---

## 📊 Exploratory Data Analysis (EDA)

- Distribution of `aderencia_investimento`: 502 "sim", 766 "nao".
- Bar charts for categorical features (`estado_civil`, `escolaridade`, etc.)
- Summary of numerical variables:
  - `idade`: from 19 to 87
  - `saldo`: may contain negative values

---

## 🧹 Data Preprocessing

### 1. Separating Features and Target
```python
x = dados.drop("aderencia_investimento", axis=1)
y = dados["aderencia_investimento"]
```

### 2. One-Hot Encoding for Categorical Features
- We used `drop='if_binary'` so that binary columns like `inadimplencia` and `fez_emprestimo` are converted to a single column: 0 for "nao", 1 for "sim".
```python
from sklearn.preprocessing import OneHotEncoder
encoder = OneHotEncoder(drop='if_binary')
x_encoded = encoder.fit_transform(x)
colunas = encoder.get_feature_names_out()
```

### 3. Label Encoding for the Target Variable
```python
from sklearn.preprocessing import LabelEncoder
label_encoder = LabelEncoder()
y = label_encoder.fit_transform(y)  # 1 = sim, 0 = nao
```

### 4. Train-Test Split
```python
from sklearn.model_selection import train_test_split
x_treino, x_teste, y_treino, y_teste = train_test_split(x_encoded, y, test_size=0.3, random_state=42)
```

---

## 🤖 Modeling and Evaluation

### ■ Dummy Classifier (Baseline)
```python
from sklearn.dummy import DummyClassifier
dummy = DummyClassifier(strategy='most_frequent')
dummy.fit(x_treino, y_treino)
score_dummy = dummy.score(x_teste, y_teste)  # 60.2%
```

### ■ Decision Tree Classifier
```python
from sklearn.tree import DecisionTreeClassifier
modelo_arvore = DecisionTreeClassifier()
modelo_arvore.fit(x_treino, y_treino)
```
- Full tree: 100% accuracy (overfitting)

#### Limiting Depth to Prevent Overfitting
```python
modelo_podado = DecisionTreeClassifier(max_depth=3)
modelo_podado.fit(x_treino, y_treino)
score_podado = modelo_podado.score(x_teste, y_teste)  # 71.6%
```

### ■ K-Nearest Neighbors (KNN)

#### Scaling Data to Range 0-1
```python
from sklearn.preprocessing import MinMaxScaler
scaler = MinMaxScaler()
x_treino_scaled = scaler.fit_transform(x_treino.toarray())
x_teste_scaled = scaler.transform(x_teste.toarray())
```

#### Fitting KNN
```python
from sklearn.neighbors import KNeighborsClassifier
knn = KNeighborsClassifier()
knn.fit(x_treino_scaled, y_treino)
score_knn = knn.score(x_teste_scaled, y_teste)  # 68%
```

---

## 📊 Accuracy Comparison

| Model                  | Accuracy |
|------------------------|----------|
| DummyClassifier        | 60.2%    |
| Decision Tree (max=3) | 71.6%    |
| KNN                   | 68%      |

---

## 📂 Saving Models with Pickle
```python
import pickle
with open("modelo_onehotenc.pkl", "wb") as f:
    pickle.dump(encoder, f)
with open("modelo_arvore.pkl", "wb") as f:
    pickle.dump(modelo_podado, f)
```

---

## 🚀 Summary
- Successfully built our first classification model using decision trees.
- Also implemented KNN to compare performances.
- Best result came from the **Decision Tree with controlled depth**.
- All encodings and models were saved using `pickle` for reuse in future predictions.




---
***PT-BR***

# Projeto

# Classificador de Investimentos em Marketing - Projeto de Aprendizado de Máquina (VERSÃO EM INGLÊS)

## 📊 Objetivo
Este projeto visa classificar clientes com probabilidade de participar de uma campanha de investimento em marketing de um banco usando algoritmos supervisionados de aprendizado de máquina.
---
## 🔍 Análise Inicial de Dados

- **Valores Ausentes**: Nenhum valor nulo encontrado no conjunto de dados.
- **Consistência dos Dados**: Algumas inconsistências encontradas no formato do texto (por exemplo, letras maiúsculas/minúsculas).
- **Tipos de Variáveis**:
    - A variável alvo `aderencia_investimento` é categórica: **problema de classificação**.
    - `inadimplencia` e `fez_emprestimo` só possuem valores "sim" (sim) e "nao" (não).

---
## 📊 Análise Exploratória de Dados (EDA)

- Distribuição de `aderencia_investimento`: 502 "sim", 766 "nao".
- Gráficos de barras para características categóricas (`estado_civil`, `escolaridade`, etc.)
- Resumo das variáveis ​​numéricas:
    - `idade`: de 19 a 87
    - `saldo`: pode conter valores negativos

---
## 🧹 Pré-processamento de dados

### 1. Separando Recursos e Alvo
```píton
x = dados.drop("aderência_investimento", eixo=1)
y = dados["adesão_investimento"]
```

### 2. Codificação One-Hot para Recursos Categóricos
- Usamos `drop='if_binary'` para que colunas binárias como `inadimplencia` e `fez_emprestimo` sejam convertidas em uma única coluna: 0 para "nao", 1 para "sim".
```python
from sklearn.preprocessing import OneHotEncoder
encoder = OneHotEncoder(drop='if_binary')
x_encoded = encoder.fit_transform(x)
colunas = encoder.get_feature_names_out()
```

### 3. Codificação de rótulo para a variável de destino
```python
from sklearn.preprocessing import LabelEncoder
label_encoder = LabelEncoder()
y = label_encoder.fit_transform(y)  # 1 = sim, 0 = nao
```

### 4. Divisão de Treinamento e Teste
```python
from sklearn.model_selection import train_test_split
x_treino, x_teste, y_treino, y_teste = train_test_split(x_encoded, y, test_size=0.3, random_state=42)
```

---

## 🤖 Modelagem e Avaliação

### ■ Classificador Dummy (Linha de Base)
```python
from sklearn.dummy import DummyClassifier
dummy = DummyClassifier(strategy='most_frequent')
dummy.fit(x_treino, y_treino)
score_dummy = dummy.score(x_teste, y_teste)  # 60.2%
```

### ■ Classificador de Árvore de Decisão
```python
from sklearn.tree import DecisionTreeClassifier
modelo_arvore = DecisionTreeClassifier()
modelo_arvore.fit(x_treino, y_treino)
```
- Árvore completa: 100% de precisão (overfitting)

#### Limitando a profundidade para evitar overfitting
```python
modelo_podado = DecisionTreeClassifier(max_depth=3)
modelo_podado.fit(x_treino, y_treino)
score_podado = modelo_podado.score(x_teste, y_teste)  # 71.6%
```
### ■ K-Vizinhos Mais Próximos (KNN)

#### Escalando Dados para o Intervalo de 0 a 1
```python
from sklearn.preprocessing import MinMaxScaler
scaler = MinMaxScaler()
x_treino_scaled = scaler.fit_transform(x_treino.toarray())
x_teste_scaled = scaler.transform(x_teste.toarray())
```
#### Adaptação KNN
```python
from sklearn.neighbors import KNeighborsClassifier
knn = KNeighborsClassifier()
knn.fit(x_treino_scaled, y_treino)
score_knn = knn.score(x_teste_scaled, y_teste)  # 68%
```

---

## 📊 Comparação de Precisão

| Model                  | Accuracy |
|------------------------|----------|
| DummyClassifier        | 60.2%    |
| Decision Tree (max=3) | 71.6%    |
| KNN                   | 68%      |

---

## 📂 Salvando modelos com Pickle
```python
import pickle
with open("modelo_onehotenc.pkl", "wb") as f:
    pickle.dump(encoder, f)
with open("modelo_arvore.pkl", "wb") as f:
    pickle.dump(modelo_podado, f)
```

---
## 🚀 Resumo
- Construímos com sucesso nosso primeiro modelo de classificação usando árvores de decisão.
- Também implementamos KNN para comparar desempenhos.
- O melhor resultado veio da **Árvore de Decisão com profundidade controlada**.
- Todas as codificações e modelos foram salvos usando `pickle` para reutilização em previsões futuras.