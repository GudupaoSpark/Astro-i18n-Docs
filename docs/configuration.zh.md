---
title: 配置
description: Astro-i18n 的详细配置选项
---

# Astro-i18n 配置

## 概述

Astro-i18n 可以通过各种选项进行配置，以自定义其行为。本文档详细说明每个配置选项、其用途和默认值。

Astro-i18n 支持两种主要操作模式：
- **基于路径**：通过 URL 路径识别语言 (例如 `/en/about`, `/zh/about`)。这由 `pathBasedRouting` 选项控制。
- **无路径**：通过其他方式 (Cookie, Accept-Language 头) 确定语言，而不改变 URL 路径。这也由 `pathBasedRouting` 选项控制。

## 通用配置选项

这些选项适用于基于路径和无路径两种模式。

### `localesDir`

- **类型**: `string`
- **说明**: 存放语言文件的目录。
- **默认值**: `'locales'`

示例:
```typescript
integrations: [
  astroI18nPlugin({
    localesDir: './my-locales'
  })
]
```

### `fallbackLang`

- **类型**: `string`
- **说明**: 当前语言找不到翻译时，回退到的语言。
- **默认值**: `'en'`

示例:
```typescript
integrations: [
  astroI18nPlugin({
    fallbackLang: 'zh'
  })
]
```

### `autoDetectLanguage`

- **类型**: `boolean`
- **说明**: 是否自动从 Accept-Language 头检测用户的语言。
- **默认值**: `true`

示例 (带自动检测):
```typescript
integrations: [
  astroI18nPlugin({
    autoDetectLanguage: true
  })
]
```

示例 (不带自动检测):
```typescript
integrations: [
  astroI18nPlugin({
    autoDetectLanguage: false
  })
]
```

### `pathBasedRouting`

- **类型**: `boolean`
- **说明**: 是否使用基于路径的路由。如果为 `true`，语言由 URL 路径识别 (例如 `/en/about`, `/zh/about`)。如果为 `false`，语言由其他方式 (Cookie, Accept-Language 头) 确定，而不改变 URL 路径。
- **默认值**: `true`

此选项决定了操作模式：
- `true` (默认): 基于路径模式。
- `false`: 无路径模式。

示例 (基于路径):
```typescript
integrations: [
  astroI18nPlugin({
    pathBasedRouting: true
  })
]
```

示例 (无路径):
```typescript
integrations: [
  astroI18nPlugin({
    pathBasedRouting: false
  })
]
```

## 基于路径模式配置 (pathBasedRouting: true)

当 `pathBasedRouting` 为 `true` 时，Astro-i18n 在基于路径模式下运行。所有通用配置选项都适用。

## 无路径模式配置 (pathBasedRouting: false)

当 `pathBasedRouting` 为 `false` 时，Astro-i18n 在无路径模式下运行。所有通用配置选项都适用。

主要区别在于语言如何确定以及 URL 如何处理：
- 在基于路径模式下，语言是 URL 路径的一部分。
- 在无路径模式下，语言由其他方式（Cookie、Accept-Language 头）确定，URL 路径保持不变。