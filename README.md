# ci-test

This template should help get you started developing with Vue 3 in Vite.

## Recommended IDE Setup

[VS Code](https://code.visualstudio.com/) + [Vue (Official)](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur).

## Recommended Browser Setup

- Chromium-based browsers (Chrome, Edge, Brave, etc.):
  - [Vue.js devtools](https://chromewebstore.google.com/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd)
  - [Turn on Custom Object Formatter in Chrome DevTools](http://bit.ly/object-formatters)
- Firefox:
  - [Vue.js devtools](https://addons.mozilla.org/en-US/firefox/addon/vue-js-devtools/)
  - [Turn on Custom Object Formatter in Firefox DevTools](https://fxdx.dev/firefox-devtools-custom-object-formatters/)

## Type Support for `.vue` Imports in TS

TypeScript cannot handle type information for `.vue` imports by default, so we replace the `tsc` CLI with `vue-tsc` for type checking. In editors, we need [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) to make the TypeScript language service aware of `.vue` types.

## Customize configuration

See [Vite Configuration Reference](https://vite.dev/config/).

## Project Setup

```sh
pnpm install
```

### Compile and Hot-Reload for Development

```sh
pnpm dev
```

### Type-Check, Compile and Minify for Production

```sh
pnpm build
```

### Run Unit Tests with [Vitest](https://vitest.dev/)

```sh
pnpm test:unit
```

### Run End-to-End Tests with [Playwright](https://playwright.dev)

```sh
# Install browsers for the first run
npx playwright install

# When testing on CI, must build the project first
pnpm build

# Runs the end-to-end tests
pnpm test:e2e
# Runs the tests only on Chromium
pnpm test:e2e --project=chromium
# Runs the tests of a specific file
pnpm test:e2e tests/example.spec.ts
# Runs the tests in debug mode
pnpm test:e2e --debug
```

### Lint with [ESLint](https://eslint.org/)

```sh
pnpm lint
```

## Automatic Docker deployment

The Dockerfile uses the company Artifactory base images by default, so local
builds can continue to use the normal command:

```sh
docker build -t ci-test:local .
```

The GitHub Actions workflow overrides `NODE_IMAGE` and `NGINX_IMAGE` with the
official Docker Hub image names, because GitHub-hosted runners do not have
access to the internal Artifactory.

Every push to `main` builds the application, publishes a Docker image to GHCR,
and deploys it to the production server. The server must have Docker installed,
and the deployment user must be allowed to run Docker commands.

Create a GitHub environment named `production`, then add these environment
secrets:

- `DEPLOY_HOST`: server IP address or hostname
- `DEPLOY_PORT`: SSH port (optional, defaults to `22`)
- `DEPLOY_USER`: SSH username
- `DEPLOY_SSH_KEY`: private key used to connect to the server
- `DEPLOY_HOST_FINGERPRINT`: server SSH host-key fingerprint in SHA256 format
- `GHCR_USERNAME`: GitHub username used by the server to pull the image
- `GHCR_PAT`: GitHub classic PAT with `read:packages` permission

Optionally add an environment variable named `APP_PORT` to change the exposed
server port. It defaults to port `80`.
