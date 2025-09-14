KNN – escolha, treino, avaliação e interpretação (LIME)
Por que KNN e como treinamos

Treino: train.csv balanceado (SMOTE) para reduzir viés por densidade;

Teste: test.csv com distribuição real para avaliação honesta.

Padronização: StandardScaler (KNN é baseado em distâncias).

Busca de hiperparâmetros (GridSearchCV):

k ? {3,…,31} (ímpares), weights ? {uniform, distance}, Minkowski com p ? {1,2} (Manhattan/Euclidiana).

Validação: StratifiedKFold(5); seleção do modelo: refit="macro_f1".

Relato de métricas: acurácia, F1-macro e F1-weighted + matriz de confusão para diagnosticar confusões entre classes.

Interpretação local com LIME

Exemplo explicado: idx = 2 — true = Enrolled (1), pred = Dropout (0).

Como ler o gráfico:

Verde (peso +): empurra a predição a favor da classe analisada (aqui, Dropout).

Vermelho (peso ?): puxa contra essa classe.

Contribuições locais (para Dropout):

Application mode: +0.065 ? aumentou a probabilidade de Dropout na vizinhança do exemplo.

Curricular units 2nd sem (approved): ?0.088 ? mais unidades aprovadas no 2º semestre reduzem a probabilidade de Dropout (coerente pedagogicamente).

O LIME fornece explicações locais: as importâncias valem para este aluno e sua vizinhança; outros casos podem ter explicações diferentes.