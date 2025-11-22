## ⚡ Previsão de Consumo de Energia com Redes Neurais Recorrentes (LSTM)

Este projeto utiliza uma rede neural Long Short-Term Memory (LSTM) para prever o consumo de energia de eletrodomésticos. O modelo foi treinado com o dataset "Appliances Energy Prediction" (ID 374) do UCI Machine Learning Repository, que fornece dados temporais e variáveis ambientais. A abordagem LSTM é ideal para capturar as dependências sequenciais complexas no consumo de energia, oferecendo previsões de alta precisão.

O ponto crucial deste trabalho reside na **metodologia rigorosa de pré-processamento** para garantir que o modelo não sofra com **"data leakage" (vazamento de dados)**, uma falha comum em problemas de séries temporais.

---

### 🔑 Destaques e Metodologia Rigorosa

#### 1. Prevenção de Data Leakage no Escalonamento
Para manter a integridade temporal do treino e teste:
* O conjunto de dados foi dividido em treino e teste (80/20) **respeitando a ordem cronológica**.
* O `MinMaxScaler` do target (`scaler_target`) foi **ajustado (`fit`) exclusivamente nos dados de treino** da coluna `Appliances`.
* **Reversão Crítica:** As métricas (RMSE e MAE) e o gráfico comparativo são gerados **somente após reverter as previsões** para a escala original (Watts) usando o `scaler_target`, garantindo interpretabilidade.

#### 2. Configuração da Janela Temporal (Sliding Window)
* **Granularidade:** Os dados foram coletados a cada **10 minutos**.
* **`LOOK_BACK` (Janela de Observação):** Foi definido como **60 passos**, o que significa que o modelo utiliza **10 horas** de dados históricos (60 passos x 10 min/passo) para fazer a próxima previsão. Isso é crucial para capturar padrões de consumo diários.
* **Dimensionalidade:** Os dados de treino para a LSTM têm o formato: **(15728 amostras, 60 passos, 7 features)**.

#### 3. Ambiente e Treinamento
* **Aceleração:** O notebook foi configurado para utilizar **GPU (T4)**, o que é ideal para o treinamento de Redes Neurais Profundas como a LSTM.
* **Target e Features:** O alvo da previsão é `Appliances` e o conjunto de features inclui temperaturas internas/externas (`T1`, `RH_1`, `T_out`), pressão (`Press_mm_hg`), velocidade do vento (`Windspeed`) e consumo de luzes (`lights`).

---

### 📉 Resultados e Métricas de Desempenho

As métricas foram calculadas na **escala original (Watts)**, revertidas pelo `scaler_target`.

| Métrica | Valor | Interpretação |
| :--- | :--- | :--- |
| **RMSE** | **57.47 Watts** | O desvio padrão dos erros de previsão (na mesma unidade do target). |
| **MAE** | **25.92 Watts** | O erro absoluto médio das previsões. O modelo erra, em média, ~26 Watts. |
| **Test Loss (MSE Escal.)** | **0.002885** | O valor de perda do modelo na escala `[0, 1]` no conjunto de teste. |

---

### 📊 Gráfico de Comparação (Valores Reais vs. Previstos)

O gráfico abaixo mostra um trecho das previsões no conjunto de teste, revertidas para a escala original (Watts), demonstrando a aderência do modelo à série temporal.

<img width="1266" height="630" alt="image" src="https://github.com/user-attachments/assets/7183f046-28c3-40fa-9c27-cae3d3fb9013" />

---

### 🚀 Como Rodar o Projeto

1.  Clone este repositório.
2.  Abra o arquivo `RNN.ipynb` em um ambiente Jupyter (como Google Colab ou VS Code).
3.  Execute as células em ordem. O download dos dados e a instalação das dependências (como `tensorflow.keras`) serão tratados automaticamente pelo notebook.
