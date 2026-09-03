# skelator

An Astro project with Svelte and Tailwind CSS v4.

## 🚀 Project Structure

```text
/
├── public/
├── src/
│   ├── pages/
│   │   └── index.astro
│   └── styles/
│       └── global.css
├── astro.config.mjs
├── svelte.config.js
└── package.json
```

Astro looks for `.astro` or `.md` files in `src/pages/`. Each page is exposed as a route based on its file name.

**Integrations:**
- **[Svelte](https://svelte.dev/)** — via `@astrojs/svelte`
- **[Tailwind CSS v4](https://tailwindcss.com/)** — via `@tailwindcss/vite`

Any static assets, like images, can be placed in the `public/` directory.

## 🧞 Commands

All commands are run from the root of the project, from a terminal:

| Command                   | Action                                           |
| :------------------------ | :----------------------------------------------- |
| `npm install`             | Installs dependencies                            |
| `npm run dev`             | Starts local dev server at `localhost:4321`      |
| `npm run build`           | Build your production site to `./dist/`          |
| `npm run preview`         | Preview your build locally, before deploying     |
| `npm run astro ...`       | Run CLI commands like `astro add`, `astro check` |
| `npm run astro -- --help` | Get help using the Astro CLI                     |

## 👀 Want to learn more?

Feel free to check [our documentation](https://docs.astro.build) or jump into our [Discord server](https://astro.build/chat).
