# Angular and Tauri

Tips for working with Tauri using an Angular front-end.

## Setting up tauri.conf.json for bundle building

For npm:

```json
"build": {
    "beforeBuildCommand": "npm run build",
    "beforeDevCommand": "npm run start",
    "devPath": "http://localhost:4200",
    "distDir": "../dist/your-app"
  },
```

For yarn:

```json
"build": {
    "beforeDevCommand": "yarn start --port 1420",
    "beforeBuildCommand": "yarn build",
    "devPath": "http://localhost:1420",
    "distDir": "../dist/tauri-pomodoro",
    "withGlobalTauri": false
  },
```

## Creating a Window

* Add the window info to your `angular.json` file

```json
"assets": [
  "src/favicon.ico",
  "src/assets",
  {
    "glob": "about.component.html",
    "input": "src/app/about",
    "output": "/about"
  }
],
```

* To use tailwind styles, you must add the link to the `styles.css` file.

```html
<link rel="stylesheet" href="/styles.css" />
<div>
  <h1 class="text-4xl font-bold text-sky-500">About Component</h1>
  <p class="text-lg">This is the content of the About component.</p>
</div>
```

* Import the Tauri windows API

```javascript
import { WebviewWindow } from '@tauri-apps/api/window';
```

Create a function called openAboutPage() in your AppComponent that uses the WebviewWindow class to create a new window:

```javascript
async openAboutPage() {
  const aboutPageWindow = new WebviewWindow('aboutPage', {
    url: 'about.html',
  });
  aboutPageWindow.once('tauri://created', () => {
    // do something once the window is created
  });
  aboutPageWindow.once('tauri://error', (e) => {
    console.error(e);
  });
}
```
