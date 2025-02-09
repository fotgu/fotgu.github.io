---
date: '2025-02-09'
draft: false
title: 'Config Hugo With Env'
description: '使用环境变量配置Hugo'
summary: '初极狭，才通人。复行数十步，豁然开朗'
tags: ['博客建站']
---

> 安全声明：文章中可能存在错误和过时的信息，请在辨别之后再决定是否采纳。

## 前置知识

- 了解 [Hugo](https://gohugo.io/)
- 了解环境变量

## 为什么要使用环境变量配置Hugo

Hugo 支持通过配置文件(hugo.toml hugo.yaml hugo.json) 做配置，也支持通过环境变量配置。
> 参考 <https://gohugo.io/getting-started/configuration/>

本地运行 Hugo 时，肯定是通过文件配置更方便； \
但是如果在托管平台执行构建，通过环境变量配置则更灵活，因为可以利用构建脚本动态指定参数。

## Hugo环境变量命名模式

Hugo 在识别环境变量配置时，有三个关键的规则:

1. 所有字符必须大写
> 根据实际测试，如果环境变量使用 `HUGO_PARAMS_myVar=aaa`，构建时拿到的是 `myvar=aaa`；
> 这意味着基本告别驼峰命名法。
2. 必须以 `HUGO` 开头
3. `HUGO` 后的**第一个字符**用作分隔符，并约定了 [合法的分隔符](https://stackoverflow.com/questions/2821043/allowed-characters-in-linux-environment-variable-names/2821183#2821183)
> 分隔符用于支持配置的级别，如 `HUGO_PARAMS_ENV` 在配置文件中对应的是 `hugo.params.env` \
> 这条规则说明，如果用 `HUGO_` 开头，就告别了蛇形命名法。 \
> 但是，可以用其他字符作为分隔符，比如 `HUGOxMY_VAR`，这样就能对应配置文件中的 `hugo.params.my_var`

基于上面的三条规则，可以得出三种环境变量命名模式：

| 类型         | 环境变量示例         | 配置key示例          | 注意                                               |
| :----------- | :------------------- | :------------------- | :------------------------------------------------- |
| 蛇形         | `HUGOxPARAMSxMY_VAR` | `hugo.params.my_var` | 配置key中需避免分隔符`x`;如果必须有`x`，需要换一个分隔符 |
| 全大写       | `HUGO_PARAMS_MYVAR`  | `hugo.params.myvar`  |                                                    |
| 多级别全大写 | `HUGO_PARAMS_MY_VAR` | `hugo.params.my.var` | 配置key中不能有`_`                                 |
