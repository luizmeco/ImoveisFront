# Casa Nobre — Imobiliária

Landing page responsiva desenvolvida com Vue 3, TypeScript e Vite. A estrutura separa a página principal dos componentes de navegação, apresentação e acesso para facilitar a evolução do projeto.
O Tailwind CSS v4 está integrado ao pipeline do Vite para uso de classes utilitárias, com tokens de tema definidos no CSS global.

## Requisitos

- Node.js 22 ou superior e npm, para desenvolvimento local.
- Docker com Docker Compose, para executar a versão containerizada.

## Desenvolvimento local

```sh
npm install
npm run dev
```

O Vite informa no terminal o endereço local da aplicação. Para validar a versão de produção:

```sh
npm run build
npm run preview
```

Use classes utilitárias do Tailwind diretamente nos templates Vue. Tokens reutilizáveis de cores e fontes estão declarados com `@theme` em `src/style.css` (por exemplo, `text-brand-blue` e `font-display`).

## Executar o front com Docker

O Docker do front é independente da API. Com Docker e Docker Compose instalados, abra um terminal nesta pasta (`ImoveisFront`) e execute:

```sh
docker compose up --build
```

O Compose inicia somente o front, servido pelo Nginx, em <http://localhost:8080>.

Para executar em segundo plano, consultar o estado e encerrar:

```sh
docker compose up --build -d
docker compose ps
docker compose down
```

## Usar junto com a API

Os projetos são iniciados separadamente. Para executar também a API, abra outro terminal na pasta `ImoveisApi` e siga o README de lá. A API fica disponível em <http://localhost:8081>; inicie os dois projetos quando precisar usar front e back ao mesmo tempo.

A interface atual ainda é demonstrativa e não consome os endpoints da API.

## Estrutura

```text
src/
  components/
    HeroSection.vue
    LoginDialog.vue
    SiteHeader.vue
  App.vue
  main.ts
  style.css
Dockerfile
nginx.conf
```

O formulário de acesso é somente demonstrativo: autenticação e integração com backend não fazem parte desta base.
