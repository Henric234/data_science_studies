### Modelo
- Hipótese sobre os dados:
- O que faz:
- Pontos fortes:
- Pontos fracos:
- Como otimiza/tuna:
- O que entra/sai:

### Regressão Linear
- Hipótese sobre os dados:
    - Supõe que a target pode ser represetada por uma combinação linear das features (relação linear entre features e target)
    - Supõe que o noise nos dados é constante (não há heteroelasticidade)
- O que faz:
    - Calcula a reta (hiperplano) que minimiza o RMSE para os dados, com método direto
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
    - Entram 