# Baseline Next.js project

## Creation

The pnpm equivalent of npx is `pnpm dlx`. This command allows you to run a package without installing it globally, similar to npx.

So instead of `npx create-next-app@latest baseline --app --ts --eslint --tailwind --use-pnpm` we can use:

```bash
pnpm dlx create-next-app@latest baseline --app --ts --eslint --tailwind --use-pnpm
```

This setup uses App Router, TypeScript, TailwindCSS, ESLint, and pnpm

## Config

Install Prettier, ESLint Plugins, and Tailwind CSS Plugin

```bash
pnpm add -D prettier eslint-config-prettier eslint-plugin-prettier @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint-plugin-tailwindcss prettier-plugin-tailwindcss
```

- eslint-config-prettier: disables ESLint rules that may conflict with Prettier
- eslint-plugin-prettier: allows Prettier rules to be run as ESLint rules and reported as part of ESLint errors and warnings result.
- @typescript-eslint/parser: parses Typescript to Javascript format that Eslint can work on.
- @typescript-eslint/eslint-plugin: provides Eslint rules and default configurations specifically for Typescript.
- prettier-plugin-tailwindcss: Automatically sorts classes based on the recommended class order.

`.prettierrc`

```json
{
  "singleQuote": true,
  "semi": true,
  "tabWidth": 2,
  "printWidth": 90,
  "trailingComma": "none",
  "jsxSingleQuote": false,
  "arrowParens": "always",
  "bracketSpacing": true,
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

### Husky

This configuration specifies which files to run linting and formatting on when staging changes for commit.

```bash
pnpm add -D husky lint-staged
```

Init Husky

```bash
pnpm exec husky init
```

```bash
#.husky/pre-commit
pnpm lint-staged
```

package.json
`"prepare": "node .husky/install.mjs || exit 0",`

install.mjs

```js
// Skip Husky install in production and CI
if (process.env.NODE_ENV === 'production' || process.env.CI === 'true') {
  process.exit(0);
}
const husky = (await import('husky')).default;
console.log(husky());
```

package.json

```js
"lint-staged": {
    "*.{js,jsx,ts,tsx}": [
      "prettier --write",
      "eslint --fix"
    ],
    "*.{css,scss}": [
      "prettier --write"
    ]
  },
```

## Commands

lint: Runs `next lint` to check for linting issues.
lint:fix: Runs `next lint --fix` to automatically fix linting issues.
format: Runs `prettier --check` . to check if the code is formatted according to Prettier rules.
format:fix: Runs `prettier --write` . to automatically format the code.
