# BeWear E-Commerce

Este projeto foi desenvolvido durante o **Bootcamp E-Commerce** do [Full Stack Club](https://www.fullstackclub.com.br/), um curso completo para desenvolvimento de aplicações e-commerce modernas.

> **Nota:** O projeto está atualmente na primeira etapa de desenvolvimento.

## 🚀 Tecnologias Utilizadas

- **Next.js 15** - Framework React para produção
- **TypeScript** - Tipagem estática para JavaScript
- **Tailwind CSS** - Framework CSS utilitário
- **Drizzle ORM** - ORM TypeScript-first para PostgreSQL
- **PostgreSQL** - Banco de dados relacional (Neon)
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

Preencha as variáveis necessárias no arquivo `.env`.

### 4. Configure o banco de dados

#### Executar migrações:
```bash
npx drizzle-kit push
```

#### Popular o banco com dados de exemplo:
```bash
npm run seed
```

### 5. Execute o projeto
```bash
npm run dev
```

O projeto estará disponível em [http://localhost:3000](http://localhost:3000).

## 📚 Documentação Adicional

Para informações detalhadas sobre instalação e configuração das tecnologias utilizadas, consulte:

📖 **[Guia de Instalação e Configuração](./SETUP.md)**

## 🗂️ Estrutura do Projeto

```
src/
├── app/              # App Router do Next.js
├── components/       # Componentes reutilizáveis
│   └── ui/          # Componentes UI (Shadcn)
├── db/              # Configuração do banco de dados
│   ├── index.ts     # Conexão com o banco
│   ├── schema.ts    # Schema do Drizzle
│   └── seed.ts      # Script para popular o banco
└── lib/             # Utilitários e configurações
```

## 🎯 Funcionalidades Atuais

- ✅ Configuração inicial do projeto
- ✅ Setup do banco de dados com Drizzle ORM
- ✅ Schema de produtos, categorias e variantes
- ✅ Sistema de seeding para dados de exemplo
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
