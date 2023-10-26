+++
title = '学习Fastify'
date = 2023-10-26T23:05:10+08:00
draft = true
+++

## 学习fastify

### 创建
```shell
pnpm init
pnpm i fastify --save
```

创建index.mjs
```javascript
import Fastify from 'fastify'
const fastify = Fastify({
  logger: true
})
// 声明路由
fastify.get('/', function (request, reply) {
  reply.send({ hello: 'world' })
})

// 启动服务！
fastify.listen(3000, function (err, address) {
  if (err) {
    fastify.log.error(err)
    process.exit(1)
  }
  // 服务器监听地址：${address}
})
```

### 启动服务器
```shell
pnpm start
```