# Relatório de Atividades – Dia 5: Do CFD aos Dados e à Inteligência Artificial

**Curso:** SimulaHub · Módulo 1 – CFD e Modelagem Hemodinâmica  
**Data da Atividade:** 07/11  
**Notebook:** `SimulaHub_Dia5_Dados_e_IA.ipynb`  
**Dataset:** `simulacoes_0d.csv` (600 simulações hemodinâmicas)  
**Referência:** Slide `Dia 5 – Do CFD aos dados e à Inteligência Artificial - Dia 5 – Do CFD aos dados e à Inteligência Artificial.pdf`  
**Status da Execução:** Concluído com sucesso (executado e validado via kernel `.venv`)

---

## 1. Visão Geral dos Conceitos Abordados
No Dia 5, conectou-se a modelagem computacional biomecânica à **ciência de dados e aprendizado de máquina**:
- **Simulações como Geradoras de Dados:** Cada rodada de simulação atua como um experimento numérico controlado gerando atributos de entrada (*features*) e alvos hemodinâmicos (*targets*).
- **Modelos Substitutos (*Surrogate Models*):** Emprego de algoritmos supervisionados (Regressão Linear, Árvores de Decisão e Random Forest) para aproximar a resposta do solver físico em tempo real (milissegundos).
- **Importância de Variáveis e Explicabilidade:** Avaliação por permutação (*permutation feature importance*) para confrontar as decisões da IA com as leis físicas de conservação e o modelo de Windkessel.
- **Limites e Falhas de Extrapolação:** Demonstração empírica de que modelos puramente estatísticos baseados em árvores são incapazes de extrapolar para além do envelope dos dados de treinamento.
- **Classificação de Risco e Limiares Clínicos:** Ajuste de limiares de decisão na Regressão Logística ponderando custos éticos e médicos entre Falsos Negativos (lesões críticas ignoradas) e Falsos Positivos (exames confirmatórios adicionais).

---

## 2. Resolução dos Exercícios

### Exercício 1 – Alvo Alternativo: Pressão Sistólica na Aorta (`P_sist_aorta`)
- **Desempenho do Modelo (Item a):**
  - MAE $= \mathbf{6{,}68\text{ mmHg}}$, RMSE $= \mathbf{9{,}49\text{ mmHg}}$, $R^2 = \mathbf{0{,}963}$.
  - A Random Forest explicou mais de $96\%$ da variação pressórica sistólica na aorta.
- **Importância das Variáveis (Item b):**
  1. `vazao_media`: importância $= \mathbf{1{,}431}$ (dominante)
  2. `R_saida`: importância $= \mathbf{0{,}192}$
  3. `reducao_area`: importância $= 0{,}096$
  4. `rigidez_EH`: importância $= 0{,}016$
  5. `raio_iliaca`: importância $= 0{,}004$
- **Discussão Fisiológica (Item c):**
  - Para a perda de carga $\Delta P$, a `reducao_area` é a variável determinante ($> 90\%$ de peso).
  - Já para a pressão sistólica na aorta (`P_sist_aorta`), as variáveis dominantes são o débito cardíaco (`vazao_media`), a resistência periférica (`R_saida`) e a rigidez arterial (`rigidez_EH`). A severidade da estenose em um único ramo periférico (ilíaca esquerda) tem impacto desprezível na raiz aórtica, pois o ramo contralateral sadio acomoda o fluxo desviado e a pressão central é ditada primariamente pela impedância de entrada do leito sistêmico global (efeito Windkessel).

---

### Exercício 2 – Quantas Simulações São Necessárias? (Curva de Aprendizado)
- **Evolução do Desempenho no Teste Fixo (Item a):**
  - $N = 30$ simulações: $R^2 = 0{,}536$ | MAE $= 6{,}00\text{ mmHg}$
  - $N = 60$ simulações: $R^2 = 0{,}686$ | MAE $= 4{,}72\text{ mmHg}$
  - $N = 120$ simulações: $R^2 = 0{,}735$ | MAE $= 4{,}31\text{ mmHg}$
  - $N = 240$ simulações: $R^2 = \mathbf{0{,}875}$ | MAE $= 2{,}94\text{ mmHg}$ $\to$ **Ultrapassa a meta de $R^2 \approx 0{,}80$**
  - $N = 480$ simulações: $R^2 = 0{,}841$ | MAE $= 2{,}81\text{ mmHg}$
- **Custo Computacional em CFD 3D (Item c):**
  - Considerando que uma simulação 3D de alta fidelidade consome tipicamente **4 horas de processamento**, gerar 200 a 240 casos exigiria de **800 a 960 horas de CPU contínuas** ($\sim \mathbf{33\text{ a }40\text{ dias}}$ de computação sem interrupções).
  - Isso demonstra que o custo de amostragem de dados é o principal gargalo em bioengenharia, tornando essencial o uso de **modelos multifidelidade** (onde modelos 0D/1D velozes treinam a base e poucas simulações 3D refinam o modelo).

---

### Exercício 3 – Extrapolação: O Modelo Fora da Zona de Conforto
- **Cenário de Teste:** Treino exclusivo com estenoses moderadas ($< 70\%$, 305 amostras) e teste exclusivamente em estenoses severas ($\ge 70\%$, 295 amostras).
- **Resultados no Teste Extrapolado (Item b):**
  - $\text{MAE} = \mathbf{15{,}64\text{ mmHg}}$ (uma piora drástica de $\mathbf{5{,}6\times}$ em relação ao teste interpolado balanceado de $2{,}81\text{ mmHg}$).
  - $R^2 = \mathbf{-0{,}364}$ (coeficiente estritamente negativo, revelando colapso preditivo).
- **Por que a IA Falha e Lições Físicas (Item c):**
  - A Random Forest opera particionando o espaço em hiper-retângulos médios; por construção, ela é incapaz de prever saídas maiores que o teto visto no treino ($\sim 19\text{ mmHg}$). Quando confrontada com a perda quadrática violenta de estenoses críticas ($> 50\text{ mmHg}$), a predição atinge um patamar artificial constante.
  - Isso prova que modelos de machine learning puros **não conhecem leis físicas fundamentais** e são perigosos fora do seu domínio restrito de treinamento. Em saúde e engenharia, a física é indispensável para garantir generalização e segurança.

---

### Exercício 4 – Escolhendo o Limiar de Decisão Clínico
- **Métricas por Limiar de Decisão (Item a):**
  - **Limiar 0,50 (Padrão):** Precisão $= 95{,}5\%$, Revocação $= 91{,}3\%$ | **Falsos Negativos $= 2$**, Falsos Positivos $= 1$.
  - **Limiar 0,30 (Recomendado):** Precisão $= 65{,}7\%$, Revocação $= \mathbf{100{,}0\%}$ | **Falsos Negativos $= 0$**, Falsos Positivos $= 12$.
  - **Limiar 0,10:** Precisão $= 43{,}4\%$, Revocação $= \mathbf{100{,}0\%}$ | **Falsos Negativos $= 0$**, Falsos Positivos $= 30$.
- **Justificativa Clínica de Triagem (Item c):**
  - Em triagem médica, o custo de um **falso negativo** é inaceitável (omissão diagnóstica de isquemia crítica, acarretando infarto ou morte do paciente). O custo de um **falso positivo** é meramente a realização de um exame confirmatório adicional de baixo risco.
  - O **limiar de 0,30** é a escolha ideal: alcança **$100\%$ de sensibilidade com ZERO falsos negativos**, mantendo uma precisão clínica de $65{,}7\%$, evitando a sobrecarga de testes desnecessários que ocorreria no limiar 0,10.

---

## 3. Verificação e Checklist
- [x] Dataset `simulacoes_0d.csv` auditado, corrigido e validado
- [x] Modelos de regressão (Linear e Random Forest) e classificação logística executados
- [x] Análises de curva de aprendizado, extrapolação e limiares de decisão implementadas
- [x] Relatório estruturado gerado
