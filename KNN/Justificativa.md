KNN � escolha, treino, avalia��o e interpreta��o (LIME)
Por que KNN e como treinamos

Treino: train.csv balanceado (SMOTE) para reduzir vi�s por densidade;

Teste: test.csv com distribui��o real para avalia��o honesta.

Padroniza��o: StandardScaler (KNN � baseado em dist�ncias).

Busca de hiperpar�metros (GridSearchCV):

k ? {3,�,31} (�mpares), weights ? {uniform, distance}, Minkowski com p ? {1,2} (Manhattan/Euclidiana).

Valida��o: StratifiedKFold(5); sele��o do modelo: refit="macro_f1".

Relato de m�tricas: acur�cia, F1-macro e F1-weighted + matriz de confus�o para diagnosticar confus�es entre classes.

Interpreta��o local com LIME

Exemplo explicado: idx = 2 � true = Enrolled (1), pred = Dropout (0).

Como ler o gr�fico:

Verde (peso +): empurra a predi��o a favor da classe analisada (aqui, Dropout).

Vermelho (peso ?): puxa contra essa classe.

Contribui��es locais (para Dropout):

Application mode: +0.065 ? aumentou a probabilidade de Dropout na vizinhan�a do exemplo.

Curricular units 2nd sem (approved): ?0.088 ? mais unidades aprovadas no 2� semestre reduzem a probabilidade de Dropout (coerente pedagogicamente).

O LIME fornece explica��es locais: as import�ncias valem para este aluno e sua vizinhan�a; outros casos podem ter explica��es diferentes.