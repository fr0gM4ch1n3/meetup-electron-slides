## electron-connect

features:
- tart (and restart) Electron application.
- reload renderer processes.
- stop Electron application.

---

## electron-debug

Adds useful debug features to your Electron app

---

## electron-store

Simple data persistence for your Electron app or module - Save and load user preferences, app state, cache, etc

---

## gulp-electron

A gulp plugin that creates electron based distributable applications.

---

## gulp-electron

```
import * as gulp from 'gulp';
import * as electron from 'gulp-electron';
const pkgJson = require('../../../src/client/package.json');
export = () => {
    gulp.src('').pipe(electron({
        src: './dist/prod',
        packageJson: pkgJson,
        release: './dist/release',
        cache: './dist/cache',
        version: 'v1.8.2',
        packaging: true,
        platforms: ['darwin-x64']
    })).pipe(gulp.dest(''));
};
```

---

## Angular Cli + Electron

Create desktop apps based on Angular CLI and Electron

<small>[example/boilerplate](https://github.com/fr0gM4ch1n3/meetup-electron)</small>

---

## Angular Cli + Electron
<br>

#### Development server

```bash
ng serve
```

or

```bash
npm start
```

---

## Angular Cli + Electron
<br>

#### Code scaffolding

```bash
ng generate directive|pipe|service|class|guard|interface|...
```
<br>

#### Running unit tests
```bash
ng test
```
```bash
ng e2e
```

---

## Angular Cli + Electron
<br>

#### Build

```bash
ng build
```

#### Packaging
```bash
npm run package:all
```

---

## Boilerplate

How to build your boilerplate with Angular Cli + Electron

---

### Boilerplate Step 1

start your cli project as usual

```bash
npm install -g @angular/cli
ng new my-electron-app
cd my-electron-app
code . # or start your code editor ;)
```

---

### Boilerplate Step 2

add electron

```
npm install electron --save-dev
```

and then we need a configuration ...

---

### Boilerplate Step 3

Create a “instruction file” called electron.dev.js under the src folder and insert the following code:

```ts
const { app, BrowserWindow } = require('electron');
const path = require('path');
const url = require('url');

// Keep a global reference of the window object, if you don't,
// the window will be closed automatically when the
// JavaScript object is garbage collected.
let win;

const createWindow = () => {
    // set timeout to render the window not until the Angular 
    // compiler is ready to show the project
    setTimeout(() => {
        // Create the browser window.
        win = new BrowserWindow({
            width: 800,
            height: 600,
            icon: './src/favicon.ico'
        });

        // and load the app.
        win.loadURL(url.format({
            pathname: 'localhost:4200',
            protocol: 'http:',
            slashes: true
        }));

        win.webContents.openDevTools();

        // Emitted when the window is closed.
        win.on('closed', () => {
            // Dereference the window object, usually you would
            // store windows in an array if your app supports
            // multi windows, this is the time when
            // you should delete the corresponding element.
            win = null;
        });
    }, 10000);
}

// This method will be called when Electron has finished
// initialization and is ready to create browser windows.
// Some APIs can only be used after this event occurs.
app.on('ready', createWindow);

// Quit when all windows are closed.
app.on('window-all-closed', () => {
    // On macOS it is common for applications and their
    // menu bar to stay active until the user quits
    // explicitly with Cmd + Q
    if (process.platform !== 'darwin') {
        app.quit();
    }
});

app.on('activate', () => {
    // On macOS it's common to re-create a window
    // in the app when the dock icon is clicked and
    // there are no other windows open.
    if (win === null) {
        createWindow();
    }
});
```

---

### Boilerplate Step 4

We create another “instruction file” called electron.prod.js under the src folder that will not include any debug stuff

```ts
const { app, BrowserWindow } = require('electron');
const path = require('path');
const url = require('url');

// Keep a global reference of the window object, if you don't,
// the window will be closed automatically when the
// JavaScript object is garbage collected.
let win;

const createWindow = () => {
    // Create the browser window.
    win = new BrowserWindow({
        width: 800,
        height: 600,
        icon: path.join(__dirname, 'favicon.ico'),
    });

    // and load the index.html of the app.
    win.loadURL(url.format({
        pathname: path.join(__dirname, 'index.html'),
        protocol: 'file:',
        slashes: true
    }));

    // Emitted when the window is closed.
    win.on('closed', () => {
            // Dereference the window object, usually you would
            // store windows in an array if your app supports
            // multi windows, this is the time when
            // you should delete the corresponding element.
        win = null;
    });
}

// This method will be called when Electron has finished
// initialization and is ready to create browser windows.
// Some APIs can only be used after this event occurs.
app.on('ready', createWindow);

// Quit when all windows are closed.
app.on('window-all-closed', () => {
    // On macOS it is common for applications and their
    // menu bar to stay active until the user quits
    // explicitly with Cmd + Q
    if (process.platform !== 'darwin') {
        app.quit();
    }
});

app.on('activate', () => {
    // On macOS it's common to re-create a window
    // in the app when the dock icon is clicked and
    // there are no other windows open.
    if (win === null) {
        createWindow();
    }
});
```

---

### Boilerplate Step 5

As a next step we will bring angular and electron together

install a tool to run both simultanous
```
npm install concurrently --save-dev
```

and modify your package.json

```json
{
  "scripts": {    
      "start": "concurrently \"ng serve\" \"npm run electron\"",
      "electron": "electron ./src/electron.dev",
      ...
  }
}
```

---

### Boilerplate Step 6

Electron uses file url, so "/" is your filesystem root. You have to fix this in your index.html

```
<base href="/">
```
to
```
<base href="./">
```

> <small>The base tag specifies how to handle relative URLs in the html document.</small>


---

### Boilerplate Step 7

An electron app has it own package.json, with a basic configuration

```
{
    "name": "meetup-electron",
    "productName": "Angular CLI + Electron",
    "version": "1.0.0",
    "description": "Angular CLI + Electron",
    "main": "electron.prod.js",
    "license": "MIT"
}
```

---

### Boilerplate Step 8

And tell angular cli to include this and the electron main code into the distribution bundle

<br>

Append the new files to the assets array inside of the .angular-cli.json file

```json
  "assets": [
    ...
    "package.json",
    "electron.prod.js"
  ],
```

---

### Boilerplate Step 9

And the last step, packaging

<br>

install the packager tool and cross-var to use the same command on each platform

```
npm install electron-packager cross-var --save-dev
``` 

---

and add the following scripts to the global package.json file

```json
"scripts": {
  ...
  "package:win": "npm run build && cross-var electron-packager dist $npm_package_name-$npm_package_version --out=packages --platform=win32 --arch=all --overwrite",
  "package:linux": "npm run build && cross-var electron-packager dist $npm_package_name-$npm_package_version --out=packages --platform=linux --arch=all --overwrite",
  "package:osx": "npm run build && cross-var electron-packager dist $npm_package_name-$npm_package_version --out=packages --platform=darwin --arch=all --overwrite",
  "package:all": "npm run build && cross-var electron-packager dist $npm_package_name-$npm_package_version --out=packages  --all --arch=all --overwrite"
}
```

---

## ngx-electron

a frictionless way to use Electron APIs with strong types inside of Angular apps
<br></br>
<small>If your app is not running inside electron, all getters will return NULL instead.</small>

---

## ngx-electron install

just install this one module

```
npm install ngx-electron --save
```

---

## ngx-electron module

import the module in your app module

```
import {NgModule} from '@angular/core';
import {BrowserModule} from '@angular/platform-browser';
import {NgxElectronModule} from 'ngx-electron';
import {AppComponent} from './app.component';
@NgModule({
    declarations: [],
    imports: [
      BrowserModule,
      NgxElectronModule
    ],
    bootstrap: [AppComponent]
})
export class AppModule {}
```

---

## ngx-electron service

import into your component
```
import {Component} from '@angular/core';
import {ElectronService} from 'ngx-electron';
@Component({
  selector: 'my-app',
  templateUrl: 'app.html'
})
export class AppComponent {
    constructor(private _electronService: ElectronService) { }
}
```

---

## ngx-electron service

and use it
```
public playPingPong() {
    if(this._electronService.isElectronApp) {
        let pong: string = this._electronService
          .ipcRenderer.sendSync('ping');
        console.log(pong);
    }
}
```

---

## ngx-electron service

what can it do?

```ts
public get isElectronApp: Boolean;
```

```ts
public get desktopCapturer(): Electron.DesktopCapturer;
public get ipcRenderer(): Electron.IpcRenderer;
public get remote(): Electron.Remote;
public get webFrame(): Electron.WebFrame;
public get clipboard(): Electron.Clipboard;
public get crashReporter(): Electron.CrashReporter;
public get process(): NodeJS.Process;
public get screen(): Electron.Screen;
public get shell(): Electron.Shell;
```

---

### Augury in Electron apps

1. Install Augury in your local Chrome Browser Installation by pulling it from the Chrome WebStore
1. open another tab and navigate to chrome://extensions/
1. locate Augury and copy the id from the extension instance. Something like elgalmkoelokbchhkhacckoklkertfdd

---

### Augury in Electron apps

find the local path of your Augury installation

#### Windows

```bash
%LOCALAPPDATA%\Google\Chrome\User Data\Default\Extensions
```

#### MacOS

```bash
~/Library/Application Support/Google/Chrome/Default/Extensions
```

#### Linux

<small>Depending on the package you used to install Chrome on your Linux system</small>

```bash
~/.config/google-chrome/Default/Extensions/
~/.config/google-chrome-beta/Default/Extensions/
~/.config/google-chrome-canary/Default/Extensions/
~/.config/chromium/Default/Extensions/
```

---

### Augury in Electron apps

example for a really simple Electron “instruction file” using addDevToolsExtension


```ts
app.on('ready', () => {
    const augury = '/Users/user/Library/Application Support' + 
    '/Google/Chrome/Default/Extensions/abcdefghijklmnopqrs' +
    '/1.0.3_0';
    BrowserWindow.addDevToolsExtension(augury);

    mainWindow = new BrowserWindow({ width: 900, height: 600 });
    mainWindow.setTitle('Augury Electron Integration');
    mainWindow.loadURL(`file://${__dirname}/index.html`);
    mainWindow.on('closed', () => {
        mainWindow = null;
    });
});
```
