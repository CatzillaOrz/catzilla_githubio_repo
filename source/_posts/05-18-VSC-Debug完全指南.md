---
title: VSC Debug完全指南
date: 2020-05-18 17:10:41
tags:
  - Debug
categories:
  - Frontend
---

###### vsc debug 完全指南 - chrome

- 安装vsc chrome debug插件
- 配置config

```js
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "chrome",
            "request": "launch",
            "name": "Launch Chrome against localhost",
            "url": "http://localhost:4200",
            "webRoot": "${workspaceFolder}/universal/"
        }
    ]
}
```

###### vsc debug 完全指南 - node

- run with nodemon

```bash
    nodemon --inspect=0.0.0.0:9229 app.js
```

- 安装vsc node debug插件

```js
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Launch",
            "type": "node",
            "request": "launch",
            "program": "${workspaceRoot}/universal/app.js",
            "stopOnEntry": false,
            "args": [],
            "cwd": "${workspaceRoot}/universal",
            "preLaunchTask": null,
            "runtimeExecutable": null,
            "runtimeArgs": [
              "--nolazy"
            ],
            "env": {
              "NODE_ENV": "development"
            },
            "console": "internalConsole",
            "sourceMaps": false,
            "outFiles": []
          },
          {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 9229,
            "address": "localhost",
            "restart": true,
            "sourceMaps": false,
            "outFiles": [],
            "localRoot": "${workspaceRoot}/universal/",
            "remoteRoot": "http://localhost:8080"
          }
    ]
}
```

[vsc-cn-doc](https://jeasonstudio.gitbooks.io/vscode-cn-doc/content/md/%E7%BC%96%E8%BE%91%E5%99%A8/%E8%B0%83%E8%AF%95.html)

[中文文档已过期，建议查看最新官方文档](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)
