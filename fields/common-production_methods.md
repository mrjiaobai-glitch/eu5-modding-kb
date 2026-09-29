# common/production_methods（生产方式）

> **一句话**：生产方式的全部字段：投入商品与数量、产出商品与产量、免维护开关，以及两个国家作用域门槛。
> **什么时候看**：写生产方式、要核对商品输入输出是否在 common/goods 存在时翻这篇。
> **体量**：27 行 · 约 2 分钟通读

来源：`in_game\common\production_methods\__readme.txt`（注意：文件名为 `__readme.txt`）

## 字段

```
<name> = {
    <goods_input> = <amount>      # 输入商品及数量
    produced = <goods>            # 产出商品
    output = <amount>             # 产出量
    potential = { <triggers> }    # 国家作用域
    no_upkeep = <yes/no>          # 阻止维护成本
    allow = { <triggers> }        # 国家作用域
}
```

## 审查要点

- `<goods_input>`/`produced` 引用须在 common/goods 存在。
- potential/allow 为国家作用域。
- 未在 readme 中说明：本地化键格式。
