+++
title = 'Fastify-cli'
date = 2023-11-13T17:50:44+08:00
draft = true
+++
## 安装fastify-cli
```
pnpm i -g fastify-cli
```

## 创建项目
```
fastify generate app
cd app
pnpm i
```

## 启动
```
pnpm dev
```
## app.js修改 
用vscode的功能修改为ESModule

```
'use strict'

import { join } from 'node:path'
import AutoLoad from '@fastify/autoload'

// Pass --options via CLI arguments in command to enable these options.
const options = {}

export default async function (fastify, opts) {
  // Place here your custom code!

  // Do not touch the following lines

  // This loads all plugins defined in plugins
  // those should be support plugins that are reused
  // through your application
  // 自动加载插件
  fastify.register(AutoLoad, {
    dir: join(__dirname, 'plugins'),
    options: Object.assign({}, opts)
  })

  // This loads all plugins defined in routes
  // define your routes in one of these
  // 自动加载路由
  fastify.register(AutoLoad, {
    dir: join(__dirname, 'routes'),
    options: Object.assign({}, opts)
  })
}

const _options = options
export { _options as options }

```