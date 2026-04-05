# Angith Jayan - Personal Website

Welcome to the repository for my personal website, accessible at [angith.dev](https://angith.dev/).
This site serves as my portfolio, blog, project showcase, and a collection of things I've learned
(TIL).

## 🚀 Tech Stack

- **Framework**: [Hugo](https://gohugo.io/) (Fast Static Site Generator)
- **Deployment**: [Netlify](https://www.netlify.com/)
- **Linting & Formatting**: `markdownlint`, `prettier`, `husky`, and `lint-staged` via Node.js

## 🛠️ Prerequisites

To run this project locally, you will need:

1. **Hugo (Extended)**: Installed locally.
   - MacOS via Homebrew: `brew install hugo`
   - _Note: The Netlify build uses Hugo v0.157.0 (as specified in `netlify.toml`)._
2. **Node.js & npm**: Used for local linting, formatting, and pre-commit hooks.

## 💻 Local Development

1. **Clone the repository:**

   ```bash
   git clone <repository-url>
   cd personal-website
   ```

2. **Install Node dependencies:** This project uses `husky` and `lint-staged` for pre-commit hooks
   to ensure Markdown formatting is consistent.

   ```bash
   npm install
   ```

3. **Run the local development server:**

   ```bash
   hugo server -D
   ```

   The site will be available at `http://localhost:1313/` (the `-D` flag renders draft files
   locally).

## 📂 Project Structure

- `content/`: Contains all the Markdown content organized by sections (`blog/`, `til/`, `projects/`,
  `portfolio/`).
- `layouts/`: Custom HTML templates and shortcodes used to structure and design the site.
- `static/`: Static assets such as CSS (`style.css`), images, and fonts. These bypass the Hugo
  processor.
- `data/`: Additional data files used throughout the templates.
- `hugo.toml`: The main configuration file defining menus, base URL, and site titles.
- `netlify.toml`: Configuration settings for the automated Netlify deployment.

## ✨ Linting & Formatting

The codebase uses Prettier and Markdownlint to ensure files stay neat. They are configured to run
automatically as pre-commit hooks via Husky. You can format files manually if you wish:

```bash
npx prettier --write "**/*.{md,json,yaml}"
npx markdownlint-cli --fix "**/*.md"
```

## 🚀 Deployment

The site is configured for continuous deployment on **Netlify**. Any commits pushed to the main
branch will automatically trigger the build command (`hugo`) and publish the `public/` directory.
