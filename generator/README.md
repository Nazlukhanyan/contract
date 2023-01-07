# Generator Readme

[![Netlify Status](https://api.netlify.com/api/v1/badges/dc7d73d9-c327-4bcd-a33a-657603bc64ab/deploy-status)](https://app.netlify.com/sites/stefanmatei/deploys)

## Running it online

You can run the generator online:
* [**Generate a contract** online →](https://stefanmatei.com/contract-generator/edit)

## Downloading and running the generator on your own server (3 options)

Alternatively, you can download the generator as a small app written in vanilla Javascript:

* [Download **generator.zip**](https://github.com/nonsalant/contract/releases/)

…and transfer it to your own server.

If running the generator locally, a local server will nedd to be started (eg: using the <a href="https://marketplace.visualstudio.com/items?itemName=yandeu.five-server" target="_blank">Five Server</a> extension in VS Code) in order for the generator to work (browsers block ES Javascript imports from being used locally).

### Optional build step
(for bundling styles and using postcss)

Based on the level of control you need over the styles for the generator and the contract itself, you can go with one of the following 3 options:
<br /><br />


### Option 1: No build setp

The generator can be used without any build step, with the existing contract styles (in regular/vanilla css) already compiled in `📁data/style.min.css`. 

---

### Option 2: Build step for the contract styles
(in `📁data/more-data/css`)

The styles for the contract use postcss for:
* importing multiple css files into a single minified file
* enabling use of selector nesting

To edit the uncompilled postcss, a build step is needed with the following commands to “build” or “watch” (re-build on every change) the contract styles:

```bash
npm run postcss:build 
```
```bash
npm run postcss:watch
```

Postcss configuration can be found in `postcss.config.js`

---

### Option 3: Enabling PostCSS for the generator styles
(in `📁styles`)

If using postcss in the generator's `📁styles` folder too (in addition to the contract styles in `📁data/more-data/css`), you will need a separate watch command for the generator styles:

```bash
postcss:watch-generator
```

...and the following files will need to be edited accordingly:

#### Inside the `package.json` file

replace:
```json
  "scripts": {
    "postcss:watch": "postcss data/more-data/css/main.css -o data/style.min.css -w",
    "postcss:build": "postcss data/more-data/css/main.css -o data/style.min.css"
  },
```
with:
```json
  "scripts": {
    "postcss:watch": "postcss data/more-data/css/main.css -o data/style.min.css -w",
    "postcss:watch-generator": "postcss styles/main.css -o styles/style.min.css -w",
    "postcss:build": "postcss data/more-data/css/main.css -o data/style.min.css & postcss styles/main.css -o style.min.css"
  },
```
#### Inside the `edit.html` file:

replace:
```html
    <link rel="stylesheet" href="styles/main.css">
    <!-- <link rel="stylesheet" href="styles/style.min.css"> -->
```
with:
```html
    <!-- <link rel="stylesheet" href="/styles/main.css"> -->
    <link rel="stylesheet" href="/style.min.css">
```
Configuration for postcss can be found in `postcss.config.js`
