# Previsão de Aluguel de Bicicletas com Azure Machine Learning

Este projeto implementa um modelo de regressão utilizando **Aprendizado de Máquina Automatizado (AutoML)** no **Azure Machine Learning**.  
O modelo prevê o número esperado de aluguéis de bicicletas em um determinado dia, considerando variáveis sazonais e climáticas.

---

## 🚀 Passo a passo realizado

### 1. Criar espaço de trabalho no Azure ML
- Entre no [Portal do Azure](https://portal.azure.com).
- Crie um **recurso de Machine Learning** com:
  - Região: Leste dos EUA
  - Conta de armazenamento, cofre de chaves e insights automáticos.

### 2. Acessar o Azure Machine Learning Studio
- [Azure ML Studio](https://ml.azure.com).
- Conectar ao workspace recém-criado.

### 3. Carregar os dados
- Baixar dataset de [bike-rentals](https://aka.ms/bike-rentals).
- Upload como **mltable** no Azure ML Studio.

### 4. Criar trabalho de ML automatizado
- Nome do experimento: `mslearn-bike-rental`
- Tipo de tarefa: **Regressão**
- Coluna alvo: `rentals`
- Modelos permitidos: **RandomForest** e **LightGBM**
- Métrica primária: `NormalizedRootMeanSquaredError`
- Limites: máximo de 3 testes, timeout 15 min.

### 5. Avaliar o modelo
- Revisar gráficos de resíduos e `predicted_true`.
- Identificar o **melhor modelo** (normalmente LightGBM).

### 6. Implantar o modelo
- Implantação como **ponto de extremidade em tempo real**:
  - Nome do endpoint: `predict-rentals`
  - Tipo: Standard_DS3_v2 (ou equivalente)
  - Instâncias: 3

### 7. Testar o endpoint
- Exemplo de entrada:
  ```json
  {
    "input_data": {
      "columns": [
        "day","mnth","year","season","holiday",
        "weekday","workingday","weathersit",
        "temp","atemp","hum","windspeed"
      ],
      "index": [0],
      "data": [[1,1,2022,2,0,1,1,2,0.3,0.3,0.3,0.3]]
    }
  }
  ```
- Saída esperada:
  ```json
  [352.35]
  ```

---

## 📂 Arquivos do repositório
- `README.md`: passo a passo do processo
- `endpoints.json`: documentação do endpoint criado
