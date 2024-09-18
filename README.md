# 📄 Yarn project boilerplate

## Features

- Yarn (berry) with PnP
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
yarn install
```

## Linting

```sh
yarn run lint
yarn run lint:fix # Lint and auto-fix
```

## Testing

```sh
yarn run test
```

Currently, the command works as an alias for the `yarn run lint` command.

## Cleaning

```sh
yarn run clean
```

## Migrate to NPM

```sh
# 1. Remove all untracked files
git clean -fdx

# 2. Patch to the project
patch -p1 < patches/migrate-to-npm.patch

# 3. Remove some files
npm x -y -- rimraf .yarn .yarnrc.yml yarn.lock

# 4. Install the dependencies
npm install

# (Optional) 5. Commit the changes
git add -A
git commit -m 'chore: migrate to NPM'
```

## Rules for Development

Welcome to contribute to this repository! For more details,
please refer to [CONTRIBUTING.md](.github/CONTRIBUTING.md).

Introduce commit message validation at commit time.
The “**[Conventional Commits](https://www.conventionalcommits.org/ja/)**”
rule is applied to discourage committing messages that violate conventions.

## LICENSE

MIT
