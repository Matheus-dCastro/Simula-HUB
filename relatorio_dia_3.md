# Relatório de Atividades – Dia 3: Da Equação ao Código: Modelos 0D e 2D (LBM)

**Curso:** SimulaHub · Módulo 1 – CFD e Modelagem Hemodinâmica  
**Data da Atividade:** 05/11  
**Notebooks:** `svzerod_estenose.ipynb` e `lbm_2d_estenose.ipynb`  
**Referência:** Slide `Dia_3 - Dia_3.pdf`  
**Status da Execução:** Concluído com sucesso (executado e validado via kernel `.venv`)

---

## 1. Visão Geral dos Conceitos Abordados
No Dia 3, conectou-se a física teórica aos métodos computacionais em diferentes escalas de fidelidade:
- **A Escada de Modelos (0D, 1D, 2D, 3D):**
  - **0D (Modelos de Ordem Reduzida / Análogos Elétricos):** Relação entre pressão e fluxo lumped ($V = IR \leftrightarrow \Delta P = QR$). Cada vaso é modelado por resistência viscosa $R$, indutância inercial $L$ e complacência elástica $C$. A estenose é capturada pelo modelo semiempírico de Young & Tsai ($\Delta P = R Q + S |Q| Q$).
  - **2D (Lattice Boltzmann Method - LBM):** Abordagem mesoscópica baseada na equação de Boltzmann em rede (D2Q9 com colisão BGK e bounce-back nas paredes). Resolve o campo completo de velocidades $(u, v)$, vorticidade $(\omega_z)$, recirculação e tensão de cisalhamento na parede (WSS).
- **Condições de Contorno de Saída:** Modelo de Windkessel de 3 elementos (RCR), acoplando a resistência proximal $R_p$, complacência arterial $C$ e resistência periférica distal $R_d$.

---

## 2. Resolução das Atividades do Modelo 0D (`svzerod_estenose.ipynb`)

### Bifurcação Aorta $\to$ Artérias Ilíacas com RCR
- **Configuração Simétrica Inicial:**
  - Débito aórtico médio: $25{,}0\text{ mL/s}$ ($\sim 1{,}5\text{ L/min}$). Pressão aórtica: $127{,}7 / 73{,}8\text{ mmHg}$.
  - Com saídas idênticas ($R_{\text{saida}} = 10.600\text{ dyn}\cdot\text{s/cm}^5$), o fluxo se reparte perfeitamente: $12{,}50\text{ mL/s}$ para a ilíaca esquerda e $12{,}50\text{ mL/s}$ para a direita ($50\%$ cada).
- **Teste de Saídas Assimétricas ($R_{\text{saida}} = (8000, 13000)$):**
  - Quando a ilíaca esquerda deságua em um leito menos resistente ($8.000$) e a direita em um leito mais resistente ($13.000$), ocorre nítida **redistribuição hemodinâmica**:
  - Ilíaca esquerda: $15{,}47\text{ mL/s}$ ($\mathbf{61{,}9\%}$ do débito aórtico).
  - Ilíaca direita: $9{,}53\text{ mL/s}$ ($\mathbf{38{,}1\%}$ do débito aórtico).
  - Isso demonstra a capacidade preditiva do modelo 0D para avaliar compensações sistêmicas sob variações de resistência periférica.

### Estenose na Ilíaca Esquerda e FFR Clínico ($P_d / P_a$)
Variou-se o grau de redução de área de $0\%$ a $90\%$:
- **0% de estenose:** $S = 0{,}0$, $\Delta P_{\text{méd}} = 0{,}2\text{ mmHg}$, $P_d/P_a = \mathbf{1{,}00}$.
- **50% de estenose:** $S = 3{,}2$, $\Delta P_{\text{méd}} = 0{,}9\text{ mmHg}$, $P_d/P_a = \mathbf{0{,}99}$.
- **70% de estenose:** $S = 17{,}4$, $\Delta P_{\text{méd}} = 3{,}4\text{ mmHg}$, $P_d/P_a = \mathbf{0{,}97}$.
- **80% de estenose:** $S = 51{,}0$, $\Delta P_{\text{méd}} = 8{,}2\text{ mmHg}$, $P_d/P_a = \mathbf{0{,}92}$.
- **90% de estenose:** $S = 258{,}3$, $\Delta P_{\text{méd}} = 26{,}5\text{ mmHg}$, $P_d/P_a = \mathbf{0{,}76}$.
- **Interpretação Clínica:** Em $90\%$ de redução de área ($s_{\text{diâmetro}} \approx 68\%$), a razão $P_d/P_a$ cai para $0{,}76 < 0{,}80$, atingindo o limiar clínico consagrado de **estenose crítica/funcional** (indicação de intervenção hemodinâmica ou revascularização).

---

## 3. Resolução das Atividades do Modelo 2D LBM (`lbm_2d_estenose.ipynb`)

### Campo de Velocidade, Jato e Recirculação
- Empregou-se o esquema D2Q9 com semelhança dinâmica em $Re = 100$ ($U_{\text{méd}} = 4{,}7\text{ cm/s}$, $\Delta x = 133\text{ }\mu\text{m}$, $\Delta t = 0{,}11\text{ ms}$).
- **Estenose de 50%:** Formação de um jato central de alta velocidade na garganta que descola da parede logo após a constrição, gerando dois vórtices simétricos de recirculação com comprimento longitudinal de aproximadamente $1{,}0\text{ H}$.
- **Vorticidade:** Destacou as espessas camadas de cisalhamento livre que se desprendem da garganta.

### Tensão de Cisalhamento na Parede (WSS) e Estenose
- Comparação dos graus de estenose ($30\%$, $50\%$, $65\%$):
  - **30%:** $\Delta P_{\text{est}} = 0{,}0032\text{ mmHg}$, WSS máximo $= 3{,}1\text{ }\tau_0$, sem recirculação detectável ($0{,}00\text{ H}$).
  - **50%:** $\Delta P_{\text{est}} = 0{,}0183\text{ mmHg}$, WSS máximo $= 6{,}3\text{ }\tau_0$, recirculação $= 1{,}00\text{ H}$.
  - **65%:** $\Delta P_{\text{est}} = 0{,}0572\text{ mmHg}$, WSS máximo $= 11{,}7\text{ }\tau_0$, recirculação $= 2{,}37\text{ H}$.

### Discussão dos Exercícios da Seção 7
1. **Comprimento de Recirculação vs. Reynolds:** Ao elevar $Re$ de 50 para 200, a inércia do jato aumenta frente ao amortecimento viscoso, fazendo com que o comprimento da zona de recirculação escale de maneira praticamente linear com o Reynolds ($L_{\text{rec}}/H \propto Re$).
2. **Comparação 2D vs. 0D:** O modelo 2D plano (Poiseuille entre placas, onde $Q \propto H^3$) não replica exatamente os coeficientes empíricos de Young & Tsai (desenvolvidos para vasos tubulares 3D, onde $Q \propto R^4$), além de omitir os efeitos tridimensionais de estiramento de vórtices (*vortex stretching*).
3. **WSS e Aterosclerose:**
   - Na garganta, o pico de WSS (mais de $11\times$ o basal) exerce efeito de limpeza mecânica, inibindo adesão de placas (embora possa predispor à ulceração ou ruptura da placa e trombose).
   - A jusante da estenose, na zona de descolamento e recirculação, o WSS torna-se **baixo, oscilatório e retrógrado**. Esse padrão hemodinâmico é sabidamente ateroprogênico: estagna macromoléculas (LDL), prolonga o tempo de contato e ativa a sinalização inflamatória e disfunção endotelial, favorecendo a progressão distal da ateromatose.

---

## 4. Verificação e Checklist
- [x] Simulações 0D com svZeroD executadas e convergidas
- [x] Simulação de bifurcação e redistribuição assimétrica analisadas
- [x] Simulações 2D LBM executadas com aceleração Numba (30%, 50%, 65% e pulsátil)
- [x] Relatório estruturado gerado
