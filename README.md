# 🚀 Guia Completo de Estudos - Quality Assurance

> **Um roadmap estratégico para dominar QA moderno - Do fundamento ao expertise**

---

## 📋 Índice

1. [📚 Preparação do Ambiente](#-preparação-do-ambiente)
2. [🎯 Fundamentos de Testes](#-fundamentos-de-testes)
3. [⚡ Jest - Framework de Testes](#-jest---framework-de-testes)
4. [✨ Qualidade de Código](#-qualidade-de-código)
5. [📦 Gerenciamento de Dependências](#-gerenciamento-de-dependências)
6. [🔍 SonarQube - Análise Estática](#-sonarqube---análise-estática)
7. [🧪 Testes Funcionais com Azure DevOps](#-testes-funcionais-com-azure-devops)
8. [🚀 CI/CD com Foco em Qualidade](#-cicd-com-foco-em-qualidade)
9. [📊 Métricas e Monitoramento](#-métricas-e-monitoramento)
10. [📚 Recursos para Estudo](#-recursos-para-estudo)

---

## 📚 Preparação do Ambiente

### 🐧 WSL (Windows Subsystem for Linux)

**Por que dominar WSL é crucial?**

- Performance superior para ferramentas de desenvolvimento
- Compatibilidade nativa com ecosistema DevOps
- Habilita uso de Docker sem overhead de VM

**Desafios para explorar:**

- Configure um ambiente completo de desenvolvimento em WSL2
- Otimize a integração entre Windows e Linux filesystems
- Estabeleça workflows eficientes para desenvolvimento cross-platform

**Comandos fundamentais para começar:**

```bash
# Instalar WSL 2
wsl --install

# Configurar recursos otimizados
# Crie .wslconfig no seu diretório Windows
[wsl2]
memory=4GB
processors=2
```

**🎯 Missões de aprendizado:**

- Integre VS Code com WSL de forma otimizada
- Configure Git para trabalhar seamlessly entre OS
- Implemente backup/restore de configurações WSL

### 🐳 Docker - Containerização

**Por que Docker revoluciona QA?**

- Ambientes isolados e reproduzíveis
- Eliminação do "funciona na minha máquina"
- Escalabilidade para testes distribuídos

**Conceitos-chave a dominar:**

- **Containers vs VMs**: Entenda as diferenças fundamentais
- **Imagens layered**: Como otimizar builds e storage
- **Networking**: Comunicação entre containers
- **Volumes**: Persistência de dados

**Setup inicial:**

```bash
# Ubuntu/WSL
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

**🚀 Desafios progressivos:**

1. **Básico**: Containerize uma aplicação Node.js simples
2. **Intermediário**: Crie um ambiente de teste multi-serviço com docker-compose
3. **Avançado**: Implemente estratégias de cache para otimizar builds
4. **Expert**: Configure health checks e auto-healing containers

---

## 🎯 Fundamentos de Testes

### 🧠 Mindset de Qualidade

**Pergunta reflexiva:** _Como você pode "quebrar" seu próprio código de forma sistemática?_

A verdadeira arte do QA não está apenas em verificar se algo funciona, mas em descobrir **como** e **quando** pode falhar.

### 🔍 Anatomia dos Tipos de Testes

#### 🧩 Testes Unitários

**Filosofia:** "Teste o menor pedaço testável de código de forma isolada"

**Características críticas:**

- **F.I.R.S.T.**: Fast, Independent, Repeatable, Self-validating, Timely
- **Isolamento**: Zero dependências externas
- **Determinismo**: Mesmo input = mesmo output, sempre

**🎯 Desafio conceitual:**
Como você testaria uma função que depende do tempo atual ou números aleatórios? Explore conceitos de **dependency injection** e **test doubles**.

#### 🔗 Testes de Integração

**Filosofia:** "Teste os contratos e interfaces entre componentes"

**Questionamentos estratégicos:**

- Onde estão os pontos de integração mais críticos do seu sistema?
- Como você validaria que diferentes módulos "conversam" corretamente?
- Quais são os failure modes mais prováveis em integrações?

#### 🌐 Testes End-to-End com Cypress

**Filosofia:** "Simule jornadas reais de usuários para validar value streams"

**Principais desafios a enfrentar:**

- **Flakiness**: Como tornar testes E2E confiáveis?
- **Page Object Model**: Quando usar e como estruturar?
- **Data Management**: Como lidar com dados de teste?
- **Cross-browser**: Estratégias para diferentes navegadores

**🚀 Setup mínimo:**

```bash
npm install --save-dev cypress
npx cypress open
```

**🎯 Missões progressivas:**

1. **Iniciante**: Automatize um fluxo de login simples
2. **Intermediário**: Implemente data-driven tests com fixtures
3. **Avançado**: Crie uma suite de testes que executa em pipeline CI/CD
4. **Expert**: Desenvolva estratégias para visual testing e acessibilidade

### 📊 A Estratégia da Pirâmide

```
         🔺 E2E (10%) - Caros, lentos, frágeis
        🔸🔸 Integração (20%) - Moderados, focados
   🔹🔹🔹🔹 Unitários (70%) - Rápidos, baratos, abundantes
```

**🤔 Perguntas para reflexão:**

- Por que a pirâmide invertida (Ice Cream Cone) é um anti-pattern?
- Como balancear velocidade vs. confiança nos testes?
- Quando quebrar as regras da pirâmide faz sentido?

### 🎭 Design Patterns para Testes

#### AAA Pattern (Arrange, Act, Assert)

**Conceito:** Estrutura mental para organizar testes de forma clara e previsível.

#### Given-When-Then (BDD)

**Conceito:** Linguagem ubíqua para conectar testes com requisitos de negócio.

**🚀 Challenge:** Implemente o mesmo teste usando ambos os patterns. Qual funciona melhor para diferentes contextos?

---

## ⚡ Jest - Framework de Testes

### 🧰 Jest como Ferramenta de Produtividade

**Por que Jest domina o ecossistema JavaScript?**

- Zero-config para casos básicos
- Built-in mocking, coverage, e watch mode
- Snapshot testing para regressão
- Parallel execution nativo

### ⚙️ Configuração Estratégica

```javascript
// jest.config.js - Ponto de partida
module.exports = {
  testEnvironment: 'node',
  collectCoverageFrom: ['src/**/*.{js,ts}', '!src/**/*.d.ts'],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80,
    },
  },
};
```

**🎯 Desafios de configuração:**

1. Configure Jest para trabalhar com ES6 modules
2. Implemente different test environments para diferentes tipos de teste
3. Otimize performance para large test suites
4. Configure custom matchers para seu domínio específico

### 🔧 Testes Unitários - Além do Básico

**🤔 Perguntas orientadoras:**

- Como você testa código assíncrono de forma robusta?
- Quando usar mocks vs stubs vs fakes?
- Como garantir que seus testes não sejam frágeis?

**🚀 Desafios progressivos:**

1. **Mocking Mastery**: Implemente diferentes estratégias de mocking
2. **Async Testing**: Domine promises, callbacks, e async/await
3. **Error Scenarios**: Teste caminhos de erro de forma abrangente
4. **Performance**: Crie testes que validem performance além de funcionalidade

### 🔌 Testes de Integração - O Próximo Nível

**Concepts to master:**

- **Test Containers**: Ambientes isolados para cada teste
- **Database Testing**: Strategies para dados de teste
- **API Contract Testing**: Validando interfaces
- **External Dependencies**: Como mockar serviços externos

**🎯 Mission:** Crie uma suite de testes de integração que pode executar tanto localmente quanto em CI/CD sem modificações.

---

## ✨ Qualidade de Código

### 🎨 A Arte da Consistência

**Filosofia:** Código inconsistente é código difícil de manter, testar e evoluir.

### 🔍 ESLint - Seu Pair Programming Automático

**Por que ESLint é essencial?**

- Detecta bugs antes da execução
- Enforça padrões de equipe
- Melhora legibilidade e manutenibilidade

**🚀 Desafios de implementação:**

1. **Custom Rules**: Crie regras específicas para seu contexto
2. **Team Standards**: Estabeleça padrões que fazem sentido para sua equipe
3. **Performance**: Configure para não impactar velocidade de desenvolvimento
4. **Integration**: Integre com seu editor e CI/CD pipeline

### 🎭 Prettier - Formatação sem Debates

**Conceito:** Elimine discussões sobre formatação focando no que importa - lógica.

**🎯 Challenge:** Configure um workflow onde formatação acontece automaticamente sem interromper o fluxo de desenvolvimento.

### 🤖 Automação com Git Hooks

**Filosofia:** "Se pode ser automatizado, deve ser automatizado."

```bash
# Setup básico
npm install --save-dev husky lint-staged
```

**🚀 Missões avançadas:**

1. Configure hooks que rodam apenas testes relacionados aos arquivos modificados
2. Implemente quality gates que bloqueiam commits problemáticos
3. Crie workflows que se adaptam ao contexto (feature branch vs. main)

---

## 📦 Gerenciamento de Dependências

### 🛡️ Security-First Mindset

**Realidade:** Cada dependência é um ponto potencial de vulnerabilidade.

**Pergunta estratégica:** Como balancear produtividade (usar libraries) com segurança (reduzir attack surface)?

### 🔍 Supply Chain Security

**Conceitos críticos:**

- **CVE (Common Vulnerabilities and Exposures)**
- **SBOM (Software Bill of Materials)**
- **Dependency Confusion Attacks**
- **Typosquatting**

### 🛠️ Ferramentas Comparativas

| Tool            | Strength              | Use Case       | Learning Curve |
| --------------- | --------------------- | -------------- | -------------- |
| npm audit       | Built-in, fast        | Basic scanning | Low            |
| Snyk            | Comprehensive, fixes  | Enterprise     | Medium         |
| OWASP Dep-Check | Open source, thorough | Compliance     | High           |

**🎯 Challenge:** Implemente uma estratégia de security scanning que funcione em diferentes estágios do desenvolvimento (dev, CI, prod).

### 🤖 Automated Dependency Management

**🚀 Exploration topics:**

- Configure Dependabot para updates inteligentes
- Implemente semantic versioning strategy
- Crie policies para aprovação de dependências

---

## 🔍 SonarQube - Análise Estática

### 🧠 Code Intelligence

**Filosofia:** "Código é lido muito mais vezes do que é escrito."

SonarQube não é apenas uma ferramenta - é um **code coach** que te ajuda a escrever código melhor.

### 🏗️ Conceitos Fundamentais

#### 🦨 Code Smells - Sinais de Alerta

- **Long Method**: Quando um método faz demais
- **Large Class**: Classes com muitas responsabilidades
- **Duplicate Code**: DRY (Don't Repeat Yourself) violations
- **Dead Code**: Código que nunca é executado

**🤔 Pergunta reflexiva:** Por que code smells são "smells" e não "errors"? Qual o impacto a longo prazo?

#### 🐛 Reliability Issues

**Conceito:** Bugs que podem causar comportamento inesperado ou crashes.

#### 🔒 Security Vulnerabilities

**Mindset:** Pense como um atacante - onde estão os pontos fracos?

### 🎯 Quality Gates Strategy

**Pergunta estratégica:** Como definir quality gates que sejam rigorosos mas não bloqueiem produtividade?

**🚀 Challenge:** Configure quality gates que evoluam com a maturidade do projeto.

### ⚙️ Setup com Docker

```bash
# Quick start
docker run -d --name sonarqube -p 9000:9000 sonarqube:community
```

**🎯 Advanced missions:**

1. Configure PostgreSQL backend para production
2. Implemente custom quality profiles
3. Integre com seu pipeline de CI/CD
4. Configure notification strategies

---

## 🧪 Testes Funcionais com Azure DevOps

### 📋 Test Management Philosophy

**Conceito:** Testes funcionais são a ponte entre requisitos de negócio e implementação técnica.

### 🔗 Traceability Matrix

**Por que rastreabilidade importa?**

- Impact analysis quando requirements mudam
- Coverage analysis para garantir completeness
- Audit trails para compliance

**🎯 Challenge:** Implemente uma estratégia de rastreabilidade completa do requirement ao test execution.

### 📊 Test Execution Strategy

**Perguntas orientadoras:**

- Como priorizar testes quando tempo é limitado?
- Quando automatizar vs. manter manual?
- Como lidar com test data management?

**🚀 Advanced topics:**

- Exploratory testing sessions
- Risk-based testing approach
- Session-based test management

---

## 🚀 CI/CD com Foco em Qualidade

### 🏗️ Pipeline Philosophy

**Conceito central:** Cada commit deve ser potencialmente deployable.

### 🚦 Quality Gates Strategy

**Shift-Left Thinking:** Encontre problemas o mais cedo possível no processo.

**🤔 Design questions:**

- Quais checks devem bloquear pipeline vs. apenas alertar?
- Como balancear velocidade de feedback com thoroughness?
- Como implementar gradual rollouts com quality monitoring?

### 🔄 Deployment Strategies

**Explore estas abordagens:**

- **Blue-Green**: Zero downtime deployments
- **Canary**: Gradual rollout with monitoring
- **Feature Flags**: Decouple deployment from release

**🎯 Challenge:** Implemente uma estratégia de deployment que permita rollback automático baseado em quality metrics.

---

## 📊 Métricas e Monitoramento

### 📈 Quality Metrics That Matter

**Filosofia:** "You can't improve what you don't measure."

### 🎯 Leading vs. Lagging Indicators

**Leading (Predictive):**

- Code coverage trends
- Complexity metrics
- Test execution time

**Lagging (Outcome):**

- Defect density
- Customer reported issues
- Time to resolution

### 🎛️ Dashboard Strategy

**🚀 Challenge:** Crie dashboards que conte uma história sobre a qualidade do seu software, não apenas números isolados.

**🤔 Questions to explore:**

- Como diferentes stakeholders precisam de diferentes views dos mesmos dados?
- Qual a frequência ideal para different tipos de métricas?
- Como evitar "gaming" de métricas?

### 🔄 Continuous Improvement

**PDCA Cycle para Quality:**

- **Plan**: Define quality goals
- **Do**: Implement practices
- **Check**: Measure outcomes
- **Act**: Adjust based on learnings

---

## 📚 Recursos para Estudo

### 🎓 Learning Path Strategy

**Mindset:** Torne-se um **T-shaped** professional - broad knowledge com deep expertise em áreas específicas.

### 📖 Essential Reading

**Books que mudam perspectiva:**

1. **"The Art of Software Testing"** - Glenford Myers
   - _Por que ler:_ Fundamentals que nunca ficam obsoletos
2. **"Clean Code"** - Robert Martin
   - _Key insight:_ Code quality é sobre comunicação, não só funcionalidade
3. **"Continuous Delivery"** - Jez Humble
   - _Game changer:_ Como deployments frequentes melhoram qualidade

### 🌐 Community & Practice

**🚀 Active learning strategies:**

- Contribute para open source projects
- Participe de code reviews
- Attend testing conferences (virtual/presencial)
- Start a testing blog ou vlog

### 🏆 Certification Path

**Strategic approach:**

- **ISTQB Foundation**: Vocabulary e concepts universais
- **Tool-specific certs**: Deep dive em ferramentas que você usa
- **Cloud platforms**: Azure, AWS DevOps certifications

---

## 🎯 Plano de Estudos (8 Semanas)

### 📅 Learning Sprints

#### **Sprint 1-2: Foundation + Environment**

**🎯 Goal:** Master your development environment and understand testing fundamentals.

**Key challenges:**

- Set up a complete WSL + Docker development environment
- Implement your first test pyramid in a real project
- Question: How do different testing strategies affect development speed?

#### **Sprint 3-4: Testing Mastery**

**🎯 Goal:** Become proficient with Jest and establish quality practices.

**Key challenges:**

- Build a comprehensive test suite for a complex application
- Implement advanced mocking strategies
- Challenge: How do you test the untestable?

#### **Sprint 5-6: Quality & Security**

**🎯 Goal:** Integrate quality gates and security practices.

**Key challenges:**

- Set up SonarQube with custom quality profiles
- Implement automated security scanning
- Exploration: How do quality practices scale with team size?

#### **Sprint 7-8: CI/CD & Monitoring**

**🎯 Goal:** Create a production-ready quality pipeline.

**Key challenges:**

- Build end-to-end CI/CD with quality gates
- Implement comprehensive monitoring
- Capstone: How do you measure the ROI of your quality practices?

### ✅ Competency Framework

#### **Novice** ⭐

- [ ] Can write basic unit tests
- [ ] Understands test pyramid concept
- [ ] Knows how to use basic ESLint rules

#### **Practitioner** ⭐⭐

- [ ] Implements effective mocking strategies
- [ ] Sets up quality pipelines
- [ ] Can design test strategies for complex systems

#### **Expert** ⭐⭐⭐

- [ ] Architects comprehensive quality strategies
- [ ] Influences team quality practices
- [ ] Can balance quality, speed, and cost effectively

### 🚀 Capstone Projects

**Project ideas que desafiam:**

1. **Quality Dashboard**: Crie métricas que realmente impactam decisões
2. **Testing Framework**: Build custom testing utilities para seu domínio
3. **Quality Culture**: Implemente práticas que transformam como sua equipe pensa sobre qualidade

---

## 🎯 Próximos Passos

### 🧠 Mindset Shifts to Embrace

1. **From Bug Finder to Quality Enabler**: Como você pode ajudar todo o time a construir qualidade?
2. **From Manual to Strategic**: Como automação libera você para trabalho mais strategic?
3. **From Reactive to Predictive**: Como usar dados para prever e prevenir problemas?

### 🚀 Your Quality Journey

**Remember:** Quality não é um destino, é uma jornada contínua de aprendizado e melhoria.

**🎯 Final Challenge:** Ao final deste guia, você deve ser capaz de responder:

- Como suas práticas de qualidade impactam o business?
- Que tipo de problemas você pode prever e prevenir?
- Como você influencia outros a pensar sobre qualidade?

---

> **💡 Philosophy**: Este guia não te dá respostas prontas - te dá as ferramentas e perspectivas para encontrar suas próprias soluções. O verdadeiro aprendizado acontece quando você questiona, experimenta e falha forward.

**🎯 Meta**: Torne-se não apenas um testador, mas um **Quality Engineer** que entende o impacto estratégico da qualidade no sucesso do produto e da empresa.

---

_Construído para desafiar mentes curiosas_ 🧠✨

---

## 🎯 Fundamentos de Testes

### 🔍 Tipos de Testes

#### 🧩 Testes Unitários

- **Objetivo**: Testar funções/componentes isoladamente
- **Características**: Rápidos, determinísticos, focados
- **Quando usar**: Lógica de negócio, funções puras, validações

```javascript
// Exemplo: Testando função pura
function calculateDiscount(price, percentage) {
  if (price <= 0 || percentage < 0 || percentage > 100) {
    throw new Error('Parâmetros inválidos');
  }
  return price * (percentage / 100);
}

test('should calculate discount correctly', () => {
  expect(calculateDiscount(100, 10)).toBe(10);
  expect(calculateDiscount(50, 20)).toBe(10);
});
```

#### 🔗 Testes de Integração

- **Objetivo**: Testar interação entre componentes
- **Foco**: APIs, banco de dados, serviços externos
- **Quando usar**: Fluxos de dados, contratos entre módulos

#### 🌐 Testes End-to-End (E2E) com Cypress

- **Objetivo**: Testar jornadas completas do usuário
- **Foco**: Interface, cenários críticos de negócio
- **Quando usar**: Fluxos principais, validação final

```javascript
// cypress/e2e/login.cy.js
describe('Login Flow', () => {
  beforeEach(() => {
    cy.visit('/login');
  });

  it('should login successfully with valid credentials', () => {
    cy.get('[data-cy=email-input]').type('user@example.com');
    cy.get('[data-cy=password-input]').type('password123');
    cy.get('[data-cy=login-button]').click();

    cy.url().should('include', '/dashboard');
    cy.get('[data-cy=welcome-message]').should('contain', 'Welcome');
  });

  it('should show error with invalid credentials', () => {
    cy.get('[data-cy=email-input]').type('invalid@example.com');
    cy.get('[data-cy=password-input]').type('wrongpassword');
    cy.get('[data-cy=login-button]').click();

    cy.get('[data-cy=error-message]').should('be.visible').and('contain', 'Invalid credentials');
  });
});
```

### 📊 Pirâmide de Testes

```
       🔺 E2E (10%)
      🔸🔸 Integração (20%)
   🔹🔹🔹🔹 Unitários (70%)
```

**Princípios:**

- **Base ampla**: Muitos testes unitários (rápidos, baratos)
- **Meio equilibrado**: Testes de integração moderados
- **Topo focado**: Poucos testes E2E (cenários críticos)

### 🎭 Padrões de Estruturação

#### Padrão AAA (Arrange, Act, Assert)

```javascript
describe('UserService', () => {
  test('should create user successfully', () => {
    // Arrange - Preparação
    const userData = { name: 'João', email: 'joao@test.com' };
    const userService = new UserService();

    // Act - Ação
    const result = userService.createUser(userData);

    // Assert - Verificação
    expect(result.success).toBe(true);
    expect(result.user.name).toBe('João');
  });
});
```

#### Padrão Given-When-Then (BDD)

```javascript
describe('Login Feature', () => {
  test('should authenticate user with valid credentials', () => {
    // Given - Estado inicial
    const validUser = { email: 'user@test.com', password: '123456' };

    // When - Ação executada
    const result = authService.login(validUser);

    // Then - Resultado esperado
    expect(result.isAuthenticated).toBe(true);
    expect(result.token).toBeDefined();
  });
});
```

---

## ⚡ Jest - Framework de Testes

### ⚙️ Configuração Inicial

```bash
# Instalação
npm install --save-dev jest @types/jest

# Configuração básica
npm install --save-dev @babel/preset-env @babel/preset-typescript
```

```javascript
// jest.config.js
module.exports = {
  testEnvironment: 'node',
  collectCoverageFrom: ['src/**/*.{js,ts}', '!src/**/*.d.ts', '!src/index.js'],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80,
    },
  },
  setupFilesAfterEnv: ['<rootDir>/src/setupTests.js'],
};
```

### 🔧 Testes Unitários Essenciais

#### Matchers Fundamentais

```javascript
// Igualdade
expect(2 + 2).toBe(4); // Igualdade exata
expect({ name: 'João' }).toEqual({ name: 'João' }); // Igualdade de objeto

// Truthiness
expect(true).toBeTruthy();
expect(false).toBeFalsy();
expect(null).toBeNull();
expect(undefined).toBeUndefined();

// Numbers
expect(2 + 2).toBeGreaterThan(3);
expect(Math.PI).toBeCloseTo(3.14159, 5);

// Strings
expect('Hello World').toMatch(/World/);
expect('João Silva').toContain('Silva');

// Arrays
expect(['a', 'b', 'c']).toContain('b');
expect(['a', 'b']).toHaveLength(2);
```

#### Mocking Estratégico

```javascript
// Mock de função
const mockCallback = jest.fn();
mockCallback('arg1', 'arg2');

expect(mockCallback).toHaveBeenCalledWith('arg1', 'arg2');
expect(mockCallback).toHaveBeenCalledTimes(1);

// Mock de módulo
jest.mock('./userService');
const UserService = require('./userService');
const mockUserService = UserService as jest.Mocked<typeof UserService>;

// Mock de implementação
mockUserService.getUser.mockResolvedValue({
  id: 1,
  name: 'João'
});
```

#### Testes Assíncronos

```javascript
// Async/Await
test('should fetch user data', async () => {
  const userData = await fetchUser(1);
  expect(userData.name).toBe('João');
});

// Promises
test('should resolve user promise', () => {
  return expect(fetchUser(1)).resolves.toMatchObject({
    name: 'João',
  });
});

// Callbacks
test('should call callback', (done) => {
  getUserAsync(1, (user) => {
    expect(user.name).toBe('João');
    done();
  });
});
```

### 🔌 Testes de Integração

```javascript
// Exemplo com Supertest para APIs
const request = require('supertest');
const app = require('../app');

describe('User API', () => {
  test('POST /users should create user', async () => {
    const userData = {
      name: 'João',
      email: 'joao@test.com',
    };

    const response = await request(app).post('/users').send(userData).expect(201);

    expect(response.body.name).toBe('João');
  });
});
```

### 📊 Coverage e Relatórios

```javascript
// package.json scripts
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "test:ci": "jest --ci --coverage --watchAll=false"
  }
}
```

---

## ✨ Qualidade de Código

### 🔍 ESLint - Análise Estática

#### Configuração Robusta

```javascript
// .eslintrc.js
module.exports = {
  root: true,
  env: {
    browser: true,
    node: true,
    es2022: true,
    jest: true,
  },
  extends: ['eslint:recommended', '@typescript-eslint/recommended', 'prettier'],
  parser: '@typescript-eslint/parser',
  plugins: ['@typescript-eslint', 'security', 'jest'],
  rules: {
    'no-console': 'warn',
    'no-unused-vars': 'error',
    'prefer-const': 'error',
    'security/detect-object-injection': 'error',
  },
  overrides: [
    {
      files: ['**/*.test.js', '**/*.spec.js'],
      rules: {
        'no-console': 'off',
      },
    },
  ],
};
```

### 🎨 Prettier - Formatação Consistente

```json
// .prettierrc
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false,
  "bracketSpacing": true,
  "arrowParens": "avoid"
}
```

### 🎣 Automação com Git Hooks

```bash
# Instalação
npm install --save-dev husky lint-staged

# Configuração
npx husky install
npx husky add .husky/pre-commit "npx lint-staged"
```

```json
// package.json
{
  "lint-staged": {
    "*.{js,ts,tsx}": ["eslint --fix", "prettier --write", "jest --findRelatedTests --passWithNoTests"]
  }
}
```

---

## 📦 Gerenciamento de Dependências

### 🛠️ NPM vs Yarn vs PNPM

| Ferramenta | Velocidade   | Espaço em Disco | Segurança | Monorepos |
| ---------- | ------------ | --------------- | --------- | --------- |
| NPM        | Moderada     | Alto            | Boa       | Limitado  |
| Yarn       | Rápida       | Moderado        | Boa       | Excelente |
| PNPM       | Muito Rápida | Baixo           | Excelente | Excelente |

### 🛡️ Segurança de Dependências

#### Auditoria Regular

```bash
# NPM
npm audit
npm audit fix

# Yarn
yarn audit
yarn audit --level high

# PNPM
pnpm audit
pnpm audit --fix
```

#### Ferramentas Avançadas

```bash
# Snyk - Continuous monitoring
npm install -g snyk
snyk test
snyk monitor

# OWASP Dependency Check
dependency-check --project "Meu Projeto" --scan ./
```

#### Automatização com Dependabot

```yaml
# .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: 'npm'
    directory: '/'
    schedule:
      interval: 'weekly'
    open-pull-requests-limit: 5
    reviewers:
      - 'your-username'
    assignees:
      - 'your-username'
```

---

## 🔍 SonarQube - Análise Estática

### 🏗️ Conceitos Fundamentais

#### 🦨 Code Smells

- **Long Method**: Métodos muito extensos
- **Large Class**: Classes com muitas responsabilidades
- **Duplicate Code**: Código duplicado
- **Dead Code**: Código não utilizado

#### 🐛 Bugs e Reliability

- **Null Pointer Exceptions**: Referências nulas
- **Resource Leaks**: Vazamentos de memória/conexões
- **Logic Errors**: Erros de lógica

#### 🔒 Vulnerabilidades de Segurança

- **Injection Flaws**: SQL, XSS, Command Injection
- **Broken Authentication**: Falhas de autenticação
- **Sensitive Data Exposure**: Exposição de dados sensíveis

### ⚙️ Configuração Docker

```yaml
# docker-compose.yml
version: '3.8'
services:
  sonarqube:
    image: sonarqube:community
    ports:
      - '9000:9000'
    environment:
      SONAR_JDBC_URL: jdbc:postgresql://db:5432/sonar
      SONAR_JDBC_USERNAME: sonar
      SONAR_JDBC_PASSWORD: sonar
    depends_on:
      - db

  db:
    image: postgres:13
    environment:
      POSTGRES_USER: sonar
      POSTGRES_PASSWORD: sonar
      POSTGRES_DB: sonar
```

### 📊 Quality Gates

```javascript
// sonar-project.properties
sonar.projectKey=my-project
sonar.projectName=My Project
sonar.projectVersion=1.0

sonar.sources=src
sonar.tests=src
sonar.test.inclusions=**/*.test.js,**/*.spec.js
sonar.exclusions=**/node_modules/**,**/dist/**

sonar.javascript.lcov.reportPaths=coverage/lcov.info
sonar.coverage.exclusions=**/*.test.js,**/*.spec.js
```

---

## 🧪 Testes Funcionais com Azure DevOps

### 📋 Azure Test Plans

#### Estrutura de Test Cases

```
📋 Test Plan: E-commerce Application
├── 📁 Test Suite: User Authentication
│   ├── ✅ TC001: Valid Login
│   ├── ❌ TC002: Invalid Credentials
│   └── ⚠️ TC003: Password Reset
├── 📁 Test Suite: Shopping Cart
│   ├── ✅ TC004: Add Product
│   ├── ✅ TC005: Remove Product
│   └── ❌ TC006: Checkout Process
```

#### Best Practices

- **Casos independentes**: Cada teste deve poder ser executado isoladamente
- **Dados parametrizados**: Use variáveis para dados de teste
- **Rastreabilidade**: Link com User Stories e Requirements

### 🔗 Integração com Pipelines

```yaml
# azure-pipelines.yml
trigger:
  branches:
    include: [main, develop]

stages:
  - stage: Test
    jobs:
      - job: UnitTests
        steps:
          - task: NodeTool@0
            inputs:
              versionSpec: '18.x'

          - script: npm ci
            displayName: 'Install dependencies'

          - script: npm run test:coverage
            displayName: 'Run unit tests'

          - task: PublishTestResults@2
            inputs:
              testResultsFormat: 'JUnit'
              testResultsFiles: '**/test-results.xml'

          - task: PublishCodeCoverageResults@1
            inputs:
              codeCoverageTool: 'Cobertura'
              summaryFileLocation: '**/coverage/cobertura-coverage.xml'
```

---

## 🚀 CI/CD com Foco em Qualidade

### 🏗️ Pipeline de Qualidade

```yaml
# GitHub Actions
name: Quality Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  quality-gate:
    runs-on: ubuntu-latest

    services:
      postgres:
        image: postgres:13
        env:
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Code Quality Checks
        run: |
          npm run lint
          npm run format:check
          npm run type-check

      - name: Security Audit
        run: npm audit --audit-level=moderate

      - name: Unit Tests
        run: npm run test:coverage

      - name: Integration Tests
        run: npm run test:integration
        env:
          DATABASE_URL: postgresql://postgres:postgres@localhost:5432/testdb

      - name: SonarCloud Scan
        uses: SonarSource/sonarcloud-github-action@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}

      - name: Quality Gate Check
        run: |
          # Verificar se quality gate passou
          if [ "${{ steps.sonarcloud.outcome }}" != "success" ]; then
            echo "Quality gate failed!"
            exit 1
          fi
```

### 🚦 Quality Gates Configuráveis

```javascript
// jest.config.js - Coverage thresholds
module.exports = {
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80,
    },
    './src/utils/': {
      branches: 90,
      functions: 90,
      lines: 90,
      statements: 90,
    },
  },
};
```

---

## 📊 Métricas e Monitoramento

### 📈 KPIs Essenciais

#### Métricas de Código

- **Code Coverage**: ≥ 80%
- **Technical Debt Ratio**: ≤ 5%
- **Cyclomatic Complexity**: ≤ 10 (média)
- **Duplicated Lines**: ≤ 3%

#### Métricas de Qualidade

- **Defect Density**: Bugs por 1000 linhas de código
- **Test Pass Rate**: ≥ 95%
- **Flaky Test Rate**: ≤ 2%
- **Mean Time to Resolution**: Tempo médio para corrigir bugs

#### Métricas de Pipeline

- **Pipeline Success Rate**: ≥ 95%
- **Deployment Frequency**: Daily/Weekly
- **Lead Time**: Tempo do commit ao deploy
- **Mean Time to Recovery**: Tempo para recuperar de falhas

### 🎛️ Dashboard de Qualidade

```javascript
// Exemplo de métricas customizadas
const qualityMetrics = {
  coverage: {
    current: 85,
    target: 80,
    trend: 'up',
  },
  technicalDebt: {
    current: 4.2,
    target: 5.0,
    trend: 'down',
  },
  testPassRate: {
    current: 96.5,
    target: 95.0,
    trend: 'stable',
  },
  vulnerabilities: {
    critical: 0,
    high: 1,
    medium: 3,
    low: 8,
  },
};
```

---

## 📚 Recursos para Estudo

### 🎓 Certificações Recomendadas

#### 🏆 ISTQB (International Software Testing Qualifications Board)

- **Foundation Level (CTFL)**: Base sólida em testes
- **Test Automation Engineer**: Foco em automação
- **Agile Testing**: Metodologias ágeis

#### 🔧 Certificações Técnicas

- **Azure DevOps Engineer Expert (AZ-400)**
- **AWS Certified DevOps Engineer**
- **SonarQube Administrator Certification**

### 📖 Bibliografia Essencial

#### Livros Fundamentais

1. **"Clean Code"** - Robert C. Martin
2. **"The Art of Software Testing"** - Glenford Myers
3. **"Continuous Delivery"** - Jez Humble & Dave Farley
4. **"Testing JavaScript Applications"** - Lucas da Costa

#### 🌐 Recursos Online

- **Kent C. Dodds** - Testing JavaScript (testingjavascript.com)
- **Martin Fowler Blog** - Patterns e práticas (martinfowler.com)
- **OWASP Testing Guide** - Segurança em testes
- **Jest Documentation** - Documentação oficial

### 🛠️ Ferramentas Complementares

#### Testing

- **Cypress**: E2E testing com ótima DX (Developer Experience)
- **Playwright**: Cross-browser automation alternativa
- **Postman/Newman**: API testing e automação
- **Artillery/k6**: Performance testing

#### Quality & Security

- **Snyk**: Vulnerability scanning
- **CodeQL**: Semantic code analysis
- **Lighthouse**: Web performance auditing
- **OWASP ZAP**: Security testing

---

## 🎯 Plano de Estudos (8 Semanas)

### 📅 Cronograma Estruturado

#### **Semana 1-2: Fundamentos + Ambiente**

- ✅ Configurar WSL e Docker
- ✅ Pirâmide de testes e estratégias
- ✅ Padrões AAA e Given-When-Then
- ✅ Primeiros testes com Jest

#### **Semana 3-4: Jest Avançado + Qualidade**

- ✅ Mocking strategies
- ✅ Testes assíncronos
- ✅ ESLint e Prettier setup
- ✅ Coverage configuration

#### **Semana 5-6: Integração + Segurança**

- ✅ API testing com Supertest
- ✅ Docker para ambientes de teste
- ✅ Dependency security scanning
- ✅ SonarQube básico

#### **Semana 7-8: CI/CD + Monitoramento**

- ✅ Pipeline completa no GitHub Actions
- ✅ Azure DevOps integration
- ✅ Quality gates avançados
- ✅ Métricas e dashboards

### ✅ Checklist de Competências

#### **Nível Básico** ⭐

- [ ] Escrever testes unitários efetivos
- [ ] Configurar ESLint e Prettier
- [ ] Usar npm audit para segurança
- [ ] Entender métricas de coverage

#### **Nível Intermediário** ⭐⭐

- [ ] Implementar testes de integração
- [ ] Configurar SonarQube quality gates
- [ ] Criar pipelines de CI/CD básicas
- [ ] Docker para ambientes de teste

#### **Nível Avançado** ⭐⭐⭐

- [ ] Arquitetar estratégia completa de testes
- [ ] Customizar regras de qualidade
- [ ] Implementar security testing
- [ ] Criar dashboards de métricas

---

## 🚀 Próximos Passos

### 🎯 Projetos Práticos Sugeridos

1. **API REST com Qualidade Total**

   - Setup completo de testes (unit, integration, E2E)
   - Pipeline CI/CD com quality gates
   - Security scanning integrado

2. **Frontend React com Testing**

   - Component testing com RTL (React Testing Library)
   - **E2E testing com Cypress** - Fluxos completos de usuário
   - Visual regression testing com Cypress

3. **Microserviços com Observabilidade**
   - Contract testing entre serviços
   - Distributed tracing
   - Quality metrics dashboard

### 📈 Evolução Contínua

- **Mantenha-se atualizado**: Tecnologias evoluem rapidamente
- **Pratique regularmente**: Implemente conceitos em projetos reais
- **Participe da comunidade**: Contribua com open source
- **Compartilhe conhecimento**: Ensine outros para consolidar aprendizado

---

> **💡 Dica Final**: Use este guia como checklist, marcando os tópicos conforme avança. Dedique tempo prático a cada ferramenta antes de prosseguir para a próxima seção.

**🎯 Meta**: Tornar-se um Quality Engineer completo, capaz de implementar estratégias de qualidade que impactem positivamente na confiabilidade, segurança e manutenibilidade de software em escala empresarial.

---

_Feito com ❤️ para a comunidade de desenvolvedores_
