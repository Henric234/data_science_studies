

### Template
- Hipótese sobre os dados:
- O que faz:
- Pontos fortes:
- Pontos fracos:
- Como otimiza/tuna:
- O que entra/sai:

---

# Resumo dos Modelos Supervisionados
## Regressão
### Regressão Linear
- Hipótese sobre os dados:
    - Supõe que a target pode ser represetada por uma combinação linear das features (relação linear entre features e target, target é um hiperplano sobre as features)
    - Supõe que o noise nos dados é constante (não há heteroelasticidade)
- O que faz:
    - Calcula a reta (hiperplano) que minimiza o RMSE para os dados, com método direto (mínimo global, método direto)
- Pontos fortes:
    - Parâmetros interpretáveis (módulo maior, mais importante para a feature)
    - Fit e predict rápidos
- Pontos fracos:
    - Instável quanto à outlier (custam mais ao RMSE, deslocam a reta)
    - Não tem hiper-parâmetro para tunar
- Como otimiza/tuna:
    - Regularização
    - Remoção de outlier
    - Aplicar log nas features (ajuda na heteroelasticidade e pode deixar os dados "mais lineares")
- O que entra/sai:
    - Entram features numéricas e saem predicts numéricos
    - Categórico pode virar dummy

### Ridge
- Hipótese sobre os dados:
    - Relação linear entre features e target
    - Parâmetros dentro de uma bola na métrica l2 (raio dado pelo $\lambda$)
- O que faz:
    - Encontra parâmetros que minimizam RMSE +$\lambda||\beta||_2$
- Pontos fortes:
    - Predict e inferência rápidos
    - Menos sucetível à interferência de outlier
    - Parâmetros mais interpretáveis (importância das features mais clara)
- Pontos fracos:
    - Não zera parâmetros sem importância
- Como otimiza/tuna:
    - Precisa normalizar features para regularização fazer sentido
    - Testa $\lambda$ para regularizar parâmetros
- O que entra/sai:
    - Entram features numéricas e saem predicts numéricos
    - Categórico pode virar dummy

### Lasso
- Hipótese sobre os dados:
    - Relação linear entre features e target
    - Parâmetros dentro de uma bola na métrica l1 (raio dado pelo $\lambda$)
- O que faz:
    - Encontra parâmetros que minimizam RMSE +$\lambda||\beta||_1$
- Pontos fortes:
    - Inferência rápida
    - Menos sucetível à interferência de outlier
    - Parâmetros mais interpretáveis (features não importantes vão ser 0)
- Pontos fracos:
    - Algoritmo de fit pode precisar de muitas iterações
- Como otimiza/tuna:
    - Precisa normalizar features para regularização fazer sentido
    - Testa $\lambda$ para regularizar parâmetros
- O que entra/sai:
    - Entram features numéricas e saem predicts numéricos
    - Categórico pode virar dummy

### Regressão Não-Linear
- Hipótese sobre os dados:
    - Relação das features com target dada pela função escolhida (target é uma hipersuperficie sobre as features)
- O que faz:
    - Cria features aplicando funções não lineares às features/ multiplicando features e fita uma regressão linear sobre esse espaço
- Pontos fortes:
    - Consegue capturar colaboração entre features e relções não lineares
    - Parâmetros possivelmente interpretáveis
- Pontos fracos:
    - Variância maior, não muito estável
    - Pode overfittar, parâmetros ficam enormes
- Como otimiza/tuna:
    - Escolhe bem as funções e composições
        - Foward/Fackward/Mixed selection
    - Regulariza os parâmetros
        - Lasso e Ridge
- O que entra/sai:
    - Entram features numéricas e saem predicts numéricos
        - Mesmo que seja regressão logística, a saída é um número de [0,1] que sugere probabilidade
    - Categórico pode virar dummy

### Support Vector Regression
- Hipótese sobre os dados:
    - Supõe que a kernel aplicada as features permita que a target seja colocada entre dois hiperplanos 
- O que faz:
    - Transforma as features usando a função do kernel (não faz isso mesmo, usa kernel trick pra calcular os produtos internos, sendo mais eficiente que a regressão não linear) e faz duas SVM's para "classificar" o noise acima e abaixo da target
- Pontos fortes:
    - Não precisa transformar as features, sendo mais eficiente
        - Transformação do Radial Kernel teria dimensão infiníta e seria literalmente impossível de fazer com regressão
- Pontos fracos:
    - Parâmetros não interpretáveis
- Como otimiza/tuna:
    - Z scaling, para "aglutinar" os dados bem próximos de maneira regular (mais facil de separar do noise)
    - Escolhe a kernel
    - Tuna os parâmetros C e $\varepsilon$
- O que entra/sai:
    - Entram features numéricas e saem predicts numéricos
    - Categórico pode virar dummy
---
## Regressão e Classificação

### Árvores
- Hipótese sobre os dados:
    - target tem mesma média em regiões retangulares nas features
    - Algoritmo guloso consegue encontrar tais regiões e médias
- O que faz:
    - A cada iteração(nó), seleciona a feature com maior variância na target e separa no meio, tendo cada região restante a estimativa como a média dos targets de treino que ficaram naquela região(folha da árvore)
- Pontos fortes:
    - Consegue capturar diversos tipo de relação entre features e não lineares
    - Fit e predict rápido
    - Resulta numa árvore de decisão muito clara
    - Gini permite entender relevância das variáveis
- Pontos fracos:
    - Variância relativamente alta, pode mudar um tanto conforme amostra
    - Algorirtmo de fit pode não conseguir encontrar boa separação pelo paradigma guloso
- Como otimiza/tuna:
    - Limita profundidade máxima pra evitar overfit
    - Coloca combinação, aplica função nas features, rotaciona o espaço (PCA, Partial Least Squares)
    - Boosta (adaboost e xgboost) arvores pequenas pra aumentar diminuir a loss
    - Bootstrap/randomforest para arvores profundas para reduzir variância
        - Contudo, se perde a interpetabilidade de ter somente uma árvore
- O que entra/sai:
    - Entra: features numéricos, categoricos encodados
    - Saída:
        - No caso de regressão, média das features em cada folha
        - No caso de classificação, pode ser voto majoritário
            - Calibrar curva ROC

### K-nn
- Hipótese sobre os dados:
    Supõe que existe k tal que a média dos k vizinhos mais próximos (métrico então depende da norma e da escala) de um exemplo é igual ao seu target
- O que faz:
    - Calcula a média/voto majoritário dos k vizinhos mais próximos
- Pontos fortes:
    - Altamente adaptavel (variância altíssima e bias pequeno)
    - Não tem muito fit
- Pontos fracos:
    - Nada interpretável
    - Indeciso quanto á dados próximos de fronteira/derivada grande
    - Depende fortemente da amostra (variância alta)
    - Dados longe de clumps não tem muito parâmetro
- Como otimiza/tuna:
    - Seleciona K (validação cruzada, pra tunar qualquer parâmetro)
    - Normaliza features
    - Pega mais dados
    - Bagging talvez funcione
    - Troca de modelo
- O que entra/sai:
    - Entra dado numérico (mesmo que encoder de classe) e sair qualquer coisa

### Redes Neurais
- Hipótese sobre os dados:
    - Supõe que os target pode ser descrito pela função de ativação aplicada na composição linear (função afim pois tem o termo independente/bias) das features e repetido o processo em cada cada, a depender da topologia da rede
- O que faz:
    - Roda o algoritmo de backpropagate para fazer descida de gradiente e minimizar a loss
        - Backpropagate calcula a derivada de cada camada, começando da última e usando a regra da cadeia pra calcular das próximas até calcular a derivada da rede toda
- Pontos fortes:
    - Adaptável à dados não estruturados (imagens)
    - Paralelizável (stockastical gradient descent), então pode ficar ráido
- Pontos fracos:
    - Precisa de muitos dados pra convergir
        - Muitos parâmetros
    - Totalmente black box, nada interpretável
- Como otimiza/tuna:
    - Função/funções de ativação
    - Inicialização dos pesos
    - Algoritmo de descida de gradiente e learning rate
    - Topologia da rede
- O que entra/sai: 
    - Depende da rede

---

## Classificação

### Regressão Logística
- Hipótese sobre os dados:
    - Supõe que uma combinação linear das features passadas pela função logística caracteriza a probabilidade de pertencer àquela classe
- O que faz:
    - Fita a função logística baseado nas features. Fronteira não linear
- Pontos fortes:
    - Simples de interpretar
    - Retorna probabilidade então dá pra calibrar curva ROC
- Pontos fracos:
    - classes precisam estar bem separadas entre si, e dados da mesma amostra próximos
    - Só duas classes
        - para multiclass, one vs all ou one versus one
- Como otimiza/tuna:
    - Regularização (l1 e l2)
    - maximo de iterações
    - Feature selection
    - Feature engeneering

### LDA
- Hipótese sobre os dados:
    - Supõe que todas as classes são distribuídas de Gaussianas com mesma matrix de covariância, variando a média entre si
- O que faz:
    - Encontra a matriz de covariância e as médias, gerando uma fronteira de decisão linear
- Pontos fortes:
    - Estima prob, dá pra calibrar ROC
    - Funciona multiclasses
    - Resultados interpretáveis
    - Fit e predict rápidos
    - Pode ter classe com poucos dados, sua média pode ser enviesada mas a matriz de covariância usa todos os dados
- Pontos fracos:
    - Hipótese limitada, muito bias
    - Dados precisam estar bem separados
- Como otimiza/tuna:
    - bagging, ensemble
    - Regularização dos parâmetros Shrinkage
        - Tem que normalizar features

### QDA
- Hipótese sobre os dados:
    - Supõe que todas as classes são distribuídas de Gaussianas com a média e a matrix de covariância variando entre si
- O que faz:
    - Encontra as matrizes de covariância e as médias, gerando uma fronteira de decisão não-linear (quadrática)
- Pontos fortes:
    - Estima prob, dá pra calibrar ROC
    - Simples e interpretável
    - Funciona multiclasses
    - Fit e preditct rápidos
- Pontos fracos:
    - Precisa de uma quantidade de dados por classe, pois precisa, pra cada uma, estimar média e matriz de covariância
- Como otimiza/tuna:
    - bagging, ensemble

### Naive Bayes
- Hipótese sobre os dados:
    - Supõe que
        - Bernoulli: features vieram de distribuições de Bernoulli (0 ou 1) (útil para onehot encoder)
        - Gaussiano: Assume que features foram amostradas de distribuição normal
- O que faz:
    - Usa a distribuição à posteriori para estimar a probablidade à priori usando o Teorema de Bayes da probabilidade condicional
- Pontos fortes:
    - Multiclass
    - Tem método para só incrementar os parâmetros, bom para escalar
    - Estima probablidade, dá pra tunar ROC
- Pontos fracos:
    - Classes precisam estar bem separados
    - Precisa entender a distribuição das features
- Como otimiza/tuna:
    - Feature selection
    - Encontra qual distribuição se assemelha aos dados
    - Boosting, bootstaping

### SVM
- Hipótese sobre os dados:
    - Supõe que, ao trasnformar as features pela função kernel, as classes possam ser "bem" separadas por um hiperplano
        - "bem" entre parenteses pois depende de C para definir quantas falhas podem ocorrer nessa separação
- O que faz:
    - Transforma as features usando a função do kernel (não faz isso mesmo, usa kernel trick pra calcular os produtos internos, sendo mais eficiente que a regressão não linear) e encontra o hiperplano com maior margem entre as duas classes
- Pontos fortes:
    - Distância da margem indica "confiança" na predição
    - Melhor que jogar features não lineares em outros modelos, por conta do kernel trick
        - Transformação do kernel radial teria dimensões infinitas, sendo impossível de calcular
- Pontos fracos:
    - Binário, para multiclass precisa de one vs all ou one versus one
- Como otimiza/tuna:
    - Scalar
    - Testa kernel
    - Parâmetro C



# Avalição de Modelos
## Métricas
### KS
### Gini
### AUC 
### RMSE
### MAE
### F1
### Recall
### Precision
### R²
---
## Validação Cruzada
### Holdout
### Leave One Out
### K fold
### Out of sample
### Out of time


# Resumo dos Modelos Não-Supervisionados

## Métodos
### K-means, K-medoids, K-means++ e etc
### Dbscan
### Hierarquicos
### GMM
### PCA
---
## Definição do Número de Cluster
### Joelho/ Elbow
### Sillouette
### Distância Intra/Entre-Cluster
## etc. (motivo à priori ou prático)

# Data Prep
## Tratamento de missings 
### Remover
- Pode enviesar a base
    - Missing pode conter informação
- Pode funcionar se proporção for pequena ou for falta proporcional
### Set média/1/0/Flag
- Colocar uma flag ou settar o valor como 0, como a média da feature
    - A média numa regressão linear seria o equivalente à ignorar a feature, o que pode ser positivo
- A depender do modelo que vai ser utilizado pode interferir
- Pode se criar uma dummy de missing. Uma árvore pode conseguir capturar a info
### Fazer predict
- Modelo composto, pode ser desnecessariamente complexo
- Mas pode só tapar os buracos muito bem, um modelos simples para poucos dados.
### Dropar feature
- Se existir uma coluna fortemente correlacionada, pode fazer sentido ignorar a feature, pois pode carregar pouca informação e atrapalhar com os dados faltantes
---
## Tratamento de outliers
### Remover media+-2DP
- Pode remover dados que atrapalham o modelo
    - Regressão linear pode ser bom
- Pode retirar dados que contem informação
    - Knn seria péssimo, melhor manter

### Aplicar transformação
- Aplicar a função log para tentar deixar os valores menores
- Pode deixar mais difícil de separar algumas classes, mas reduz a variância
    - Uma árvore poderia decidir melhor por qual feature separar
### Settar flag 
- Colocar ">=5000"
- A rigor é uma transformação
    - Em dados lineares, seria péssimo, aumentaria muito a loss principalmente pq os dados mais extremos pesam mais na loss
    - Caso a target desse grupo seja próxima, pode ajudar uma árvore a melhorar
---
## Categorização de variáveis continuas e discretas
### Variáveis discretas
- Se poucos valores únicos, pode valer a pena fazer dummy
- Pode juntar, com método não supervisionado, em grupos
- Pode juntar por faixas, intervalos de mesmo tamanho
### Variáveis contínuas
- Pode juntar, com método não supervisionado, em grupos
- Pode juntar por faixas, intervalos de mesmo tamanho ou intervalos que crescem para agrupar os grandes juntos
    - Pode ajudar a balancear as classes
---
## PCA
- Encontra o hiperplano no qual a projeção dos dados tem a maior variância. Portanto, o vetor normal desse hiperplano descreve os dados, sendo a combinação linear de features.
- Essencialmente, junta features que carregam a mesma informação (pois são correlacionadas)
    - Pode ajudar o modelo à não ter que escolher entre dar prioridade à uma ou outra feature.
- Buscar trazer features mais independentes, no sentido de serem LI, enquanto reduz variância dos dados
    - Reduz a variância dos dados, podendo evitar overfit
- Quanto mais vetores, melhor explica a variância dos dados, melhor representa os dados

