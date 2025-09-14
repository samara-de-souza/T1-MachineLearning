Naïve Bayes — Justificativa de Treinamento e Avaliação

Escolhemos F1-macro como métrica principal por se tratar de um problema multiclasse com desbalanceamento no conjunto real: ela dá peso igual a cada classe e evita que o bom desempenho nas classes mais frequentes esconda erros nas demais. Reportamos também Balanced Accuracy (média dos recalls por classe) como medida de cobertura equilibrada, além da Accuracy apenas como referência global. Para transparência, incluímos precisão, recall e F1 por classe via classification_report.

Todas as 40 variáveis de entrada já estão em formato **numérico** (continuous ou one-hot), tornando plausível a hipótese Gaussiana — daí a escolha do *Gaussian* Naïve Bayes como ponto de partida.

O treinamento usou GaussianNB com ajuste de var_smoothing por GridSearch (CV estratificada, 5 folds) maximizando F1-macro. Para evitar vazamento, reamostragem (SMOTE) e transformações foram encapsuladas em Pipeline e avaliadas dentro da CV; a avaliação final foi feita em hold-out (test.csv) preservando a distribuição real. Também aplicamos calibração de probabilidades (CalibratedClassifierCV, sigmoid) para obter probabilidades mais confiáveis (útil para limiares), sem esperar ganhos relevantes de acurácia.

Todos os passos que envolvem aleatoriedade (SMOTE, folds da CV, calibração) foram fixados com **random_state=42**, garantindo reprodutibilidade completa.

Resultados: após o ajuste, o NB apresentou Accuracy ≈ 0,652, F1-macro ≈ 0,610 e Balanced Accuracy ≈ 0,615, superiores ao baseline em equilíbrio entre classes (com acurácia semelhante). Mantivemos o GaussianNB com var_smoothing otimizado e calibração como modelo final do NB. Limitação conhecida: o NB assume independência condicional entre features; por isso priorizamos métricas equilibradas (F1-macro/Balanced Accuracy) e interpretabilidade (priors, médias θ e variâncias σ² por classe) em vez de perseguir apenas a maior acurácia.

Ganho absoluto observado: **F1-macro subiu ~7 p.p.** em relação ao GaussianNB baseline (de 0,537 para 0,610), comprovando o benefício do ajuste/correções sem sacrificar a acurácia global.