# 🚀 Guia Completo de Estudos - Quality Assurance

> **Um guia estruturado para dominar QA moderno, do básico ao avançado**

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

**Por que WSL?**

- Ambiente Linux no Windows sem virtualização completa
- Melhor performance para ferramentas de desenvolvimento
- Compatibilidade com scripts e comandos Unix/Linux

**Instalação e Configuração:**

```bash
# Instalar WSL 2
wsl --install

# Instalar Ubuntu (recomendado)
wsl --install -d Ubuntu

# Configurar recursos
# Criar arquivo .wslconfig no diretório do usuário Windows
[wsl2]
memory=4GB
processors=2
swap=2GB
```

**Configuração Inicial:**

```bash
# Atualizar sistema
sudo apt update && sudo apt upgrade -y

# Instalar ferramentas essenciais
sudo apt install curl wget git vim build-essential -y

# Configurar Git
git config --global user.name "Seu Nome"
git config --global user.email "seu.email@exemplo.com"
```

### 🐳 Docker - Containerização

**Conceitos Essenciais:**

- **Container**: Ambiente isolado com aplicação e dependências
- **Image**: Template para criar containers
- **Dockerfile**: Script para criar imagens customizadas

**Instalação:**

```bash
# Ubuntu/WSL
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh

# Adicionar usuário ao grupo docker
sudo usermod -aG docker $USER

# Instalar Docker Compose
sudo apt install docker-compose -y
```

**Docker para QA - Casos de Uso:**

```dockerfile
# Dockerfile para ambiente de teste
FROM node:18-alpine

WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

COPY . .
EXPOSE 3000

CMD ["npm", "start"]
```

```yaml
# docker-compose.yml para testes
version: '3.8'
services:
  app:
    build: .
    ports:
      - '3000:3000'
    environment:
      - NODE_ENV=test
    depends_on:
      - db
      - redis

  db:
    image: postgres:13
    environment:
      POSTGRES_DB: testdb
      POSTGRES_USER: test
      POSTGRES_PASSWORD: test
    ports:
      - '5432:5432'

  redis:
    image: redis:6-alpine
    ports:
      - '6379:6379'
```

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
