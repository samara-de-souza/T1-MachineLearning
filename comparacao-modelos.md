# Comparação de Interpretabilidade

## 1. Dataset e Experimento
Dataset: Predict Students' Dropout and Academic Success — UCI ML Repository. 

- Instâncias originais: 4.424 estudantes; 36 variáveis brutas.  
- Target: 3 classes (0=Dropout, 1=Enrolled, 2=Graduate).  
- Pré-processamento (ver `Data/preprocessing.ipynb`): análise inicial, tratamento de ausentes, codificação de categóricas, normalização de numéricas e balanceamento com SMOTE para o conjunto de treino.  
- Split 80/20 com estratificação:  
  - `train.csv`: 5.301 amostras balanceadas (≈33.3% por classe), 40 features + `Target`.  
  - `test.csv`: 885 amostras com a distribuição original (avaliação realista).  

Observação: os notebooks de cada modelo carregam esses arquivos e executam o fluxo de treino/avaliação sobre esse split.

## 2. Desempenho dos Modelos
| Modelo | Accuracy | F1-macro | Observação |
|-------|-----------|----------|------------|
| K-NN  | 0.64 | 0.58 | k=3, distância Manhattan |
| Árvore de Decisão | 0.69 | 0.63 | profundidade 30, gini |
| Gaussian NB | 0.65 | 0.61 | var_smoothing=1e-4 |

## 3. Interpretabilidade (nos notebooks)
Para manter reprodutibilidade e evitar duplicação, as análises detalhadas estão dentro de cada notebook:

- K-NN: `KNN/knn.ipynb` — Importância de Permutação (F1-macro) e explicações locais com LIME (acerto, erro, aleatório).  
- Árvore de Decisão: `DecisionTree/algorithm_tree.ipynb` — Visualização do topo da árvore, `feature_importances_` (Gini) e Importância de Permutação.  
- Naïve Bayes: `NaiveBayes/naive_bayes.ipynb` — Priors, θ/σ² e ranking global via |μ₂−μ₀|/σ.

## 4. Comparação Cruzada
| Feature | K-NN (perm) | Árvore (Gini) | NB (\|μ₂−μ₀\|/σ) |
|---|---:|---:|---:|
| `Curricular units 2nd sem (approved)` | 3º | 1º | 1º |
| `Curricular units 2nd sem (grade)` | 9º | 3º | 2º |
| `Curricular units 1st sem (approved)` | 4º | 8º | 3º |
| `Curricular units 1st sem (grade)` | – | 10º | 4º |
| `Tuition fees up to date_1` | 1º | 2º | 5º |
| `Debtor_1` | 2º | – | 7º |
| `Curricular units 2nd sem (enrolled)` | – | 5º | 11º |
| `Curricular units 1st sem (enrolled)` | – | 4º | 12º |

– **Convergência**: desempenho curricular (aprovações/notas 1º–2º sem) e adimplência aparecem sistematicamente entre os primeiros lugares nos três modelos.  
– **Consistência**: Árvore e NB concordam fortemente no topo; K-NN (perm) também traz as mesmas variáveis, com “Tuition fees” e “Debtor” muito altos — coerente com a natureza baseada em distância.  
– **Senso prático**: resultados fazem sentido no domínio: rendimento recente e situação financeira são preditores fortes de evasão/sucesso.

### Análise comparativa
- **Os resultados fizeram sentido?** Sim. Variáveis diretamente relacionadas ao desempenho recente do aluno (aprovadas/notas dos 1º e 2º semestres) e à situação financeira (tuition/debtor) explicam a permanência, matrícula ativa ou graduação.  
- **Houve concordância entre modelos?** Alta. A árvore (Gini/Permutação) e o NB (|μ₂−μ₀|/σ) colocam “Curricular units 2nd sem (approved)” no topo. O K-NN por permutação também destaca desempenho curricular e adiciona forte peso para “Tuition fees up to date_1”/“Debtor_1”.  
- **Ferramentas de interpretabilidade usadas:**  
  - K-NN: Importância de Permutação (global) e LIME (local).  
  - Árvore: Visualização do topo, `feature_importances_` (Gini) e Importância de Permutação.  
  - Naïve Bayes: Priors; θ/σ² por classe; ranking |μ₂−μ₀|/σ (global).  
- **Limitações de interpretabilidade:**  
  - K-NN: sem parâmetros globais; explicações variam por instância; sensível à escala.  
  - Árvore: pode ficar profunda; regras locais podem se multiplicar; risco de variância.  
  - Naïve Bayes: assume independência condicional e gaussianidade; correlações podem distorcer pesos.

## 5. Limitações
* K-NN: difícil interpretação global; sensível à escala; custo alto para LIME/SHAP.  
* Árvore: regras claras no topo, mas árvore profunda (>30) gera centenas de folhas; propensa a overfitting.  
* NB: supõe independência condicional e gaussianidade; altamente influenciado por features correlacionadas.

## 6. Conclusões
– Para stakeholders que precisam regras claras, Árvore é a escolha.  
– K-NN entrega desempenho similar, mas interpretabilidade depende de métodos pós-hoc.  
– NB é simples, rápido e razoavelmente interpretável via estatísticas, mas perdeu um pouco de F1.  
– Em todos os casos, as mesmas variáveis de rendimento aparecem como fatores-chave, reforçando confiança nos resultados.
