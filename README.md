# BeWear E-Commerce

Este projeto foi desenvolvido durante o **Bootcamp E-Commerce** do [Full Stack Club](https://www.fullstackclub.com.br/), um curso completo para desenvolvimento de aplicações e-commerce modernas.

> **Nota:** O projeto está atualmente em fase de desenvolvimento.

## 🚀 Tecnologias Utilizadas

- **Next.js 15** - Framework React para produção
- **TypeScript** - Tipagem estática para JavaScript
- **Tailwind CSS** - Framework CSS utilitário (Mobile-First)
- **Drizzle ORM** - ORM TypeScript-first para PostgreSQL
- **PostgreSQL** - Banco de dados relacional (Neon)
- **Better Auth** - Sistema de autenticação moderno e seguro
- **React Hook Form** - Biblioteca para gerenciamento de formulários
- **Zod** - Validação de esquemas TypeScript-first
- **React Query** - Gerenciamento de estado do servidor
- **React Number Format** - Formatação de números e valores monetários
- **Shadcn/ui** - Componentes UI reutilizáveis
- **ESLint + Prettier** - Linting e formatação de código

## 📋 Pré-requisitos

Antes de começar, certifique-se de ter instalado:

- **Node.js** (versão 18 ou superior)
- **npm** ou **yarn**
- **Git**

## ⚙️ Configuração do Ambiente

### 1. Clone o repositório
```bash
git clone <url-do-repositorio>
cd bewear-bootcamp
```

### 2. Instale as dependências
```bash
npm install
```

### 3. Configure as variáveis de ambiente
Crie um arquivo `.env` na raiz do projeto baseado no `.env.example`:

```bash
cp .env.example .env
```

Preencha as variáveis necessárias no arquivo `.env`:

```env
# Database
DATABASE_URL="postgresql://username:password@host:port/database"

# Next.js
NEXT_PUBLIC_APP_URL="http://localhost:3000"

# Better Auth
BETTER_AUTH_SECRET="your-secret-key-here"
BETTER_AUTH_URL="http://localhost:3000"

# Google OAuth
GOOGLE_CLIENT_ID="your-google-client-id"
GOOGLE_CLIENT_SECRET="your-google-client-secret"

# Stripe - Pagamentos
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY="pk_test_..."
STRIPE_SECRET_KEY="sk_test_..."
STRIPE_WEBHOOK_SECRET="whsec_..."
```

### 4. Configure o Google OAuth (Opcional)

Para habilitar a autenticação com Google, siga estes passos:

1. **Acesse o [Google Cloud Console](https://console.cloud.google.com/)**
2. **Crie um novo projeto** ou selecione um existente
3. **Ative a API do Google+** (se necessário)
4. **Vá para "Credenciais"** no menu lateral
5. **Clique em "Criar credenciais" > "ID do cliente OAuth 2.0"**
6. **Configure a tela de consentimento OAuth** (se solicitado):
   - Preencha as informações básicas do projeto
   - Adicione os escopos necessários
7. **Crie o cliente OAuth** com as seguintes configurações:
   - **Tipo de aplicativo**: Aplicativo da Web
   - **Origens JavaScript autorizadas**: `http://localhost:3000`
   - **URIs de redirecionamento autorizados**: `http://localhost:3000/api/auth/callback/google`
8. **Copie o Client ID e Client Secret** para o arquivo `.env`

### 5. Configure o Stripe (Pagamentos)

Para habilitar pagamentos com Stripe:

1. **Acesse o [Stripe Dashboard](https://dashboard.stripe.com/)**
2. **Crie uma conta** ou faça login
3. **Vá para "Developers" > "API keys"**
4. **Copie as chaves de teste**:
   - **Publishable key** (pk_test_...)
   - **Secret key** (sk_test_...)
5. **Configure as variáveis no arquivo `.env`**

#### Configurar Webhooks do Stripe (OBRIGATÓRIO para testes)

⚠️ **IMPORTANTE**: Para que os pagamentos funcionem corretamente e o status dos pedidos seja atualizado de "Pagamento Pendente" para "Pago", é **obrigatório** ter o Stripe CLI em execução durante os testes.

1. **Instale o Stripe CLI**:
   ```bash
   # Windows (via winget)
   winget install Stripe.StripeCli
   ```

2. **Faça login no Stripe**:
   ```bash
   stripe login
   ```

3. **Escute webhooks localmente** (mantenha este comando em execução):
   ```bash
   stripe listen --forward-to localhost:3000/api/stripe/webhook
   ```

4. **Copie o webhook secret** exibido e adicione ao `.env`:
   ```env
   STRIPE_WEBHOOK_SECRET="whsec_..."
   ```

#### Testando Pagamentos

📝 **Antes de testar, certifique-se de que:**
- O Stripe CLI está em execução (`stripe listen --forward-to localhost:3000/api/stripe/webhook`)
- O servidor de desenvolvimento está rodando (`npm run dev`)
- As variáveis de ambiente do Stripe estão configuradas

Use estes dados de teste para simular pagamentos:

- **Cartão de Crédito**: `4242 4242 4242 4242`
- **Data de Validade**: Qualquer data futura (ex: 12/34)
- **CVC**: Qualquer 3 dígitos (ex: 123)
- **CEP**: Qualquer CEP válido

✅ **Após o pagamento bem-sucedido**, o webhook do Stripe atualizará automaticamente o status do pedido para "Pago".

### 6. Configure o banco de dados

#### Executar migrações:
```bash
npx drizzle-kit push
```

#### Popular o banco com dados de exemplo:
```bash
npm run seed
```

### 7. Execute o projeto
```bash
npm run dev
```

O projeto estará disponível em [http://localhost:3000](http://localhost:3000).

## 📚 Documentação Adicional

Para informações detalhadas sobre instalação e configuração das tecnologias utilizadas, consulte:

📖 **[Guia de Instalação e Configuração](./SETUP.md)**

## 📱 Abordagem Mobile-First

Este projeto foi desenvolvido seguindo a metodologia **Mobile-First**, garantindo uma experiência otimizada em dispositivos móveis:

- **Design Responsivo**: Layouts que se adaptam perfeitamente a diferentes tamanhos de tela
- **Performance Otimizada**: Carregamento rápido em conexões móveis
- **UX Mobile**: Interface pensada primeiro para dispositivos móveis
- **Breakpoints Tailwind**: Utilização dos breakpoints do Tailwind CSS (sm, md, lg, xl, 2xl)
- **Touch-Friendly**: Elementos interativos otimizados para toque

## 🎯 Funcionalidades Atuais

- ✅ Configuração inicial do projeto
- ✅ Setup do banco de dados com Drizzle ORM
- ✅ Schema de produtos, categorias e variantes
- ✅ Sistema de seeding para dados de exemplo
- ✅ Autenticação com Better Auth e Google OAuth
- ✅ Formulários com React Hook Form e validação Zod
- ✅ Gerenciamento de estado do servidor com React Query
- ✅ Design responsivo com abordagem Mobile-First
- ✅ Formatação de valores monetários com React Number Format
- ✅ Configuração de ESLint e Prettier
- ✅ Componentes UI com Shadcn

## 📝 Scripts Disponíveis

```bash
npm run dev          # Inicia o servidor de desenvolvimento
npm run build        # Gera build de produção
npm run start        # Inicia servidor de produção
npm run lint         # Executa o linter
npm run seed         # Popula o banco com dados de exemplo
```

---

**Desenvolvido com ❤️ durante o Bootcamp E-Commerce do [Full Stack Club](https://www.fullstackclub.com.br/)**
