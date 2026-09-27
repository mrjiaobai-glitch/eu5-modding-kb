# common/country_description_categories（国家简介分类）

来源：`in_game\common\country_description_categories\readme.txt`

## 格式

- 空对象，仅用于在 lobby 设置国家简介描述：

```
<object_key> = {}
```

## 本地化键（由 object_key 推导，固定前缀）

- 分类名：`country_description_category_name_<object_key>`
- 分类描述：`country_description_category_desc_<object_key>`

## 审查要点

- 该分类用于 `in_game\setup\countries` 的国家定义中——引用须存在。
- 本地化键是固定前缀 + object_key，漏键显示 raw key。
