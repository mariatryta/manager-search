# Pokemon registry app

Build on top of Vue & Vite and using Axios for fetching JSON data, this app allows you to search through managers based on their first and last name.

Main feature is accessible autocomplete, that user can navigate through with keyboard and is accessible to screen readers.

Missing feature - currently no unit testing is present on the project. See to-do list bellow.

Live site: https://manager-search.netlify.app

Repo: https://github.com/mariatryta/manager-search

## Project Setup

- Create an .env file and copy data from .env.example
- Requires node v14

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

- [x] Create search input component = [ ] show 2 managers in list + rest in overflow = [ ] enter closes search and selects manager
  - [x] hide dropdown menu when whole component looses 'focus'
  - [x] style component
  - [x] click, space, enter should select value
  - [x] arrow down enters dropdown and moves down a value
- [x] Fetch JSON data
- [x] Filter data
  - [x] case insesitive
  - [x] name + last name as one string
- [x] Ability to return to input with value after lost focus
- [x] Accesibility
- [ ] Unit tests
- [ ] Make more reusable
