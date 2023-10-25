+++
title = 'Knex Page'
date = 2023-10-25T10:04:27+08:00
draft = true
+++
## knex 中分页优化

query在service层写，可用更好的适应复杂些的查询：join，like等，而调用queryPage,避免了都写计算count，和offset，limit并且统一了返回的数据格式

```javascript
const query = this.db(tbl).where(field, value)
const result = await this.db.queryPage(query, 1, 20)
return result
```

```javascript
const queryPage = async (query, page, size, fields) => {
    const total = await query.clone().count('*', { as: 'count' })
    if (fields) {
        query.select(fields)
    }
    const rows =  query.offset((page - 1) * size).limit(size)
    const rst = { total: total[0].count, records: await rows, current: page.page, size: page.size }
    return rst
}
```