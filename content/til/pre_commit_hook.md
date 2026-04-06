---
title: 'Setting up pre-commit hooks with Husky'
date: 2026-04-05
---

When building a personal site, manual code formatting easily falls through the cracks. Catching syntax or formatting errors locally ensures faster deployments and keeps your repository history clean.

I automated my workflow to lint and format code before every commit using **[Husky](https://typicode.github.io/husky/)**, **[lint-staged](https://github.com/okonet/lint-staged)**, and **[Prettier](https://prettier.io/)**.

## Setup and Configuration

First, install the required dependencies. I include `markdownlint-cli` to automatically check for structural errors in my blog posts.

```bash
npm install --save-dev prettier husky lint-staged markdownlint-cli
```

Next, configure Husky to trigger before every commit.

```bash
npm pkg set scripts.prepare="husky"
npm run prepare
npx husky init
echo "npx lint-staged" > .husky/pre-commit
```

Create a `.prettierrc` file at the root of your project to define your styling conventions.

```json
{
  "printWidth": 100,
  "tabWidth": 2,
  "semi": false,
  "singleQuote": true
}
```

Finally, add the `lint-staged` configuration to your `package.json`. This setup ensures Prettier and your linters only run against currently staged files, making the pre-commit process nearly instantaneous.

```json
{
  "lint-staged": {
    "*.md": ["markdownlint", "prettier --write"],
    "*.json": ["prettier --write"],
    "*.yaml": ["prettier --write"]
  }
}
```

Now, running `git commit` intercepts your staged files. The hook reformats configurations and lints Markdown. If any tooling check fails, the commit aborts entirely—guaranteeing that broken formatting never reaches your production build.
