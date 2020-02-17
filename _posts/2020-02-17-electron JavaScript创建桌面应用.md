---
layout: post
title: electron + JavaScript + html创建桌面应用
categories: [electron]
tags: [electron]
fullview: false
---
1.安装node.js,修改npm下载镜像,安装electron,安装electron-builder

```bash
npm config set ELECTRON_MIRROR http://npm.taobao.org/mirrors/electron/
npm install -g cross-env
git clone https://github.com/electron/electron-quick-start
cd electron-quick-start
cross-env npm install --save-dev electron
cross-env npm install --save-dev electron-builder
npm start
```

2.修改配置文件package.json：
```json
    "build": {
        "productName": "AppDemo",
        "appId": "com.AppDemo.app",
        "copyright": "AppDemo",
        "directories": {
        "output": "build"
    },
    "extraResources":  {
        "from":"./resources",
        "to":""
    },
    "win": {
        "icon": "./build/ico/app.ico",
        "target": [
            "nsis",
            "zip"
        ]
    },
    "nsis": {
        "oneClick": false,
        "allowElevation": true,
        "allowToChangeInstallationDirectory": true,
        "installerIcon": "./build/ico/app.ico",
        "uninstallerIcon": "./build/ico/app.ico",
        "createDesktopShortcut": true,
        "createStartMenuShortcut": true,
        "shortcutName": "AppDemo"
    }
  },
  "scripts": {
        "start": "electron .",
        "dist": "electron-builder --win --x64"
  }
```

3.打包

```bash
npm run dist
```

4.vscode开发环境配置：

打开main.js，点debug按钮，点配置按钮代开launch.json

```json
{
    "version":"0.2.0",
    "configurations":[
        {
            "name":"Debug Main Process",
            "type":"node",
            "request":"launch",
            "cwd":"${workspaceRoot}",
            "runtimeExecutable":"${workspaceRoot}/node_modules/.bin/electron",
            "windows":{
                "runtimeExecutable":"${workspaceRoot}/node_modules/.bin/electron.cmd"
            },
            "args":["."],
            "outputCapture":"std"
        }
    ]
}
```

5.修改main.js，使electron可以调用nodejs

```js
function createWindow () {
  // Create the browser window.
  mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      javascript: true,
      plugins: true,
      nodeIntegration: true, // 是否集成 Nodejs
      webSecurity: false,
      preload: path.join(__dirname, 'preload.js')
    }
  }
```

6.修改main.js使应用不显示菜单栏

```js
//在function createWindow () {最后调用函数createMenu()；
function createMenu() {
  // darwin as macOS，config for macOS
  if (process.platform === 'darwin') {
      const template = [
      {
          label: 'App Demo',
          submenu: [
              {
                  role: 'about'
              },
              {
                  role: 'quit'
              }]
      }]
      let menu = Menu.buildFromTemplate(template)
      Menu.setApplicationMenu(menu)
  } else {
      // windows and linux
      Menu.setApplicationMenu(null)
  }
}
```

二、electron开发中注意点

1.使html中css生效，在head中加入行
```html
<meta htto-equiv="Content-Security-Policy" content="default-src 'none'; script-src 'self'">
```

2.利用js触发html点击事件
```html
index.html
<button id="btn_click">查看系统信息</button>
<div style="color:#F00; font-size:18px" id="sysInfo"></div>
```
```js
rander.js
document.getElementById("btn_click").onclick=function(){
    console.log("arch", process.arch);
    var env=JSON.stringify(process.env); //object to string
    document.getElementById("sysInfo").innerText=("env: " + env);
}
```

3.index.html页面中加载其他页面，并为每个页面创建相应的js文件，就实现了模块化。在render.js中控制各加载页面的显示属性，使index.html中每个按钮对应一个模块。
```html
<webview class="soft" id="vcf2hmp" src="vcf2hmp.html" style="display:inline-flex;" nodeintegration></webview>
```

4.调用系统命令或脚本
```js
var cp = require('child_process');
var result_code=0;
var result_stderr="";

result = cp.spawn("script\\perl.exe", ['script\\vcf2hmp.pl', fileName_vcf, fileName_multianno]);
result.stderr.on('data', function(data) {
    result_stderr=data;
});
result.on('close', function(code) {
    result_code=code;
    if(result_code==0){
        print_log("vcf to hmp process successfully end!\n");
    }else{
        print_log(result_stderr);
        print_log("vcf to hmp process error end!\n");
    }
    enableButton();
});
```

5.美化滚动条
```css
::-webkit-scrollbar-track {
  background-color:#f8f8f8;
  width:8px;
  height: 8px;
}
::-webkit-scrollbar {
  width:8px;
  height: 8px;
}
::-webkit-scrollbar-thumb {
  background-color:#dddddd;
  background-clip:padding-box;
  min-height:28px;
}
::-webkit-scrollbar-thumb:hover {
  background-color:#bbb;
}
::-webkit-scrollbar-button {
  background-color: #bbb;
  height: 8px;
  width: 8px;
}
```

6.使div显示点击效果
```css
.menu{
    background-color:rgb(240, 240, 240);
    height:555px;
    width:25%;
    float:left;
    margin-right: 1px;
    font-size: 11pt;
    overflow:auto;
}

.menu-item{
    height:40px;text-align:right;
    padding-top: 20px;
    padding-right: 10px;
}

.menu-item:hover{
    background-color: rgb(230, 230, 230);
}

.menu-item:active{
    background-color: rgb(220, 220, 220);
}
```

7.pdf浏览器
```html
<div>
    <table>
    <tr>
        <td class="label">pdf_file:</td>
        <td><input type="text" name="" size="50" id="path_pdf" disabled="disabled"></td>
        <td>
        <input type="file" name="" id="file_pdf" style="display:none">
        <button id="open_pdf">open file</button>
        </td>
    </tr>
    </table>
    <iframe id="pdf_view" frameborder="0"></iframe>
</div>
```
```js
var fileName_pdf="";

document.getElementById("open_pdf").onclick=function(){
    file_vcf=document.getElementById("file_pdf").click();
    //document.getElementById("log").innerText=file_vcf;
}
document.getElementById("file_pdf").onchange=function(){
    var selectedFile = document.getElementById('file_pdf').files[0];
    fileName_pdf = selectedFile.path;
    document.getElementById("path_pdf").value=fileName_pdf;
    document.getElementById("pdf_view").setAttribute("src","./js/pdf/web/viewer.html?file=" + fileName_pdf);
}
```