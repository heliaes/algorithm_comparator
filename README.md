# Algorithm Comparator

A simple web app for a university project. Paste two algorithms, give them
the same inputs, and see whether their outputs match — plus a side-by-side
view of their declared time and space complexity.

The app ships with **iterative** and **recursive binary search** as example
algorithms so you can try it out immediately.

![screenshot placeholder](screenshot.png)

## Features

- Two code editors (Algorithm A and Algorithm B)
- Custom test inputs as JSON
- Output comparison with ✅ / ❌ / "N/A" badges
- Time and space complexity selectors (Big-O)
- Results table with per-test timings and aggregate runtime
- Clean, minimal UI built with React, Vite, and Tailwind CSS

## Important caveats

- **Passing the same tests does NOT prove two algorithms are equivalent.**
  The app only checks the cases you provide. Use this tool for quick
  experimentation, not as a formal proof.
- **Complexity values are user-declared.** The app does not infer Big-O
  from runtime numbers — that would be unreliable. Wall-clock timings are
  also noisy at small input sizes and depend on the browser engine.

## How it works (for the curious)

The editor for each algorithm expects you to define a function with this
exact signature:

```js
function solve(input) {
  // your code here
  return output;
}
```

When you click **Run comparison**, the app:

1. Parses your test cases from JSON.
2. Compiles each algorithm with `new Function(...)` so it runs in your
   browser.
3. Calls `solve(input)` for every test case, on both algorithms.
4. Compares the returned values with a deep-equality check.
5. Times each call (5 runs, averaged) and reports totals.

## Project structure

```
.
├── index.html              # Vite entry HTML
├── package.json
├── src/
│   ├── App.tsx             # Main UI
│   ├── main.tsx            # React entry
│   ├── index.css           # Tailwind setup
│   ├── executor.ts         # Code runner + comparison helpers
│   └── algorithms/
│       └── presets.ts      # Iterative + recursive binary search
└── README.md
```

## Running locally

You need Node.js 18+.

```bash
# 1. Install dependencies
npm install

# 2. Start the dev server
npm run dev
```

Then open the URL Vite prints (usually http://localhost:5173).

## Building for deployment

```bash
npm run build
```

This produces a `dist/` folder with static files you can host anywhere
(GitHub Pages, Netlify, Vercel, …).

## Publishing on GitHub Pages

The simplest path:

1. Push your project to a GitHub repository.
2. Install the `gh-pages` helper as a dev dependency:

   ```bash
   npm install --save-dev gh-pages
   ```

3. Add these two scripts to `package.json`:

   ```json
   {
     "scripts": {
       "predeploy": "npm run build",
       "deploy": "gh-pages -d dist"
     }
   }
   ```

   Also set the `homepage` field at the top of `package.json`:

   ```json
   "homepage": "https://<your-username>.github.io/<your-repo-name>"
   ```

   If you are publishing to a project page (not a user page), also edit
   `vite.config.ts` to set the base path:

   ```ts
   export default defineConfig({
     plugins: [react()],
     base: "/<your-repo-name>/",
   });
   ```

4. Deploy:

   ```bash
   npm run deploy
   ```

5. In GitHub → **Settings → Pages**, make sure the source is set to the
   `gh-pages` branch. Your site will be live at the `homepage` URL above.

## Tech stack

- **React 18** for UI
- **Vite** for dev server / build
- **Tailwind CSS** for styling
- Plain `new Function()` to run user code (browser-only)

## License

MIT — do whatever you want, no warranty.