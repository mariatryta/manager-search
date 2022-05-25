# Pokemon registry app

Build on top of Vue & Vite and using api for fetching JSON data, this app allows you to search through managers based on their first and last name.

## Project Setup

Create an .env file and copy data from .env.example

```sh
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

### Run Unit Tests with [Cypress Component Testing](https://docs.cypress.io/guides/component-testing/introduction)

```sh
npm run test:unit # or `npm run test:unit:ci` for headless testing
```

### Run End-to-End Tests with [Cypress](https://www.cypress.io/)

```sh
npm run build
npm run test:e2e # or `npm run test:e2e:ci` for headless testing
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```

## To-Do

- [ ] Create search input component = [ ] show 2 managers in list + rest in overflow = [ ] enter closes search and selects manager
- [ ] Fetch JSON data
- [ ] Filter data
  - [ ] case insesitive
  - [ ] name + last name as one string
- [ ] Ability to return to input with value after lost focus
- [ ] Accesibility
- [ ] Unit tests
