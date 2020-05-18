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
            "restart": false,
            "sourceMaps": false,
            "outFiles": [],
            "localRoot": "${workspaceRoot}/universal/",
            "remoteRoot": "http://localhost:8080"
          }
    ]
}
```
