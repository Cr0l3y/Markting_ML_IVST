Texto traduzido no translate.google EN. - Texto em PT-BR 

Text translated on translate.google EN. - Text in PT-BR 



***EN***

# Project


Project: Analysis of Marketing Investment Data

    📌 Objective

This project aims to analyze data related to marketing investments and understand patterns that can help in future strategic decisions.


    🔍 Initial Data Analysis

Null Values: After the initial verification, it was found that there are no null values ​​in the dataset, therefore, there is no need to treat this aspect.

Data Consistency:

-Some categorical columns present values ​​that can harm the analysis, such as categories written in different ways (e.g.: "Male", "male", "M").

-There is also no guarantee that the numerical variables are within an acceptable range. For example, the variable "age" could contain negative or absurdly high values, which needs to be verified.

-The column "adherence_investment" is of the categorical type, which indicates that the problem is a classification problem and not a regression problem.


    📊 Exploratory Data Analysis (EDA)

Visualization with Graphs (Plotly)

The exploratory analysis was conducted using interactive graphs with Plotly, starting with the target variable investment_adherence:

- Two categories were identified: "yes" (502 records) and "no" (766 records), with no signs of inconsistency.


Analysis of Categorical Variables

- For variables such as marital_status, education, default, loan_taken, we used colored bar graphs based on the category (color='investment_adherence') to verify the integrity of the information.

- Although it is a repetitive analysis, it is essential to ensure that there are no outliers or typing problems.

Analysis of Numerical Variables

Using boxplots, we identified:

- Age: ranges from 19 to 87 years old, with no negative values ​​or values ​​above 200 years old.

- Balance: has negative values, which is acceptable, since it indicates possible debts.

Even with possible outliers, the data was kept, since it carries relevant information.

Exploratory analysis is a crucial step in Machine Learning projects, since it allows a better understanding of the data and detecting possible inconsistencies before modeling.


    🧹 Data Preparation

Separating Target Variable

        x = dados.drop("investment_adherence", axis=1)
        y = dados["investment_adherence"]

Treatment of Categorical Variables

- Many columns are in text format, which needs to be converted, since machine learning algorithms do not understand texts, only numbers.

- Although it is possible to transform the texts into simple numbers (e.g.: 1 = married, 2 = single), this would create a false hierarchy between the categories, which can negatively affect the model.

Solution: One-Hot Encoding

The One-Hot Encoding technique was used to transform categorical columns into numeric columns, without assigning false orders to the categories.

Each category is transformed into a new binary column:

1 indicates the presence of the feature.

0 indicates the absence.




---
***PT-BR***

# Projeto

Projeto: Análise de Dados de Investimentos em Marketing

    📌 Objetivo

Este projeto tem como objetivo analisar dados relacionados a investimentos em marketing e entender padrões que possam auxiliar em futuras decisões estratégicas.


    🔍 Análise Inicial dos Dados

Valores Nulos: Após a verificação inicial, foi constatado que não existem valores nulos no dataset, portanto, não há necessidade de tratamento nesse aspecto.

Consistência dos Dados:

-Algumas colunas categóricas apresentam valores que podem prejudicar a análise, como categorias escritas de formas diferentes (ex: "Masculino", "masculino", "M").

-Também não há garantias de que as variáveis numéricas estejam dentro de uma faixa aceitável. Por exemplo, a variável "idade" poderia conter valores negativos ou absurdamente altos, o que precisa ser verificado.

-A coluna "aderencia_investimento" é do tipo categórico, o que indica que o problema se trata de classificação e não de regressão.


    📊 Análise Exploratória dos Dados (EDA)


Visualização com Gráficos (Plotly)


A análise exploratória foi conduzida utilizando gráficos interativos com Plotly, começando pela variável alvo aderencia_investimento:

- Duas categorias foram identificadas: "sim" (502 registros) e "nao" (766 registros), sem indícios de inconsistência.

Análise das Variáveis Categóricas

- Para as variáveis como estado_civil, escolaridade, inadimplencia, fez_emprestimo, utilizamos gráficos de barras coloridos com base na categoria (color='aderencia_investimento') para verificar a integridade das informações.

- Apesar de ser uma análise repetitiva, ela é essencial para garantir que não há valores discrepantes ou problemas de digitação.


Análise das Variáveis Numéricas

Com o uso de boxplots, identificamos:

- Idade: varia de 19 a 87 anos, sem valores negativos ou acima de 200 anos.

- Saldo: possui valores negativos, o que é aceitável, já que indica possíveis dívidas.


Mesmo com possíveis outliers, os dados foram mantidos, pois carregam informações relevantes.

A análise exploratória é uma etapa crucial em projetos de Machine Learning, pois permite entender melhor os dados e detectar possíveis inconsistências antes da modelagem.




    🧹 Preparação dos Dados

Separando Variável Alvo

        x = dados.drop("aderencia_investimento", axis=1)
        y = dados["aderencia_investimento"]


Tratamento das Variáveis Categóricas

- Muitas colunas estão em formato textual, o que precisa ser convertido, já que algoritmos de machine learning não compreendem textos, apenas números.

- Embora seja possível transformar os textos em números simples (ex: 1 = casado, 2 = solteiro), isso criaria uma falsa hierarquia entre as categorias, o que pode afetar negativamente o modelo.



Solução: One-Hot Encoding

- Utilizamos o One-Hot Encoder para transformar as colunas categóricas em colunas binárias.

- Observação: Para colunas com apenas duas categorias como inadimplencia e fez_emprestimo, usamos o parâmetro drop='if_binary' no OneHotEncoder, para evitar a criação de colunas redundantes (ex: apenas uma coluna onde 1 = sim e 0 = nao).


        from sklearn.preprocessing import OneHotEncoder

        encoder = OneHotEncoder(drop='if_binary')
        x_encoded = encoder.fit_transform(x)
        colunas = encoder.get_feature_names_out()


- Armazenamos os nomes das colunas transformadas em uma variável (colunas) para manter controle sobre as novas variáveis geradas.

- O encoder guarda o padrão dos dados, o que permite aplicá-lo a futuros dados de forma consistente.



Transformação da Variável Alvo

- A variável aderencia_investimento também precisa ser transformada, pois está em formato "sim"/"nao".

- Utilizamos o LabelEncoder do Scikit-Learn:


        from sklearn.preprocessing import LabelEncoder
        label_encoder = LabelEncoder()
        y = label_encoder.fit_transform(y)
     Resultado: 1 = sim, 0 = nao


🤖 Modelagem e Avaliação


Divisão dos Dados:
- Para avaliar o desempenho do modelo, não utilizamos todos os dados de uma vez.
- Dividimos em dados de treino e dados de teste com train_test_split:

        from sklearn.model_selection import train_test_split
        x_treino, x_teste, y_treino, y_teste = train_test_split(x_encoded, y, test_size=0.3, random_state=42)


Modelo 1: DummyClassifier
- O DummyClassifier serve como uma baseline para avaliarmos o desempenho mínimo aceitável:


        from sklearn.dummy import DummyClassifier
        dummy = DummyClassifier(strategy='most_frequent')
        dummy.fit(x_treino, y_treino)
        score_dummy = dummy.score(x_teste, y_teste)
    Resultado: 60,2% de acerto



Modelo 2: Árvore de Decisão


- O modelo de árvore de decisão compara os valores das colunas para tomar decisões de classificação.


        from sklearn.tree import DecisionTreeClassifier

        modelo_arvore = DecisionTreeClassifier()
        modelo_arvore.fit(x_treino, y_treino)
        score_arvore = modelo_arvore.score(x_teste, y_teste)
    Resultado: 100% nos dados de treino

O modelo decorou os dados, o que causa overfitting.
- Aplicando Limite de Profundidade:



        modelo_podado = DecisionTreeClassifier(max_depth=3)
        modelo_podado.fit(x_treino, y_treino)
        score_podado = modelo_podado.score(x_teste, y_teste)
    Resultado: 71,6% de acerto

Com a poda da árvore (limite de profundidade), o modelo generalizou melhor, evitando o overfitting.

