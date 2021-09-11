# Setting Up Tailwind CSS

## Install dependencies

First let's init `the package.json` file.

```bash
npm init -y
```

and then install required dependencies.

```bash
npm install -D tailwindcss postcss autoprefixer vite
```

Update the `scripts` field in the `package.json` file for development mode.

```json
"scripts": {
    "dev": "vite"
}
```

## Configuration

Next we need to generate configure files(`tailwind.config.js`, `postcss.config.js`) for Tailwindcss and Postcss, so that it will process our css automatically.

```bash
npx tailwindcss init -p
```

## Optimizing for production

For production mode we add

```json
"scripts": {
    "dev": "vite",
    "build": "vite build"
}
```

### Removing unused CSS

We need to provide an array of paths to all of your template files using the purge option. This list should include any files in your project that reference any of your styles by name.

```javascript
// tailwind.config.js
module.exports = {
  purge: ['./src/**/*.html', './src/**/*.vue', './src/**/*.jsx'],
  theme: {},
  variants: {},
  plugins: [],
};
```

Now whenever you compile your CSS with **NODE_ENV** set to **production**, Tailwind will automatically purge unused styles from your CSS.

Let's test it by running `npm run build`.
