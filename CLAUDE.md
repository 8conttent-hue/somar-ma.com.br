# CLAUDE.md — SOMAR-MA

Site gerado pelo **SF (Site Factory)** em 15/04/2026. Migrado para o modelo Cloudflare + Supabase em 24/09/2026.

## Contexto do Site

**Nome:** SOMAR-MA
**Nicho:** Negócios e Empreendedorismo
**Keywords:** A Somar A Somar Assessoria e Servicos nasceu a partir da identificacao
**Paleta de cores:** gold | **Fonte:** outfit

A Somar A Somar Assessoria e Serviços nasceu a partir da identificação da necessidade de uma empresa que tenha em seu perfil, além da flexibilidade, três fatores muitos importantes para a Administração Pública e Privada: Seriedade, Compromisso e Ética Profissional. Isso é Legitimo. Contamos com uma equipe constituída por profissionais qualificados, competentes e criativos, os quais, em função da metodologia de trabalho e atendimento personalizado, procuram focar seus esforços nos interesses dos clientes, orientando-os para a melhor solução. Dentre os padrões rigorosamente éticos e legais, a Somar tem o compromisso de trabalhar pelo desenvolvimento de seus clientes buscando independência, competência e credibilidade. Nosso maior objetivo é possibilitar aos clientes o desenvolvimento e aprimoramento operacional de suas empresas e órgãos públicos, com o compromisso sério e efetivo de sempre buscar a excelência através da prestação de serviços de alta qualidade.   Principios Garantia de compromisso com os propósitos de nossos clientes na pontualidade, na qualidade, no aprimoramento e desenvolvimento dos serviços prestados por meio do modelo de gestão e manutenção dos princípios e valores organizacionais.   Nossa Visão Buscar o crescimento contínuo como forma de garantir a manutenção de nossos negócios nos serviços contratados por meio de uma equipe integrada e capacitada, sendo referência nesse segmento e no atendimento às necessidades de nossos clientes. – MEMBROS QUE INTEGRAM O INSTITUTO SOMAR – SOCIO- ADMINISTRADOR – GERENTE GERAL – GERENTE ADMINSTRATIVA – CONTADOR – ADVOGADO – PEDAGOGO – ADMINISTRADORA

## Componentes visuais usados

| Seção | Variante |
|-------|----------|
| Header | Header-A |
| Hero | Hero-C |
| Features | Features-B |
| About Section | About-C |
| Posts | Posts-H |
| Footer | Footer-J |
| Página Sobre | Sobre-H |
| Página Contato | Contato-C |

## Estrutura do projeto

```
src/
  sections/        # Layout escolhido pelo SF — Header, Hero, Features, About, Posts, Footer, Sobre, Contato
  data/            # JSONs com todo o conteúdo editável
  lib/             # supabase.ts (cliente) e posts.ts (getPosts/getPostBySlug)
  components/      # Seo.astro (meta tags + JSON-LD)
  pages/           # Rotas Astro (index, sobre, contato, blog, privacidade, termos, [...slug])
  layouts/         # BaseLayout com fonte e cores dinâmicas
  styles/          # global.css com variáveis CSS de cor
public/
  images/          # hero.jpg, about.jpg, sobre.jpg
```

## O que editar

### Textos e conteúdo
- **`src/data/home.json`** — hero (título, subtítulo, botão), features (título, items), about section (título, desc, stats), posts
- **`src/data/sobre.json`** — conteúdo completo da página Sobre (hero, texto, missão)
- **`src/data/contato.json`** — título, subtítulo, email, tempo de resposta
- **`src/data/siteConfig.json`** — nome, slug, email, redes sociais, menu (título/descrição/OG/JSON-LD derivam daqui)

### Imagens
Imagens já estão em `public/images/` (via Pexels). Para substituir, mantenha os mesmos nomes de arquivo:
- `hero.jpg` — imagem de fundo do Hero (e og:image padrão)
- `about.jpg` — imagem da seção About (home)
- `sobre.jpg` — imagem de fundo da página Sobre

### Posts do blog
Os posts NÃO ficam mais em markdown local. São carregados do Supabase (tabela `network_posts`, filtrados por `domain = somar-ma.com.br`).
- `src/lib/posts.ts` — `getPosts()` e `getPostBySlug()`; `formatContentToHtml()` converte markdown → HTML.
- Sem painel admin. Novos posts/posts editados entram pela plataforma 8links e publicam automaticamente (via Git/CF).

### Cores
Variáveis em `src/styles/global.css`: `--color-primary`, `--color-accent`, `--color-dark`.

## SEO

- `src/components/Seo.astro` injetado pelo `BaseLayout`: title, description, canonical, OG, Twitter, `name="robots"`, JSON-LD (WebSite nas páginas estáticas, BlogPosting nos artigos).
- `src/pages/robots.txt.ts` e `src/pages/sitemap.xml.ts` gerados dinamicamente (sitemap inclui posts com lastmod).

## Deploy

```bash
bun install
bun run build
# Publicar no Cloudflare: a pasta dist/ é servida como Worker (adaptador @astrojs/cloudflare)
# Envs opcionais no CF: SUPABASE_URL e SUPABASE_ANON_KEY (fallbacks embutidos no código)
```