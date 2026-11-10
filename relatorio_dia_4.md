# Relatório de Atividades – Dia 4: CFD Cardiovascular: Do Vaso Normal à Estenose

**Curso:** SimulaHub · Módulo 1 – CFD e Modelagem Hemodinâmica  
**Data da Atividade:** 06/11  
**Notebook:** `SimulaHub_Dia4_Interpretacao.ipynb`  
**Dataset:** `dados_dia4.npz`  
**Referência:** Slide `Dia 4 – CFD cardiovascular do vaso normal à estenose 2 - Dia 4 – CFD cardiovascular do vaso normal à estenose 2.pdf`  
**Status da Execução:** Concluído com sucesso (executado e validado via kernel `.venv`)

---

## 1. Visão Geral dos Conceitos Abordados
No Dia 4, o foco centrou-se na **interpretação física, clínica e biomecânica** dos campos hemodinâmicos gerados por CFD:
- **Campos de Velocidade e Vorticidade:** Identificação de jatos de alta velocidade, descolamento de camada limite, camadas de cisalhamento livre e recirculação pós-estenótica.
- **Gradientes de Pressão:** Conversão Venturi/Bernoulli na estenose versus dissipação turbilhonar irreversível (perda de carga permanente) na expansão súbita.
- **Tensão de Cisalhamento Parietal (WSS):** Diferenciação entre regiões com hipercisalhamento erosivo (garganta) e regiões com baixo cisalhamento/recirculação retrógrada (aterogênese distal).
- **Hemodinâmica Pulsátil:** O papel da inércia transitória e da desaceleração sistólica na amplificação drástica dos vórtices e do cisalhamento oscilatório (OSI).

---

## 2. Resolução dos Exercícios

### Exercício 1 – Hipóteses Teóricas × Resultados de Simulação
- **Estimativa de Velocidade Máxima:** $v_{\text{máx}} \approx 1{,}5 \frac{U}{1 - s}$.
  - $0\%$: Teórica $= 7{,}08\text{ cm/s}$ | Simulada $= 7{,}10\text{ cm/s}$ ($+0{,}4\%$).
  - $30\%$: Teórica $= 10{,}11\text{ cm/s}$ | Simulada $= 9{,}11\text{ cm/s}$ ($-9{,}9\%$).
  - $50\%$: Teórica $= 14{,}15\text{ cm/s}$ | Simulada $= 12{,}49\text{ cm/s}$ ($-11{,}7\%$).
  - $65\%$: Teórica $= 20{,}22\text{ cm/s}$ | Simulada $= 17{,}59\text{ cm/s}$ ($-13{,}0\%$).
- **Por que a estimativa é maior que a simulação (Item b):** A teoria assume um perfil parabólico de Poiseuille completamente desenvolvido na garganta ($v_{\text{máx}} = 1{,}5 v_{\text{méd}}$). Na estenose real, o estreitamento é curto; a aceleração vigorosa no bocal convergente gera um núcleo de velocidade achatado (*plug-like*), onde $v_{\text{máx}} / v_{\text{méd}} < 1{,}5$, além do efeito do atrito viscoso pré-estenótico.
- **Crescimento da Velocidade vs. WSS (Item c):**
  - Razão entre 65% e 0%: Velocidade máx $= \mathbf{2{,}48\times}$ | WSS máx $= \mathbf{11{,}60\times}$.
  - A WSS escala com o quadrado do estreitamento ($\tau_w \approx \mu \frac{v_{\text{máx}}}{\delta} \propto \frac{U}{(1-s)^2}$), enquanto a velocidade escala linearmente com $(1-s)^{-1}$.

---

### Exercício 2 – Dinâmica de Pressão no Eixo
- **Resultados Numéricos (Item a):**
  - Caso 0%: $P_{\text{mín}} \approx -0{,}0041\text{ mmHg}$; $P(5{,}6\text{ cm}) \approx 0{,}0000\text{ mmHg}$.
  - Caso 65%: Queda brusca para $P_{\text{mín}} = -0{,}0406\text{ mmHg}$ na garganta; $P(5{,}6\text{ cm}) \approx -0{,}0000\text{ mmHg}$.
- **Bernoulli vs. Simulação (Item b):**
  - Entre $x = 1{,}0\text{ cm}$ ($v_1 = 7{,}04\text{ cm/s}$) e a garganta ($v_g = 17{,}59\text{ cm/s}$):
  - $\Delta P_{\text{Bernoulli}} = \frac{1}{2}\rho(v_g^2 - v_1^2) = \mathbf{0{,}1033\text{ mmHg}}$.
  - $\Delta P_{\text{Simulação}} = \mathbf{0{,}1099\text{ mmHg}}$ (diferença de apenas $6{,}0\%$).
- **Mecanismos Físicos (Item c):**
  - A queda na garganta decorre da conversão de pressão estática em energia cinética (efeito Venturi/Bernoulli).
  - A recuperação incompleta após a estenose se deve à separação do escoamento na expansão divergente, onde o gradiente adverso de pressão gera vórtices e recirculações que dissipam irreversivelmente a energia cinética mecânica em calor (perda de carga).

---

### Exercício 3 – Mapeamento de Risco Parietal (WSS)
- **Resultados Numéricos (Item a):**
  - Posição do pico de WSS: $x = \mathbf{2{,}35\text{ cm}}$ (ápice da garganta), com $\text{WSS}_{\text{máx}} = \mathbf{1{,}66\text{ Pa}}$.
  - Região com WSS retrógrada ($< 0$): $x \in [\mathbf{2{,}67\text{ cm}}, \mathbf{4{,}55\text{ cm}}]$, configurando uma extensa zona de recirculação de $\mathbf{1{,}88\text{ cm}}$.
- **Perfil de Velocidade Retrógrada (Item b):**
  - No ponto médio da recirculação ($x \approx 3{,}61\text{ cm}$), o fluido junto à parede atinge velocidade estritamente negativa ($u = -0{,}80\text{ cm/s}$), confirmando o refluxo mecânico.
- **Implicações Clínicas (Item c):**
  - **A jusante (WSS baixa/oscilatória/negativa):** Favorece a **progressão e crescimento da placa aterosclerótica**. Prolonga o tempo de contato de partículas aterogênicas (LDL), desregula a barreira endotelial, inibe a eNOS e recruta monócitos inflamatórios.
  - **Na garganta (pico elevado de WSS):** Risco de **lesão endotelial aguda**. O cisalhamento mecânico extremo desgasta o glicocálix, pode causar erosão/ruptura da capa fibrosa da placa e desencadear trombose intravascular aguda (IAM/AVC).

---

### Exercício 4 – Análise Dinâmica do Ciclo Cardíaco Pulsátil
- **Resultados ao Longo das Fases (Item a e b):**
  - **Maior zona de WSS negativa:** Ocorre na **fase de desaceleração tardia ($t/T = 0{,}64$, fator $= 0{,}54$)**, onde a recirculação atinge expressivos $\mathbf{5{,}37\text{ cm}}$ de comprimento (mais de $67\%$ do vaso).
  - **Menor zona de WSS negativa:** Ocorre no **início da sístole / aceleração rápida ($t/T = 0{,}00$, fator $= 1{,}00$)**, onde o gradiente favorável empurra o fluxo e o comprimento retrógrado cai para $\mathbf{0{,}00\text{ cm}}$.
- **Por que a Simulação Estacionária Subestima o Risco (Item c):**
  - Subestima a área de recirculação em quase $3\times$ ($1{,}89\text{ cm}$ no modelo fixo vs. $5{,}37\text{ cm}$ no regime pulsátil).
  - Oculta os picos sistólicos de WSS máxima (que podem ultrapassar os limites de ruptura tecidual).
  - É cega para a oscilação direcional do cisalhamento (OSI), biomarcador crucial para predizer lesões vasculares vulneráveis.

---

## 3. Verificação e Checklist
- [x] Dados de CFD validados (`dados_dia4.npz`)
- [x] Cálculos hidrodinâmicos e de Bernoulli implementados
- [x] Mapeamento de WSS e análise pulsátil concluídos
- [x] Relatório estruturado gerado
