## Etapa 1: Pré-processamento

### Dataset: **Predict Students' Dropout and Academic Success** - [UCI ML Repository](https://archive.ics.uci.edu/dataset/697/predict+students+dropout+and_academic_success)
- **Instâncias**: 4.424 estudantes
- **Features**: 36 variáveis
- **Target**: 3 classes (Dropout, Enrolled, Graduate)


### Arquivo `preprocessing.ipynb`
- Análise inicial dos dados
- Verificação de valores ausentes
- Tratamento de variáveis categóricas
- Balanceamento com SMOTE
- Normalização de variáveis numéricas
- Divisão dos dados (80/20)
  - `train.csv`: 5.301 amostras (balanceadas - 33.3% cada classe)
  - `test.csv`: 885 amostras (distribuição original para avaliação real)
  - 40 features + 1 coluna target (0=Dropout, 1=Enrolled, 2=Graduate)