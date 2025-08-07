# 📖 Guia de Instalação e Configuração

Este guia fornece instruções detalhadas para configurar todas as tecnologias utilizadas no projeto BeWear E-Commerce.

**[← Voltar ao README principal](./README.md)**

## 📑 Índice

- [Node Version Manager (NVM)](#node-version-manager-nvm)
- [Shadcn/ui](#shadcnui)
- [Configurações do Prettier/ESLint](#configurações-do-prettiereslint)
- [Plugin de Ordenação Tailwind e Imports](#plugin-de-ordenação-tailwind-e-imports)
- [Settings.json do VS Code](#settingsjson-do-vs-code)
- [Drizzle ORM](#drizzle-orm)
- [Neon Database](#neon-database)

---

## Node Version Manager (NVM)

O NVM permite gerenciar múltiplas versões do Node.js no mesmo sistema.

### Windows

1. **Baixe o NVM para Windows:**
   - Acesse: https://github.com/coreybutler/nvm-windows/releases
   - Baixe o arquivo `nvm-setup.exe`

2. **Instale e configure:**
   ```bash
   # Instalar a versão LTS mais recente do Node.js
   nvm install lts
   
   # Usar a versão instalada
   nvm use lts
   
   # Verificar a versão ativa
   nvm current
   ```

### macOS/Linux

1. **Instale via curl:**
   ```bash
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
   ```

2. **Recarregue o terminal e configure:**
   ```bash
   # Instalar Node.js LTS
   nvm install --lts
   
   # Usar como padrão
   nvm use --lts
   nvm alias default lts/*
   ```

---

## Shadcn/ui

Sistema de componentes baseado em Radix UI e Tailwind CSS.

### Instalação Inicial

```bash
# Inicializar shadcn/ui no projeto
npx shadcn@latest init
```

### Configuração do components.json

O arquivo `components.json` já está configurado no projeto:

```json
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "new-york",
  "rsc": true,
  "tsx": true,
  "tailwind": {
    "config": "tailwind.config.ts",
    "css": "src/app/globals.css",
    "baseColor": "neutral",
    "cssVariables": true,
    "prefix": ""
  },
  "aliases": {
    "components": "@/components",
    "utils": "@/lib/utils"
  }
}
```

### Adicionando Componentes

```bash
# Adicionar componentes específicos
npx shadcn@latest add button
npx shadcn@latest add card
npx shadcn@latest add input

# Adicionar múltiplos componentes
npx shadcn@latest add button card input form
```

### Configuração de Temas

Para personalizar temas, use o gerador oficial:
- Acesse: https://ui.shadcn.com/themes
- Copie as variáveis CSS para `src/app/globals.css`

---

## Configurações do Prettier/ESLint

### Prettier

Configuração no arquivo `.prettierrc.json`:

```json
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

### ESLint

Configuração no arquivo `eslint.config.mjs`:

```javascript
import { FlatCompat } from "@eslint/eslintrc";
import simpleImportSort from "eslint-plugin-simple-import-sort";
import { dirname } from "path";
import { fileURLToPath } from "url";

const __filename = fileURLToPath(import.meta.url);
const __dirname = dirname(__filename);

const compat = new FlatCompat({
  baseDirectory: __dirname,
});

const eslintConfig = [
  ...compat.extends("next/core-web-vitals", "next/typescript"),
  {
    plugins: {
      "simple-import-sort": simpleImportSort,
    },
    rules: {
      "simple-import-sort/imports": "error",
      "simple-import-sort/exports": "error",
    },
  },
];

export default eslintConfig;
```

### Dependências Necessárias

```bash
# Prettier e plugin do Tailwind
npm install -D prettier prettier-plugin-tailwindcss

# ESLint e plugins
npm install -D eslint eslint-config-next eslint-plugin-simple-import-sort
```

---

## Plugin de Ordenação Tailwind e Imports

### Plugin do Tailwind (prettier-plugin-tailwindcss)

**Funcionalidade:** Ordena automaticamente as classes do Tailwind CSS.

**Instalação:**
```bash
npm install -D prettier-plugin-tailwindcss
```

**Configuração:** Já incluído no `.prettierrc.json`

### Plugin de Ordenação de Imports (eslint-plugin-simple-import-sort)

**Funcionalidade:** Ordena e agrupa imports automaticamente.

**Instalação:**
```bash
npm install -D eslint-plugin-simple-import-sort
```

**Regras aplicadas:**
- Imports externos primeiro
- Imports internos depois
- Imports relativos por último
- Ordenação alfabética dentro de cada grupo

---

## Settings.json do VS Code

Configuração no arquivo `.vscode/settings.json`:

```json
{
  "eslint.workingDirectories": [
    {
      "mode": "auto"
    }
  ],
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact"
  ],
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  }
}
```

### Extensões Recomendadas

Crie o arquivo `.vscode/extensions.json`:

```json
{
  "recommendations": [
    "bradlc.vscode-tailwindcss",
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint",
    "ms-vscode.vscode-typescript-next"
  ]
}
```

---

## Drizzle ORM

ORM TypeScript-first para PostgreSQL.

### Instalação

```bash
# Dependências principais
npm install drizzle-orm pg dotenv

# Dependências de desenvolvimento
npm install -D drizzle-kit @types/pg tsx
```

### Configuração

**1. Arquivo `drizzle.config.ts`:**

```typescript
import "dotenv/config";
import { defineConfig } from "drizzle-kit";

export default defineConfig({
  out: "./drizzle",
  schema: "./src/db/schema.ts",
  dialect: "postgresql",
  dbCredentials: {
    url: process.env.DATABASE_URL!,
  },
});
```

**2. Conexão com o banco (`src/db/index.ts`):**

```typescript
import "dotenv/config";
import { drizzle } from "drizzle-orm/node-postgres";
import { Pool } from "pg";

const pool = new Pool({
  connectionString: process.env.DATABASE_URL!,
});

export const db = drizzle(pool);
```

### Comandos Úteis

```bash
# Gerar migrações
npx drizzle-kit generate

# Aplicar migrações
npx drizzle-kit push

# Visualizar banco (Drizzle Studio)
npx drizzle-kit studio

# Popular banco com dados
npm run seed
```

---

## Neon Database

Banco PostgreSQL serverless na nuvem.

### Configuração

1. **Criar conta:**
   - Acesse: https://console.neon.tech
   - Faça login com GitHub, Google ou email

2. **Criar projeto:**
   - Clique em "Create Project"
   - Escolha um nome para o projeto
   - Selecione a região mais próxima
   - Escolha a versão do PostgreSQL (recomendado: mais recente)

3. **Obter string de conexão:**
   - No dashboard do projeto, vá em "Connection Details"
   - Copie a "Connection string"
   - Adicione no arquivo `.env.local`:

   ```bash
   DATABASE_URL="postgresql://username:password@ep-example-123456.us-east-1.aws.neon.tech/neondb?sslmode=require"
   ```

### Recursos do Neon

- **Branching:** Crie branches do banco para desenvolvimento
- **Autoscaling:** Escala automaticamente baseado no uso
- **Connection Pooling:** Pool de conexões integrado
- **Backups:** Backups automáticos e point-in-time recovery

### Comandos de Gerenciamento

```bash
# Conectar via psql (se instalado)
psql "postgresql://username:password@host/database?sslmode=require"

# Executar migrações
npx drizzle-kit push

# Visualizar dados
npx drizzle-kit studio
```

---

## 🔧 Troubleshooting

### Problemas Comuns

**1. Erro de conexão com o banco:**
```bash
# Verificar se a URL está correta
echo $DATABASE_URL

# Testar conexão
npx drizzle-kit studio
```

**2. Problemas com ESLint:**
```bash
# Limpar cache do ESLint
npx eslint --cache-location .eslintcache --cache

# Verificar configuração
npx eslint --print-config src/app/page.tsx
```

**3. Problemas com Prettier:**
```bash
# Verificar configuração
npx prettier --check .

# Formatar todos os arquivos
npx prettier --write .
```

---

**[← Voltar ao README principal](./README.md)**