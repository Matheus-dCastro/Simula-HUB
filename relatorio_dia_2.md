# Relatório de Atividades – Dia 2: Fluido Real, Viscosidade, Reynolds e Navier–Stokes

**Curso:** SimulaHub · Módulo 1 – CFD e Modelagem Hemodinâmica  
**Data da Atividade:** 04/11  
**Notebook:** `SimulaHub_Dia2_Viscosidade_Reynolds_Poiseuille.ipynb`  
**Referência:** Slide `Dia 2 – Fluido real viscosidade, Reynolds e Navier–Stokes - Dia 2 – Fluido real viscosidade, Reynolds e Navier–Stokes.pdf`  
**Status da Execução:** Concluído com sucesso (executado e validado via kernel `.venv`)

---

## 1. Visão Geral dos Conceitos Abordados
No Dia 2, a teoria foi aprofundada para contemplar a física dos fluidos reais e as equações fundamentais de transporte de momento:
- **Viscosidade e Lei de Newton para Tensão de Cisalhamento:** $\tau = \eta \frac{dv}{dy}$ e força de arrasto viscoso entre placas $F = \frac{\eta v A}{z}$. O sangue foi caracterizado com comportamento newtoniano sob taxas fisiológicas moderadas ($\eta = 4{,}0\text{ mPa}\cdot\text{s}$, aproximadamente 4 vezes a viscosidade da água).
- **Número de Reynolds e Regimes de Escoamento:** $Re = \frac{\rho v D}{\eta}$. Identificação dos regimes laminar ($Re < 2000$), transição ($2000 \le Re \le 3000$) e turbulento ($Re > 3000$). Efeito da estenose na aceleração e no aumento de $Re_2 = \frac{Re_1}{1-s}$.
- **Lei de Hagen-Poiseuille e Resistência Hidráulica:** Perfil parabólico de velocidades $v(r) = \frac{\Delta P}{4\eta L}(R^2 - r^2)$, com velocidade máxima no centro e vazão proporcional à quarta potência do raio ($Q \propto R^4$).
- **Simulação Numérica 1D de Navier-Stokes:** Discretização das equações de momento com termo fonte de gradiente de pressão e termo difusivo ($\rho \partial_t v = G + \eta \partial_y^2 v$), condições de contorno de não-deslizamento nas paredes, análise de convergência e limite de estabilidade de von Neumann (CFL difusivo).

---

## 2. Resolução dos Exercícios

### Exercício 1 – Viscosímetro de Placas
- **Enunciado:** Placa de $20\text{ cm} \times 20\text{ cm}$ ($A = 0{,}04\text{ m}^2$) separada por $0{,}5\text{ mm}$ de fluido, deslocando-se a $5{,}0\text{ cm/s}$ ($0{,}05\text{ m/s}$).
- **Resultados Numéricos:**
  - Água ($20^\circ\text{C}$, $\eta = 1{,}0\text{ mPa}\cdot\text{s}$): $F = \mathbf{0{,}0040\text{ N}} = \mathbf{4{,}0\text{ mN}}$
  - Sangue ($37^\circ\text{C}$, $\eta = 4{,}0\text{ mPa}\cdot\text{s}$): $F = \mathbf{0{,}0160\text{ N}} = \mathbf{16{,}0\text{ mN}}$
  - Óleo de motor ($30^\circ\text{C}$, $\eta = 0{,}20\text{ Pa}\cdot\text{s}$): $F = \mathbf{0{,}8000\text{ N}} = \mathbf{800{,}0\text{ mN}}$
  - Glicerina ($20^\circ\text{C}$, $\eta = 1{,}41\text{ Pa}\cdot\text{s}$): $F = \mathbf{5{,}6400\text{ N}} = \mathbf{5640{,}0\text{ mN}}$
- **Gráfico:** Barra horizontal em escala logarítmica destacando a variação de mais de 3 ordens de grandeza.
- **Item (c):** Para o sangue atingir $F = 1{,}0\text{ mN}$, a velocidade necessária é $v = \frac{F z}{\eta A} = \mathbf{3{,}125\text{ mm/s}}$ ($0{,}3125\text{ cm/s}$). Comentou-se que esta velocidade é extremamente lenta (ordem de grandeza do deslocamento de insetos ou ponteiros mecânicos), refletindo a baixa força de cisalhamento exigida sob baixas taxas de deformação.

---

### Exercício 2 – Água, Sangue e Turbulência
- **Resultados Numéricos e Analíticos (Item b):**
  - Em tubo de diâmetro $D = 2{,}0\text{ cm}$, a velocidade para transição inicial ($Re = 2000$) é:
    - Água: $v_{\text{crítico}} = \frac{2000 \cdot \eta}{\rho D} = \mathbf{10{,}0\text{ cm/s}}$ ($0{,}10\text{ m/s}$).
    - Sangue: $v_{\text{crítico}} = \frac{2000 \cdot \eta}{\rho D} = \mathbf{37{,}74\text{ cm/s}}$ ($0{,}3774\text{ m/s}$).
  - A maior viscosidade cinemática do sangue permite escoar a velocidades quase quatro vezes superiores à água antes de perder a estabilidade laminar.
- **Gráfico (Item a):** $Re \times v$ marcando as faixas laminar ($Re < 2000$), transição ($2000 \le Re \le 3000$) e turbulento ($Re > 3000$).
- **Estenose na Aorta (Item c):**
  - Aorta saudável ($Q = 5\text{ L/min}$, $R = 1{,}25\text{ cm}$): $Re_1 \approx 1125$.
  - Como $Re_2 = \frac{Re_1}{1 - s}$, para atingir regime turbulento ($Re_2 \ge 3000$):
    $$s \ge 1 - \frac{1125}{3000} = \mathbf{62{,}5\%}$$
  - A partir de $62{,}5\%$ de estenose em diâmetro, o escoamento na garganta atinge regime plenamente turbulento, gerando dissipação caótica e o sopro vascular audível.

---

### Exercício 3 – Vasoconstrição e Vasodilatação
- **Resultados Numéricos (Item b):**
  - **Vasoconstrição ($-20\%$ no raio):**
    - Vazão relativa ($Q/Q_0 = (0{,}8)^4$): $\mathbf{40{,}96\%}$ (queda de quase $60\%$ no fluxo para a mesma pressão!).
    - Fator de $\Delta P$ para manter $Q_0$: $\mathbf{2{,}44\times}$ ($+144\%$ de elevação no gradiente pressórico!).
  - **Vasodilatação ($+20\%$ no raio):**
    - Vazão relativa ($Q/Q_0 = (1{,}2)^4$): $\mathbf{207{,}36\%}$ (mais que o dobro da vazão, $+107\%$).
    - Fator de $\Delta P$ para manter $Q_0$: $\mathbf{0{,}48\times}$ (redução de mais da metade, $-52\%$).
- **Discussão Fisiológica (Item c):** O corpo utiliza as arteríolas para regulação porque o raio atua elevado à **4ª potência** ($R^4$), garantindo uma modulação colossal de fluxo com mínimo esforço muscular e metabólico, enquanto o comprimento vascular é anatomicamente invariável e a viscosidade é uma variável sistêmica lenta.

---

### Exercício 4 – Solver Numérico 1D de Navier-Stokes
- **Experimento (a) - Viscosidade:**
  - Ao dobrar $\eta$ de $4$ para $8\text{ mPa}\cdot\text{s}$, a velocidade máxima final caiu exatamente pela metade ($20{,}0\text{ cm/s} \to 10{,}0\text{ cm/s}$), compatível com $v_{\text{max}} = \frac{G h^2}{2\eta}$.
  - A escala de tempo característica de convergência $\tau = h^2/\nu$ caiu pela metade ($26{,}5\text{ s} \to 13{,}2\text{ s}$), acelerando a convergência física.
- **Experimento (b) - Refinamento de Malha:**
  - $N = 11$: $\Delta y = 2{,}0\text{ mm}$, $\Delta t = 424\text{ ms}$, 142 iterações.
  - $N = 51$: $\Delta y = 0{,}4\text{ mm}$, $\Delta t = 16{,}96\text{ ms}$, 3.538 iterações.
  - $N = 201$: $\Delta y = 0{,}1\text{ mm}$, $\Delta t = 1{,}06\text{ ms}$, 56.604 iterações.
  - O refinamento exige redução quadrática de $\Delta t$ ($\Delta t \propto \Delta y^2$), elevando cubicamente o tempo de processamento total ($O(N^3)$).
- **Experimento (c) - Estabilidade Numérica:**
  - Fator $0{,}4 \le 0{,}5$: esquema de Euler explícito estável, convergência assintótica suave.
  - Fator $0{,}6 > 0{,}5$: violação do critério CFL difusivo ($\Delta t \le \frac{\Delta y^2}{2\nu}$), causando divergência numérica exponencial e overflow (`NaN`) em apenas alguns passos de tempo.
- **Item (d) - Conceitos de CFD:** Detalhada a correlação de cada bloco do código com malha espacial, condições de contorno de não-deslizamento, discretização temporal, iterações e critérios de convergência.

---

## 3. Verificação e Checklist
- [x] Cálculos analíticos validados
- [x] Códigos executados sem warnings com geração de todas as figuras
- [x] Experimentos do solver 1D conduzidos e comparados com teoria
- [x] Relatório estruturado gerado
