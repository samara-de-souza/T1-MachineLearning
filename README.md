Trabalho 1 – Interpretabilidade de Modelos de AM (2025/02)
Professor: Otávio Parraga

Integrantes: Alice Colares, Giovana Raupp, Mykelly Barros, Samara Tavares, Vitoria Gonzalez

## Dataset e Pré‑processamento
- Fonte: UCI – Predict Students' Dropout and Academic Success.
- Split 80/20 estratificado: `train.csv` (5.301 amostras balanceadas; 40 features + `Target`) e `test.csv` (885 amostras com distribuição original).
- Detalhes do pipeline (ausentes, codificação, normalização, SMOTE) em `Data/preprocessing.ipynb` e `Data/README.md`.

## Notebooks (treino, avaliação e interpretabilidade)
- K-NN: `KNN/knn.ipynb` – métricas; Importância de Permutação (global) e LIME (local) com 3 exemplos.
- Decision Tree: `DecisionTree/algorithm_tree.ipynb` – métricas; visualização do topo; `feature_importances_` (Gini) e Permutação.
- Naïve Bayes: `NaiveBayes/naive_bayes.ipynb` – métricas; priors, θ/σ² e ranking |μ₂−μ₀|/σ.

## Comparação global
- Relatório de síntese e análise cruzada: `comparacao-modelos.md` (convergências, ferramentas usadas e limitações por modelo).

## Como executar
1. Ambiente: Python 3.9+ (recomendado). Instale dependências necessárias (scikit-learn, imbalanced-learn, pandas, numpy, matplotlib, seaborn, lime).
2. Para reproduzir do zero o split e os CSVs processados, rode `Data/preprocessing.ipynb`.
3. Execute cada notebook de modelo em sequência (Run All) para treinar, avaliar e visualizar interpretabilidade.

Observação: justificativas e discussões estão dentro dos notebooks e no arquivo `comparacao-modelos.md`, conforme solicitado no enunciado.