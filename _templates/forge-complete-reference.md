# FORGE - Referencia Completa Full-Stack (Production)

## STACK RECOMENDADO PADRÃO

**FRONTEND WEB:**
- React 18+ ou Next.js 14+
- TypeScript
- Tailwind CSS ou Material-UI
- React Query ou SWR (data fetching)
- Zustand ou Context API (state management)
- Vite (build tool)

**FRONTEND MOBILE:**
- React Native com Expo
- TypeScript
- NativeWind (Tailwind para RN)
- Expo SDK (push, camera, etc)
- React Navigation

**BACKEND:**
- Node.js + Express OU Python FastAPI
- TypeScript
- Prisma ORM (Node) ou SQLAlchemy (Python)
- JWT Auth
- Joi/Pydantic (validation)

**DATABASE:**
- PostgreSQL (Primary)
- Redis (Cache/Sessions)
- Supabase (managed PostgreSQL)

**DEPLOYMENT:**
- Vercel (Next.js frontend)
- Railway/Render (Backend API)
- GitHub Actions (CI/CD)
- VPS próprio (se escalável)

---

## ESTRUTURA DE PASTAS RECOMENDADA

### Frontend (Next.js)
```
src/
├── app/                    # Pages e layouts
│   ├── (auth)/
│   │   ├── login/page.tsx
│   │   └── register/page.tsx
│   ├── dashboard/page.tsx
│   └── layout.tsx
├── components/             # Componentes reutilizaveis
│   ├── ui/                 # Atomicos (Button, Card, etc)
│   ├── forms/              # Form components
│   └── layouts/            # Layout wrappers
├── hooks/                  # Custom React hooks
│   ├── useAuth.ts
│   └── useFetch.ts
├── lib/                    # Utilitarios
│   ├── api.ts             # Axios/fetch config
│   └── utils.ts           # Helper functions
├── styles/                # Global CSS
└── types/                 # TypeScript types
```

### Backend (Node + Express)
```
src/
├── routes/                # API endpoints
│   ├── auth.ts
│   ├── users.ts
│   └── products.ts
├── controllers/           # Business logic
│   ├── authController.ts
│   └── userController.ts
├── services/              # Database queries
│   ├── userService.ts
│   └── authService.ts
├── middleware/            # Express middleware
│   ├── auth.ts
│   └── errorHandler.ts
├── prisma/                # Database
│   └── schema.prisma
├── config/                # Configuracoes
│   └── database.ts
└── types/                 # TypeScript types
```

---

## ENVIRONMENT VARIABLES (.env.local e .env.example)

### FRONTEND
```
# API
NEXT_PUBLIC_API_URL=http://localhost:3001/api  # ou dominio em prod
NEXT_PUBLIC_SUPABASE_URL=https://xxx.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=xxx

# Auth
NEXT_PUBLIC_AUTH_PROVIDER=supabase  # ou custom-jwt

# Analytics
NEXT_PUBLIC_ANALYTICS_ID=xxx
```

### BACKEND
```
# Server
PORT=3001
NODE_ENV=production

# Database
DATABASE_URL=postgresql://user:pass@localhost:5432/dbname
REDIS_URL=redis://localhost:6379

# Auth
JWT_SECRET=seu_secret_muito_longo_aqui
JWT_EXPIRY=24h

# Email
SMTP_HOST=smtp.resend.com
SMTP_PORT=587
SMTP_USER=xxx
SMTP_PASS=xxx
SMTP_FROM=noreply@seudominio.com

# Supabase
SUPABASE_URL=https://xxx.supabase.co
SUPABASE_SERVICE_ROLE_KEY=xxx

# External APIs
STRIPE_SECRET_KEY=sk_live_xxx
STRIPE_PUBLISHABLE_KEY=pk_live_xxx
```

---

## SETUP INICIAL - NODE + EXPRESS + PRISMA

```bash
# 1. Criar projeto
mkdir meu-app-backend && cd meu-app-backend
npm init -y

# 2. Instalar dependencias
npm install express cors dotenv joi bcrypt jsonwebtoken
npm install -D typescript ts-node @types/node @types/express
npm install -D prisma @prisma/client

# 3. Inicializar TypeScript
npx tsc --init

# 4. Inicializar Prisma
npx prisma init

# 5. Criar arquivo .env
echo "DATABASE_URL=postgresql://user:pass@localhost/dbname" > .env
echo "JWT_SECRET=meu_secret_super_seguro" >> .env

# 6. Atualizar package.json scripts
{
  "scripts": {
    "dev": "ts-node src/server.ts",
    "build": "tsc",
    "start": "node dist/server.js",
    "prisma:generate": "prisma generate",
    "prisma:migrate": "prisma migrate dev --name init",
    "prisma:studio": "prisma studio"
  }
}
```

---

## PRISMA SCHEMA BASICO

```prisma
// prisma/schema.prisma

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

model User {
  id    Int     @id @default(autoincrement())
  email String  @unique
  password String
  name  String?
  role  String  @default("user")  // user, admin
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  
  posts Post[]
}

model Post {
  id    Int     @id @default(autoincrement())
  title String
  content String
  published Boolean @default(false)
  author User @relation(fields: [authorId], references: [id])
  authorId Int
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

---

## EXPRESS SERVER BASICO

```typescript
// src/server.ts
import express from 'express';
import cors from 'cors';
import dotenv from 'dotenv';
import { PrismaClient } from '@prisma/client';

dotenv.config();
const app = express();
const prisma = new PrismaClient();

// Middleware
app.use(cors());
app.use(express.json());

// Health check
app.get('/api/health', (req, res) => {
  res.json({ status: 'ok' });
});

// Routes
app.post('/api/auth/register', async (req, res) => {
  try {
    const { email, password, name } = req.body;
    // Validation + hash password + create user
    res.status(201).json({ message: 'User created' });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// Start server
const PORT = process.env.PORT || 3001;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```

---

## GITHUB ACTIONS CI/CD BASICO

```yaml
# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm ci
      - run: npm run build
      - run: npm test
      - name: Deploy to Railway
        env:
          RAILWAY_TOKEN: ${{ secrets.RAILWAY_TOKEN }}
        run: npm run deploy
```

---

## DEPLOY CHECKLIST

### Frontend (Vercel)
- [ ] Conectar repositorio GitHub
- [ ] Configurar variaveis de ambiente (NEXT_PUBLIC_API_URL, etc)
- [ ] Testar build local (`npm run build`)
- [ ] Gerar preview URL

### Backend (Railway/Render)
- [ ] Conectar repositorio GitHub
- [ ] Configurar DATABASE_URL (Supabase)
- [ ] Configurar JWT_SECRET, API keys
- [ ] Executar migrations: `prisma migrate deploy`
- [ ] Testar API com curl/Postman

### Database (Supabase)
- [ ] Criar projeto Supabase
- [ ] Criar banco PostgreSQL
- [ ] Copiar CONNECTION_STRING
- [ ] Executar migrations Prisma
- [ ] Backup automático ativado

### VPS Proprio (se usar)
- [ ] SSH: `ssh user@ip`
- [ ] Install Node 18+: `curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - && sudo apt-get install -y nodejs`
- [ ] Install Nginx: `sudo apt-get install nginx`
- [ ] Clone repo: `git clone xxx && cd xxx`
- [ ] Install deps: `npm ci --production`
- [ ] Build: `npm run build`
- [ ] PM2 start: `pm2 start dist/server.js -i max`
- [ ] Config Nginx proxy para localhost:3001
- [ ] SSL com Let's Encrypt

---

## COMANDOS ESSENCIAIS PRISMA

```bash
prisma init                    # Inicializar novo projeto
prisma generate               # Gerar Prisma Client
prisma migrate dev --name xxx  # Criar migracao local
prisma migrate deploy          # Executar migracoes prod
prisma studio                  # Interface visual do banco
prisma db push                 # Sync schema com banco
prisma db seed                 # Popular banco com dados iniciais
```

---

## AUTHENTICATION JWT

```typescript
import jwt from 'jsonwebtoken';

const secret = process.env.JWT_SECRET || 'default_secret';

// Gerar token
const generateToken = (userId: number) => {
  return jwt.sign({ userId }, secret, { expiresIn: '24h' });
};

// Verificar token (middleware)
const verifyToken = (req, res, next) => {
  const token = req.headers.authorization?.split(' ')[1];
  if (!token) return res.status(401).json({ error: 'No token' });
  try {
    const decoded = jwt.verify(token, secret);
    req.userId = decoded.userId;
    next();
  } catch (e) {
    res.status(401).json({ error: 'Invalid token' });
  }
};
```

---

## ESTRUTURA RESPOSTA API

```typescript
// Sucesso (200)
{
  "success": true,
  "data": { /* seus dados */ },
  "message": "Operacao realizada com sucesso"
}

// Erro (400+)
{
  "success": false,
  "error": "invalid_input",
  "message": "Email ja registrado",
  "details": { /* campo especifico */ }
}
```

---

## DICAS GERAIS

1. **Sempre use TypeScript** em producao
2. **Validate inputs** com Joi/Zod/Pydantic
3. **Hash passwords** com bcrypt
4. **Rate limiting** em endpoints publ icos
5. **CORS** configurado corretamente
6. **Error handling** centralizado
7. **Logging** em prod (Winston, Pino)
8. **Health checks** e monitoring
9. **Backups** automaticos do banco
10. **Tests** unitarios e e2e
