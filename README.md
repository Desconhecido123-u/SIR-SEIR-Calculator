# 🦠 Calculadora Epidemiológica SIR/SEIR

> Simulação de propagação de doenças infecciosas com modelos matemáticos compartimentais — React + Chart.js + Runge-Kutta 4ª ordem

![Banner](https://img.shields.io/badge/Modelos-SIR%20%7C%20SEIR-blue?style=flat-square)
![React](https://img.shields.io/badge/React-18-61dafb?style=flat-square&logo=react)
![Chart.js](https://img.shields.io/badge/Chart.js-4-ff6384?style=flat-square)
![License](https://img.shields.io/badge/Licença-MIT-green?style=flat-square)

---

## 📋 Descrição

Aplicação web acadêmica para simulação de surtos epidemiológicos utilizando os modelos **SIR** e **SEIR**. O usuário configura os parâmetros populacionais e epidemiológicos e obtém:

- Curvas evolutivas de cada compartimento
- Pico de infectados e dia do pico
- Taxa de ataque final
- Número básico de reprodução (R₀)
- Gráfico de distribuição final

A integração numérica é realizada pelo método **Runge-Kutta de 4ª ordem** (dt = 0,5 dias), garantindo precisão superior ao método de Euler.

---

## 🧮 Modelos Matemáticos

### Modelo SIR

Proposto por Kermack & McKendrick (1927). Divide a população em três compartimentos:

| Símbolo | Significado |
|---------|-------------|
| **S** | Suscetíveis — ainda não infectados |
| **I** | Infectados — ativamente transmitindo |
| **R** | Recuperados — com imunidade permanente |

**Sistema de EDOs:**

```
dS/dt = −β · S · I / N
dI/dt =  β · S · I / N  −  γ · I
dR/dt =  γ · I
```

**Parâmetros:**
- `β` (beta) — taxa de transmissão: contatos infectantes por unidade de tempo
- `γ` (gamma) — taxa de recuperação: inverso da duração média da infecção

**Número básico de reprodução:** R₀ = β / γ

---

### Modelo SEIR

Extensão do SIR que inclui um compartimento de **Expostos (E)**, representando indivíduos em período de incubação — infectados mas ainda não infecciosos.

| Símbolo | Significado |
|---------|-------------|
| **S** | Suscetíveis |
| **E** | Expostos (incubação) |
| **I** | Infectados (infecciosos) |
| **R** | Recuperados |

**Sistema de EDOs:**

```
dS/dt = −β · S · I / N
dE/dt =  β · S · I / N  −  σ · E
dI/dt =  σ · E  −  γ · I
dR/dt =  γ · I
```

**Parâmetro adicional:**
- `σ` (sigma) — taxa de incubação: inverso do período médio de incubação

---

### Método Numérico: Runge-Kutta 4ª Ordem

A integração das EDOs utiliza RK4 com passo `dt = 0,5 dias`:

```
k1 = f(t,  y)
k2 = f(t + dt/2,  y + dt/2 · k1)
k3 = f(t + dt/2,  y + dt/2 · k2)
k4 = f(t + dt,    y + dt   · k3)

y(t+dt) = y(t) + (dt/6) · (k1 + 2k2 + 2k3 + k4)
```

---

## 🛠 Tecnologias

| Camada | Tecnologia |
|--------|-----------|
| UI Framework | React 18 + Vite |
| Gráficos | Chart.js 4 |
| Estilização | CSS Modules + variáveis CSS |
| Integração numérica | Runge-Kutta 4 (implementação própria) |
| Backend (stub) | Node.js + Express |

---

## 🚀 Como Executar

### Pré-requisitos

- Node.js ≥ 18
- npm ≥ 9

### Frontend (desenvolvimento)

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/sir-seir-calculator.git
cd sir-seir-calculator

# Instale as dependências
npm install

# Inicie o servidor de desenvolvimento
npm run dev
```

Acesse: [http://localhost:5173](http://localhost:5173)

### Build de produção

```bash
npm run build
npm run preview
```

### Backend (opcional)

```bash
cd server
npm install express cors
node index.js
```

API disponível em: [http://localhost:3001](http://localhost:3001)

Configure a URL da API criando um arquivo `.env` na raiz:

```env
VITE_API_URL=http://localhost:3001
```

---

## 📁 Estrutura do Projeto

```
sir-seir-calculator/
│
├── index.html                    # Entry point HTML
├── vite.config.js                # Vite configuration
├── package.json
│
├── src/
│   ├── main.jsx                  # React app bootstrap
│   ├── App.jsx                   # Root component + navigation state
│   │
│   ├── screens/                  # Telas da aplicação (multi-step flow)
│   │   ├── LandingScreen.jsx     # Tela inicial hero
│   │   ├── LandingScreen.module.css
│   │   ├── ModelSelectScreen.jsx # Seleção SIR / SEIR
│   │   ├── ModelSelectScreen.module.css
│   │   ├── ParametersScreen.jsx  # Formulário de parâmetros
│   │   ├── ParametersScreen.module.css
│   │   ├── ResultsScreen.jsx     # Resultados + gráficos
│   │   └── ResultsScreen.module.css
│   │
│   ├── components/               # Componentes reutilizáveis
│   │   ├── Button.jsx
│   │   ├── Button.module.css
│   │   ├── StepIndicator.jsx     # Progresso multi-step
│   │   ├── StepIndicator.module.css
│   │   ├── FormField.jsx         # Input com validação
│   │   ├── FormField.module.css
│   │   ├── MetricCard.jsx        # Card de estatística
│   │   ├── MetricCard.module.css
│   │   ├── FormulaBox.jsx        # Exibição de EDOs
│   │   └── FormulaBox.module.css
│   │
│   ├── services/
│   │   ├── solver.js             # Integração Runge-Kutta 4 (SIR/SEIR)
│   │   └── apiService.js         # Stub para integração futura com backend
│   │
│   ├── utils/
│   │   ├── validation.js         # Validação dos parâmetros
│   │   ├── statistics.js         # Cálculo de métricas de resultado
│   │   └── constants.js          # Constantes, defaults, paleta de cores
│   │
│   └── assets/
│       └── styles/
│           └── global.css        # Tokens de design + reset CSS
│
└── server/
    └── index.js                  # Backend Express (stub pronto para uso)
```

---

## ⚙️ Parâmetros da Simulação

| Parâmetro | Símbolo | Descrição | Intervalo típico |
|-----------|---------|-----------|-----------------|
| População total | N | Tamanho da população homogênea | 1 000 – 10 000 000 |
| Infectados iniciais | I₀ | Casos no dia 0 | 1 – N |
| Recuperados iniciais | R₀ | Imunizados pré-existentes | 0 – N |
| Expostos iniciais | E₀ | Em incubação no dia 0 (SEIR) | 0 – N |
| Taxa de transmissão | β | Contatos infectantes por dia | 0.1 – 2.0 |
| Taxa de recuperação | γ | 1 / duração da infecção | 0.05 – 0.5 |
| Taxa de incubação | σ | 1 / período de incubação (SEIR) | 0.1 – 1.0 |
| Dias simulados | — | Horizonte temporal | 10 – 1000 |

### Exemplos de parâmetros por doença

| Doença | Modelo | β | γ | σ | R₀ estimado |
|--------|--------|---|---|---|-------------|
| COVID-19 (2020) | SEIR | 0.25 | 0.07 | 0.19 | ~3.5 |
| Sarampo | SIR | 1.0 | 0.14 | — | ~7.1 |
| Influenza sazonal | SEIR | 0.35 | 0.25 | 0.50 | ~1.4 |
| Ebola | SEIR | 0.20 | 0.10 | 0.13 | ~2.0 |

---

## 📊 Funcionalidades

- ✅ Seleção de modelo SIR ou SEIR
- ✅ Formulário com validação em tempo real
- ✅ Preview de R₀ durante configuração
- ✅ Integração numérica RK4 (dt = 0,5 dias)
- ✅ Gráfico de linha interativo com tooltips
- ✅ Gráfico de barras empilhadas (estado final)
- ✅ Métricas: pico, dia do pico, taxa de ataque, suscetíveis restantes
- ✅ Indicador R₀ com classificação de risco
- ✅ Dark mode automático
- ✅ Design responsivo
- ✅ Stub de API REST para integração futura

---

## 🔌 API REST (Backend Stub)

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `GET` | `/api/health` | Health check |
| `GET` | `/api/presets` | Lista de doenças pré-configuradas |
| `POST` | `/api/simulate` | Executa simulação server-side |

**Exemplo de requisição:**

```bash
curl -X POST http://localhost:3001/api/simulate \
  -H "Content-Type: application/json" \
  -d '{
    "model": "SEIR",
    "params": {
      "N": 100000, "I0": 10, "R0init": 0, "E0": 50,
      "beta": 0.3, "gamma": 0.1, "sigma": 0.2, "days": 180
    }
  }'
```

---

## 📚 Referências

- Kermack, W. O., & McKendrick, A. G. (1927). *A contribution to the mathematical theory of epidemics*. Proceedings of the Royal Society of London.
- Hethcote, H. W. (2000). *The mathematics of infectious diseases*. SIAM Review, 42(4), 599–653.
- Anderson, R. M., & May, R. M. (1991). *Infectious Diseases of Humans*. Oxford University Press.
- Brauer, F., & Castillo-Chavez, C. (2012). *Mathematical Models in Population Biology and Epidemiology*. Springer.

---

## 📄 Licença

MIT License — veja [LICENSE](LICENSE) para detalhes.

---

## 👤 Autor

Desenvolvido com fins acadêmicos e educacionais.
