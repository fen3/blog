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

### 增加knex
```shell
pnpm i knex --save
pnpm i mysql2
```

./database.mjs
```javascript
import fastifyPlugin from 'fastify-plugin'
import knex from 'knex'

const fastifyKnex = fastifyPlugin((fastify, options, next) => {
    try {
        if (!fastify.db) {
            //定义一个数据库的实例
            const handler = knex(
                {
                    client: 'mysql2', connection: {
                        host, port: 3306,
                        user: 'root', password: pwd, database: 'jsduty'
                    }, pool: { max: 10, min: 2 }
                }
            )
            fastify.decorate('db', handler)
            fastify.addHook('onClose', (fastify, done) => {
                if (fastify.db === handler) {
                    fastify.db.destroy()
                    delete fastify.db
                }
                done()
            })
    next()
    } catch (error) {
        next(error)
    }
})

export default fastifyKnex
```


在index.mjs增加下面两句
```javascript
import database from './database.mjs'
fastify.register(database)
```

完成后index.mjs变成
```javascript
import Fastify from 'fastify'
import database from './database.mjs'
const fastify = Fastify({
  logger: true
})

fastify.register(database)
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

## 加上 jwt
```shell
pnpm i @fastify/jwt
```

在index.mjs增加引用
```javascript
import jwt from '@fastify/jwt'
fastify.register(jwt, {
    secret: 'secret!!'
})
```
加上登录的方法
```javascript
fastify.post('/signup', (req, reply) => {
  const play = '各种方法获取/生成的，可以用户名，角色'
  const token = fastify.jwt.sign({ payload })
  reply.send({ token })
})
//请求时验证 token
fastify.addHook('onRequest', async (req, res) => {
    try {
        if (`${base_url}/signup` != req.url) {
            await req.jwtVerify()
            //解出 jwt，检查超时，在1小时内，更新到4小时后
            const ctxJwt = await req.jwtDecode()
            if ((ctxJwt.exp - 3600) * 1000 < new Date().valueOf()) {
                res.newToken = fastify.jwt.sign(req.user, { expiresIn: 3600 * 4 })
            }
        }
    } catch (error) {
        res.send(error)
    }
})
```
完成后 index.mjs
```javascript
import Fastify from 'fastify'
import jwt from '@fastify/jwt'
import database from './database.mjs'
const fastify = Fastify({
  logger: true
})

fastify.register(jwt, {
    secret: 'secret!!'
})
fastify.register(database)
// 声明路由
fastify.get('/', function (request, reply) {
  reply.send({ hello: 'world' })
})
fastify.post('/signup', (req, reply) => {
  const play = '各种方法获取/生成的，可以用户名，角色'
  const token = fastify.jwt.sign({ payload })
  reply.send({ token })
})
//请求时验证 token
fastify.addHook('onRequest', async (req, res) => {
    try {
        if (`${base_url}/signup` != req.url) {
            await req.jwtVerify() //验证token
        }
    } catch (error) {
        res.send(error)
    }
})
// 启动服务！
fastify.listen(3000, function (err, address) {
  if (err) {
    fastify.log.error(err)
    process.exit(1)
  }
})
```


