# 📄 PNPM project template

## Features

- PNPM
- TypeScript
- ESLint
- Prettier
- Commitlint with Husky
- Visual Studio Code / Vim ready
- CI configurations
  - CodeRabbit
  - Dependabot
  - GitHub Actions

## System Requirements

- Node.js
  - Hydrogen LTS (`^18.20.4`)
  - Iron LTS (`^20.10`)
  - `>=22`

## Install the dependencies

```sh
corepack enable
pnpm install
```

## Linting

```sh
pnpm run lint
pnpm run lint:fix # Lint and auto-fix
```

## Testing

```sh
pnpm run test
```

Currently, the command works as an alias for the `pnpm run lint` command.

## Cleaning

```sh
pnpm run clean
```

## Rules for Development

Welcome to contribute to this repository! For more details,
please refer to [CONTRIBUTING.md](.github/CONTRIBUTING.md).

Introduce commit message validation at commit time.
The “**[Conventional Commits](https://www.conventionalcommits.org/ja/)**”
rule is applied to discourage committing messages that violate conventions.

## LICENSE

MIT
