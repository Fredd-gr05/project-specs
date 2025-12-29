# Project Specs - Especificacoes de Projetos

Repositorio centralizado com especificacoes tecnicas, roadmaps e documentacao de todos os projetos. Ideias transformadas em specs estruturadas pela equipe Kael, Nexus, Iris, Forge e Orion.

---

## Estrutura de Diretorios

project-specs/
+-- projeto-A/
    +-- product-spec.md         (Especificacao de Produto - Kael)
    +-- roadmap.md              (Roadmap & Priorizacao - Orion)
    +-- architecture.md         (Arquitetura Tecnica - Forge)
    +-- user-stories.md         (User Stories Detalhadas)
    +-- wireframes/             (Wireframes & Fluxos)

+-- projeto-B/
    +-- product-spec.md
    +-- roadmap.md
    +-- architecture.md

+-- _templates/                 (Templates para novos projetos)
    +-- PRODUCT_SPEC_TEMPLATE.md
    +-- ROADMAP_TEMPLATE.md
    +-- ARCHITECTURE_TEMPLATE.md
    +-- USER_STORIES_TEMPLATE.md

---

## Equipe de Desenvolvimento

### Kael - Estrategista de Produto
- Valida ideias e cria Product Specs
- Define MVP, identifica riscos
- Cria documentos estruturados

### Nexus - Prototipador Web (Vanilla JS)
- Cria MVPs simples com HTML5/CSS3/JavaScript puro
- Landing pages, sites estaticos, prototipos basicos
- Hospeda em GitHub Pages

### Iris - Prototipador Vue
- Cria MVPs interativos com Vue.js 3
- Dashboards, formularios complexos, estados dinamicos
- Hospeda em GitHub Pages

### Forge - Desenvolvedor Full-Stack
- Evolui MVPs para producao (web + mobile)
- React + Node.js + Supabase + Deploy VPS
- TypeScript, testes, CI/CD

### Orion - Gestor de Projeto
- Coordena toda equipe
- Cria roadmaps e sprints
- Gerencia prazos e riscos

---

## Como Comecar

1. Nova Ideia
Comeca com Orion
  -> Orion direciona para Kael
  -> Kael cria Product Spec em [projeto-novo/product-spec.md]
  -> Orion escolhe prototipador (Nexus ou Iris)
  -> Criar repo MVP: [projeto-novo-mvp] no GitHub

2. Estrutura de Arquivo
Ao criar um novo projeto, copie a estrutura:
seu-projeto/
+-- product-spec.md
+-- roadmap.md
+-- architecture.md
+-- user-stories.md
+-- wireframes/

3. Ciclo de Vida
Ideia
  -> Product Spec (Kael)
  -> Roadmap (Orion)
  -> MVP Prototipo (Nexus/Iris)
  -> Validacao com Usuarios
  -> Producao Full-Stack (Forge)
  -> Iteracoes (Orion gerencia)

---

## Links Uteis

- ai-prompts (Repositorio de Prompts): https://github.com/Fredd-gr05/ai-prompts
- Supabase (Database): https://supabase.co
- Vercel (Frontend Deploy): https://vercel.com
- Railway (Backend Deploy): https://railway.app

---

## Status Atual

Repositorio criado: 2025-12-29
Versao: 1.0.0
Projetos Ativos: 0

---

Mantido pela equipe de desenvolvimento. Para adicionar um novo projeto, copie a estrutura de _templates/ e renomeie.
