# Neon-Vue-DigitalOcean-Demo-Project

Welcome to the Neon-Vue-DigitalOcean-Demo-Project! This app showcases the power of Vue, DigitalOcean, and Neon for database management.

# Technologies Used

- [Neon](https://neon-bindings.com/docs)
- [VueJS](https://vuejs.org/)
- [DigitalOcean](https://www.digitalocean.com/)


# Getting Started

## Clone the Repository

First, clone this repository to your local machine:

```
git clone https://github.com/femakin/Neo-Demo-PostgREST-DigitalOcean-App

```

cd Neo-Demo-PostgREST-DigitalOcean-App


Install Dependencies
Navigate to the project directory and install the necessary dependencies:

```
npm install

```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```


## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur) + [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin).

## Customize configuration

See [Vite Configuration Reference](https://vitejs.dev/config/).

### Run Unit Tests with [Vitest](https://vitest.dev/)

```sh
npm run test:unit
```

### Run End-to-End Tests with [Cypress](https://www.cypress.io/)

```sh
npm run test:e2e:dev
```

This runs the end-to-end tests against the Vite development server.
It is much faster than the production build.

But it's still recommended to test the production build with `test:e2e` before deploying (e.g. in CI environments):

```sh
npm run build
npm run test:e2e
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```

