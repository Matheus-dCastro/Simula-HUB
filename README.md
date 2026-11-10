# Simula-HUB · CFD, Hemodinâmica e Inteligência Artificial

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](#licença)
[![CFD](https://img.shields.io/badge/Physics-CFD%20%7C%20LBM%20%7C%200D-orange.svg)](#visão-geral)
[![AI/ML](https://img.shields.io/badge/AI-Surrogate%20Models%20%7C%20Scikit--Learn-purple.svg)](#módulo-5--do-cfd-aos-dados-e-à-inteligência-artificial)

Repositório dedicado ao estudo aprofundado, modelagem computacional e integração de aprendizado de máquina aplicados à **Biomecânica Cardiovascular e Hemodinâmica**. O projeto abrange desde os fundamentos da mecânica dos fluidos contínuos até simulações mesoscópicas por Lattice Boltzmann (LBM), modelos de parâmetros concentrados (0D - svZeroD) e modelos substitutos (*surrogate models*) baseados em IA.

---

## 🧭 Visão Geral e Estrutura dos Módulos

O programa está organizado em 5 módulos sequenciais ("Dias"), integrando teoria matemática, resolução analítica, simulações numéricas e modelos preditivos:

```
                      A ESCADA DE MODELAGEM HEMODINÂMICA
                      
   Dia 1                  Dia 2                  Dia 3                  Dia 4                  Dia 5
[Estática &]   --->   [Viscosidade &]   --->   [Modelos 0D &]   --->   [CFD & Biomecânica] ---> [Surrogate Models]
[Bernoulli ]          [Navier-Stokes]          [LBM Mesoscópico]       [WSS / Estenose   ]      [Machine Learning]
```

### 🔹 Dia 1 – Da Pressão ao Movimento dos Fluidos
* **Notebook:** `SimulaHub_Dia1_Pressao_e_Escoamento.ipynb` | **Relatório:** `relatorio_dia_1.md`
* **Conceitos:** Estática de fluidos, equação hidrostática ($\Delta P = \rho g \Delta h$), conservação de massa (equação da continuidade) e conservação de energia mecânica (equação de Bernoulli).
* **Aplicações:** Comparativo hemodinâmico humano vs. girafa sob gravidade, débito cardíaco da aorta em repouso vs. exercício (5 a 20 L/min), e a relação não-linear entre redução de diâmetro vascular e perda de área luminal em estenoses arteriais.

### 🔹 Dia 2 – Fluido Real, Viscosidade, Reynolds e Navier–Stokes
* **Notebook:** `SimulaHub_Dia2_Viscosidade_Reynolds_Poiseuille.ipynb` | **Relatório:** `relatorio_dia_2.md`
* **Conceitos:** Viscosidade e Lei de Newton para cisalhamento ($\tau = \eta \frac{dv}{dy}$), regimes de escoamento pelo Número de Reynolds ($Re$), Lei de Hagen-Poiseuille ($Q \propto R^4$).
* **Aplicações:** Comparativo reológico (água, sangue, óleo e glicerina), risco de turbulência pós-estenótica e um **solver numérico 1D em diferenças finitas** para as equações de Navier-Stokes com critério de estabilidade CFL difusivo.

### 🔹 Dia 3 – Modelos de Ordem Reduzida (0D) e Lattice Boltzmann (LBM)
* **Notebooks:** `svzerod_estenose.ipynb` e `lbm_2d_estenose.ipynb` | **Relatório:** `relatorio_dia_3.md`
* **Conceitos:** Modelagem de parâmetros concentrados 0D (analogia elétrica $V = IR \leftrightarrow \Delta P = QR$, elementos Windkessel RCR, perdas por estenose de Young & Tsai) e método mesoscópico de Lattice Boltzmann (D2Q9 com aproximação BGK acelerado via Numba).
* **Aplicações:** Redistribuição assimétrica de fluxo na bifurcação aórtica-ilíaca e simulação 2D de jato estenótico, recirculação e vorticidade.

### 🔹 Dia 4 – CFD Cardiovascular: Do Vaso Normal à Estenose
* **Notebook:** `SimulaHub_Dia4_Interpretacao.ipynb` | **Dataset:** `dados_dia4.npz` | **Relatório:** `relatorio_dia_4.md`
* **Conceitos:** Análise detalhada de campos hemodinâmicos bidimensionais e tridimensionais, descolamento de camada limite, gradientes de pressão reversíveis vs. irreversíveis e Tensão de Cisalhamento Parietal (*Wall Shear Stress* - WSS).
* **Aplicações:** Quantificação de hipercisalhamento erosivo na garganta estenótica, zonas de baixo WSS oscilatório distal propensas à aterogênese, e impactos da pulsatilidade e desaceleração sistólica.

### 🔹 Dia 5 – Do CFD aos Dados e à Inteligência Artificial
* **Notebook:** `SimulaHub_Dia5_Dados_e_IA.ipynb` | **Dataset:** `simulacoes_0d.csv` | **Relatório:** `relatorio_dia_5.md`
* **Conceitos:** Construção de modelos substitutos (*Surrogate Models*) para aproximação de física computacional em tempo real a partir de 600 simulações hemodinâmicas.
* **Aplicações:**
  - Regressão com Random Forest para predição de pressão sistólica aórtica ($R^2 = 0{,}963$).
  - Análise de explicabilidade física via *Permutation Feature Importance*.
  - Teste de limites e quebra de extrapolação de modelos baseados em árvores.
  - Classificação de severidade estenótica com calibração de limiar clínico na Regressão Logística (minimização de falsos negativos críticos).

---

## 📂 Organização do Repositório

```text
Simula-HUB/
├── CHECKLIST.md                           # Acompanhamento detalhado da execução
├── README.md                              # Documentação principal do projeto
├── requirements.txt                       # Dependências diretas do projeto
├── requirements-lock.txt                  # Congelamento exato (pip freeze) do ambiente
│
├── SimulaHub_Dia1_Pressao_e_Escoamento.ipynb
├── SimulaHub_Dia2_Viscosidade_Reynolds_Poiseuille.ipynb
├── svzerod_estenose.ipynb                 # Modelo 0D (svZeroD)
├── lbm_2d_estenose.ipynb                  # Simulação 2D Lattice Boltzmann
├── SimulaHub_Dia4_Interpretacao.ipynb     # Interpretação hemodinâmica e CFD
├── SimulaHub_Dia5_Dados_e_IA.ipynb        # Modelos de Machine Learning e Surrogate
│
├── relatorio_dia_1.md                     # Relatórios técnicos individuais
├── relatorio_dia_2.md
├── relatorio_dia_3.md
├── relatorio_dia_4.md
├── relatorio_dia_5.md
│
├── dados_dia4.npz                         # Campos numéricos de CFD do Dia 4
├── simulacoes_0d.csv                      # Dataset tabulado de 600 rodadas hemodinâmicas
│
└── Dia *.pdf                              # Slides e material de apoio teórico de cada dia
```

---

## 🛠️ Instalação e Configuração

### 1. Clonar o Repositório
```bash
git clone https://github.com/seu-usuario/Simula-HUB.git
cd Simula-HUB
```

### 2. Criar e Ativar o Ambiente Virtual
Recomenda-se utilizar Python 3.10 ou superior:

```bash
# Criar ambiente virtual
python3 -m venv .venv

# Ativar no Linux / macOS
source .venv/bin/activate

# Ativar no Windows (PowerShell)
# .venv\Scripts\Activate.ps1
```

### 3. Instalar as Dependências
Para instalar as versões compatíveis recomendadas:
```bash
pip install -r requirements.txt
```

Caso queira replicar exatamente as versões do ambiente de desenvolvimento original:
```bash
pip install -r requirements-lock.txt
```

---

## 🚀 Como Executar

### Utilizando Jupyter Notebook / Jupyter Lab
Com o ambiente ativado, inicie o servidor:
```bash
jupyter lab
# ou
jupyter notebook
```
Selecione o kernel do `.venv` ao abrir qualquer um dos notebooks `.ipynb`.

### Utilizando VS Code / Cursor
1. Abra a pasta do projeto no VS Code / Cursor.
2. Abra qualquer arquivo `.ipynb`.
3. No canto superior direito do editor, clique em **Select Kernel** e selecione o Python do ambiente virtual `.venv`.

---

## 📊 Principais Tecnologias e Bibliotecas

| Biblioteca | Finalidade no Projeto |
| :--- | :--- |
| **`numpy`** | Operações matriciais, discretização espacial e vetores físicos |
| **`scipy`** | Métodos numéricos avançados e integração |
| **`numba`** | Compilação JIT para acelerar as iterações do Lattice Boltzmann (LBM) |
| **`matplotlib`** | Renderização de gráficos estáticos, perfis de velocidade e animações |
| **`pandas`** | Estruturação e análise dos dados tabulares das simulações |
| **`scikit-learn`** | Regressão Linear, Árvores de Decisão, Random Forest, métricas e ROC |
| **`svzerod`** | Solver de ordem reduzida (0D) para redes cardiovasculares lumped |
| **`ipywidgets`** | Interatividade dinâmica com sliders para parâmetros fisiológicos |

---

## 📄 Licença e Reconhecimentos

Desenvolvido no âmbito do programa **SimulaHub - Módulo 1 (CFD e Modelagem Hemodinâmica)**. Os notebooks e materiais foram estruturados para fins educacionais, científicos e de pesquisa em engenharia biomédica e medicina cardiovascular in silico.
