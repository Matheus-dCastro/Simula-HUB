# Relatório de Atividades – Dia 1: Da Pressão ao Movimento dos Fluidos

**Curso:** SimulaHub · Módulo 1 – CFD e Modelagem Hemodinâmica  
**Data da Atividade:** 03/11  
**Notebook:** `SimulaHub_Dia1_Pressao_e_Escoamento.ipynb`  
**Referência:** Slide `Dia 1 – modulo1 - Dia 1 – modulo1.pdf`  
**Status da Execução:** Concluído com sucesso (executado e validado via kernel `.venv`)

---

## 1. Visão Geral dos Conceitos Abordados
No Dia 1, foram explorados os fundamentos da mecânica dos fluidos aplicada à hemodinâmica:
- **Estática dos Fluidos:** Definição de pressão manométrica, densidade sanguínea ($\rho = 1060\text{ kg/m}^3$), gravidade ($g = 9{,}81\text{ m/s}^2$) e equação fundamental da hidrostática ($P = P_0 + \rho g h$).
- **Cinemática dos Fluidos e Continuidade:** Conservação de massa em fluidos incompressíveis ($Q = A v = \text{constante}$), relação entre raio vascular e velocidade média.
- **Dinâmica dos Fluidos e Equação de Bernoulli:** Conservação de energia mecânica ao longo de linhas de corrente ($P + \frac{1}{2}\rho v^2 + \rho g z = \text{constante}$), efeito Venturi e queda transitória de pressão na garganta de estenoses.
- **Limites do Modelo Ideal:** Omissão de atrito viscoso e perda de carga permanente em expansões bruscas pós-estenóticas.

---

## 2. Resolução dos Exercícios

### Exercício 1 – Hidrostática: Humano × Girafa
- **Enunciado:** Comparar a coluna hidrostática entre o coração e a cabeça no ser humano ($\Delta h = 0{,}4\text{ m}$) e na girafa ($\Delta h = 2{,}0\text{ m}$).
- **Resultados Numéricos:**
  - Diferença hidrostática no humano: $\Delta P = \rho g \Delta h \approx 4.159{,}4\text{ Pa} \approx \mathbf{31{,}20\text{ mmHg}}$.
  - Diferença hidrostática na girafa: $\Delta P = \rho g \Delta h \approx 20.797{,}2\text{ Pa} \approx \mathbf{155{,}99\text{ mmHg}}$.
- **Gráfico:** Gerado gráfico comparativo no intervalo de $0$ a $2{,}5\text{ m}$, destacando com marcadores os pontos anatômicos do humano e da girafa.
- **Discussão Fisiológica (Item c):** Para manter a perfusão cerebral mínima de $60\text{ mmHg}$, a girafa deve gerar no coração uma pressão média mínima de $P_{\text{coração}} = 60 + 156 \approx \mathbf{216\text{ mmHg}}$ (com pressões sistólicas acima de 280–300 mmHg), o que justifica sua acentuada hipertrofia ventricular esquerda e controle reflexo barorreceptor único.

---

### Exercício 2 – Débito Cardíaco em Exercício Físico
- **Enunciado:** Avaliar a velocidade média do sangue na aorta ($r = 1{,}25\text{ cm}$, $A \approx 4{,}91\times 10^{-4}\text{ m}^2$) em repouso ($5\text{ L/min}$) e exercício intenso ($20\text{ L/min}$).
- **Resultados Numéricos:**
  - Repouso ($5\text{ L/min}$): $v = \mathbf{0{,}1698\text{ m/s}} \approx \mathbf{16{,}98\text{ cm/s}}$.
  - Exercício ($20\text{ L/min}$): $v = \mathbf{0{,}6791\text{ m/s}} \approx \mathbf{67{,}91\text{ cm/s}}$.
- **Gráfico:** Curva de $v$ (cm/s) vs. $Q$ (L/min) de $3$ a $25\text{ L/min}$ com destaque aos pontos de operação.
- **Justificativa (Item c):** Relação estritamente linear ($v = Q/A$). Com área fixa, a velocidade média na aorta é diretamente proporcional ao débito cardíaco, quadruplicando quando a vazão quadruplica.

---

### Exercício 3 – Qual Estenose Multiplica a Velocidade por 5?
- **Dedução Analítica (Item a):**
  $$Q = A_0 v_0 = A_g v_g \implies \frac{v_g}{v_0} = \frac{A_0}{A_g} = \frac{1}{(1-s)^2} = 5$$
  $$(1-s) = \frac{1}{\sqrt{5}} \implies s = 1 - \frac{1}{\sqrt{5}} \approx \mathbf{55{,}28\%}$$
- **Confirmação Numérica e Gráfica (Item b):** Gráfico de $v_g/v_0$ vs. $s$ confirmando a interseção exata em $s = 55{,}28\%$.
- **Comparação Diâmetro vs. Área (Item c):**
  - Redução de área: $\frac{1}{1-s_{\text{área}}} = 5 \implies s_{\text{área}} = \mathbf{80{,}0\%}$.
  - Discussão clínica: Como $A \propto D^2$, uma redução moderada em diâmetro luminal ($55\%$) oculta uma estenose crítica de área ($80\%$), crucial para interpretação de angiografia vs. IVUS.

---

### Exercício 4 – Queda de Pressão e os Limites do Modelo Ideal
- **Resultados Numéricos e Gráficos (Itens a e b):**
  - $\Delta P = \frac{1}{2} \rho v_0^2 \left[ \frac{1}{(1-s)^4} - 1 \right] \propto Q^2$.
  - Razão entre as curvas para $20\text{ L/min}$ e $5\text{ L/min}$: exatamente **$16\times$** ($4^2 = 16$).
  - Para estenose de $70\%$, a queda de pressão de Bernoulli passa de $\sim 14\text{ mmHg}$ em repouso para mais de $200\text{ mmHg}$ em exercício.
- **Discussão Crítica (Item c):**
  - O modelo de Bernoulli falha em vasos reais ao ignorar a viscosidade (condição de não-deslizamento), a separação da camada limite após a garganta e a formação de vórtices na expansão súbita.
  - A perda de carga irreversível leva à hipoperfusão distal dos tecidos a jusante (gerando isquemia miocárdica em coronárias) e submete o ventrículo a uma sobrecarga pressórica crônica.

---

## 3. Verificação e Checklist
- [x] Resolução analítica completa
- [x] Códigos implementados sem warnings e com gráficos rotulados
- [x] Execução do notebook validada
- [x] Relatório estruturado gerado
