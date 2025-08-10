# 📖 Guia de Instalação e Configuração

Este guia fornece instruções detalhadas para configurar todas as tecnologias utilizadas no projeto BeWear E-Commerce.

**[← Voltar ao README principal](./README.md)**

## 📑 Índice

- [Node Version Manager (NVM)](#node-version-manager-nvm)
- [Shadcn/ui](#shadcnui)
- [Better Auth](#better-auth)
- [React Hook Form + Zod](#react-hook-form--zod)
- [React Query](#react-query)
- [Google OAuth Setup](#google-oauth-setup)
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

## Better Auth

Sistema de autenticação moderno e seguro para Next.js.

### Instalação

```bash
# Dependência principal
npm install better-auth

# Para React (hooks)
npm install better-auth/react
```

### Configuração

**1. Arquivo `src/lib/auth.ts`:**

```typescript
import { betterAuth } from "better-auth";
import { drizzleAdapter } from "better-auth/adapters/drizzle";
import { db } from "@/db";
import { userTable, sessionTable, accountTable, verification } from "@/auth-schema";

export const auth = betterAuth({
  database: drizzleAdapter(db, {
    provider: "pg",
    schema: {
      user: userTable,
      session: sessionTable,
      account: accountTable,
      verification: verification,
    },
  }),
  socialProviders: {
    google: {
      clientId: process.env.GOOGLE_CLIENT_ID!,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET!,
    },
  },
  secret: process.env.BETTER_AUTH_SECRET!,
  baseURL: process.env.BETTER_AUTH_URL!,
});
```

**2. Cliente para React (`src/lib/auth-client.ts`):**

```typescript
import { createAuthClient } from "better-auth/react";

export const authClient = createAuthClient();
```

### Uso nos Componentes

```typescript
import { authClient } from "@/lib/auth-client";

function LoginButton() {
  const { signIn } = authClient.useSignIn();
  
  return (
    <button onClick={() => signIn.social({ provider: "google" })}>
      Login with Google
    </button>
  );
}
```

---

## React Hook Form + Zod

Combinação poderosa para gerenciamento e validação de formulários.

### Instalação

```bash
# React Hook Form
npm install react-hook-form

# Zod para validação
npm install zod

# Integração entre os dois
npm install @hookform/resolvers
```

### Exemplo de Uso

**1. Schema de validação com Zod:**

```typescript
import { z } from "zod";

const loginSchema = z.object({
  email: z.string().email("Email inválido"),
  password: z.string().min(6, "Senha deve ter pelo menos 6 caracteres"),
});

type LoginForm = z.infer<typeof loginSchema>;
```

**2. Formulário com React Hook Form:**

```typescript
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";

function LoginForm() {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm<LoginForm>({
    resolver: zodResolver(loginSchema),
  });

  const onSubmit = (data: LoginForm) => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("email")} type="email" />
      {errors.email && <span>{errors.email.message}</span>}
      
      <input {...register("password")} type="password" />
      {errors.password && <span>{errors.password.message}</span>}
      
      <button type="submit">Login</button>
    </form>
  );
}
```

---

## React Query

Gerenciamento de estado do servidor com cache, sincronização e muito mais.

### Instalação

```bash
# TanStack Query (React Query v5)
npm install @tanstack/react-query

# DevTools (opcional, mas recomendado)
npm install @tanstack/react-query-devtools
```

### Configuração

**1. Provider (`src/providers/react-query.tsx`):**

```typescript
"use client";

import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { ReactQueryDevtools } from "@tanstack/react-query-devtools";
import { useState } from "react";

export function ReactQueryProvider({ children }: { children: React.ReactNode }) {
  const [queryClient] = useState(
    () =>
      new QueryClient({
        defaultOptions: {
          queries: {
            staleTime: 60 * 1000, // 1 minuto
            gcTime: 10 * 60 * 1000, // 10 minutos
          },
        },
      })
  );

  return (
    <QueryClientProvider client={queryClient}>
      {children}
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  );
}
```

**2. Uso no layout principal:**

```typescript
import { ReactQueryProvider } from "@/providers/react-query";

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html>
      <body>
        <ReactQueryProvider>
          {children}
        </ReactQueryProvider>
      </body>
    </html>
  );
}
```

### Exemplo de Uso

```typescript
import { useQuery, useMutation, useQueryClient } from "@tanstack/react-query";

// Hook para buscar produtos
function useProducts() {
  return useQuery({
    queryKey: ["products"],
    queryFn: async () => {
      const response = await fetch("/api/products");
      return response.json();
    },
  });
}

// Hook para criar produto
function useCreateProduct() {
  const queryClient = useQueryClient();
  
  return useMutation({
    mutationFn: async (product: CreateProductData) => {
      const response = await fetch("/api/products", {
        method: "POST",
        body: JSON.stringify(product),
      });
      return response.json();
    },
    onSuccess: () => {
      // Invalidar cache dos produtos
      queryClient.invalidateQueries({ queryKey: ["products"] });
    },
  });
}
```

---

## Google OAuth Setup

Configuração detalhada para autenticação com Google.

### Passo a Passo no Google Cloud Console

**1. Criar/Selecionar Projeto:**
- Acesse [Google Cloud Console](https://console.cloud.google.com/)
- Crie um novo projeto ou selecione um existente
- Anote o ID do projeto

**2. Ativar APIs Necessárias:**
- Vá para "APIs e Serviços" > "Biblioteca"
- Procure e ative:
  - Google+ API (se disponível)
  - Google Identity API

**3. Configurar Tela de Consentimento OAuth:**
- Vá para "APIs e Serviços" > "Tela de consentimento OAuth"
- Escolha "Externo" (para desenvolvimento)
- Preencha as informações obrigatórias:
  - Nome do aplicativo
  - Email de suporte do usuário
  - Domínios autorizados: `localhost`
  - Email de contato do desenvolvedor
- Adicione escopos (opcional para desenvolvimento)
- Adicione usuários de teste (emails que podem fazer login)

**4. Criar Credenciais OAuth:**
- Vá para "APIs e Serviços" > "Credenciais"
- Clique em "Criar credenciais" > "ID do cliente OAuth 2.0"
- Selecione "Aplicativo da Web"
- Configure:
  - **Nome**: Nome descritivo
  - **Origens JavaScript autorizadas**: 
    - `http://localhost:3000`
    - `http://127.0.0.1:3000` (opcional)
  - **URIs de redirecionamento autorizados**:
    - `http://localhost:3000/api/auth/callback/google`

**5. Obter Credenciais:**
- Copie o **Client ID** e **Client Secret**
- Adicione no arquivo `.env`:

```env
GOOGLE_CLIENT_ID="seu-client-id-aqui"
GOOGLE_CLIENT_SECRET="seu-client-secret-aqui"
```

### Configuração para Produção

Quando for para produção, atualize:

**Origens JavaScript autorizadas:**
- `https://seudominio.com`

**URIs de redirecionamento:**
- `https://seudominio.com/api/auth/callback/google`

### Troubleshooting

**Erro "redirect_uri_mismatch":**
- Verifique se a URL de callback está exatamente igual no Google Console
- Certifique-se de que não há barras extras no final

**Erro "invalid_client":**
- Verifique se o Client ID e Secret estão corretos no `.env`
- Confirme se as variáveis de ambiente estão sendo carregadas

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
   - Adicione no arquivo `.env`:

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

**2. Problemas com Better Auth:**
```bash
# Verificar variáveis de ambiente
echo $BETTER_AUTH_SECRET
echo $BETTER_AUTH_URL

# Verificar se as tabelas de auth existem
npx drizzle-kit push
```

**3. Problemas com Google OAuth:**
```bash
# Verificar credenciais
echo $GOOGLE_CLIENT_ID
echo $GOOGLE_CLIENT_SECRET

# Verificar URLs de callback no Google Console
# Deve ser exatamente: http://localhost:3000/api/auth/callback/google
```

**4. Problemas com React Query:**
```bash
# Verificar se o provider está configurado
# O ReactQueryProvider deve envolver toda a aplicação

# Limpar cache do React Query (no código)
# queryClient.clear()
```

**5. Problemas com formulários (React Hook Form + Zod):**
```bash
# Verificar se as dependências estão instaladas
npm list react-hook-form zod @hookform/resolvers

# Verificar se o resolver está configurado corretamente
# resolver: zodResolver(schema)
```

**6. Problemas com ESLint:**
```bash
# Limpar cache do ESLint
npx eslint --cache-location .eslintcache --cache

# Verificar configuração
npx eslint --print-config src/app/page.tsx
```

**7. Problemas com Prettier:**
```bash
# Verificar configuração
npx prettier --check .

# Formatar todos os arquivos
npx prettier --write .
```

---

**[← Voltar ao README principal](./README.md)**