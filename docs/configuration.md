---
title: Configuration
description: Detailed configuration options for Astro-i18n
---

# Astro-i18n Configuration

## Overview

Astro-i18n can be configured with various options to customize its behavior. This document details each configuration option, its purpose, and default value.

Astro-i18n supports two main modes of operation:
- **Path-based**: Languages are identified by URL paths (e.g., `/en/about`, `/zh/about`). This is controlled by the `pathBasedRouting` option.
- **Path-less**: Language is determined by other means (cookie, Accept-Language header) without changing the URL path. This is also controlled by the `pathBasedRouting` option.

## Common Configuration Options

These options are available for both path-based and path-less modes.

### `localesDir`

- **Type**: `string`
- **Description**: The directory where your locale files are stored.
- **Default**: `'locales'`

Example:
```typescript
integrations: [
  astroI18nPlugin({
    localesDir: './my-locales'
 })
]
```

### `fallbackLang`

- **Type**: `string`
- **Description**: The language to fall back to if a translation is not found for the current language.
- **Default**: `'en'`

Example:
```typescript
integrations: [
 astroI18nPlugin({
    fallbackLang: 'zh'
  })
]
```

### `autoDetectLanguage`

- **Type**: `boolean`
- **Description**: Whether to automatically detect the user's language from the Accept-Language header.
- **Default**: `true`

Example (with auto-detect):
```typescript
integrations: [
  astroI18nPlugin({
    autoDetectLanguage: true
  })
]
```

Example (without auto-detect):
```typescript
integrations: [
  astroI18nPlugin({
    autoDetectLanguage: false
  })
]
```

### `pathBasedRouting`

- **Type**: `boolean`
- **Description**: Whether to use path-based routing. If `true`, languages are identified by URL paths (e.g., `/en/about`, `/zh/about`). If `false`, language is determined by other means (cookie, Accept-Language header) without changing the URL path.
- **Default**: `true`

This option determines the mode of operation:
- `true` (default): Path-based mode.
- `false`: Path-less mode.

Example (Path-based):
```typescript
integrations: [
  astroI18nPlugin({
    pathBasedRouting: true
  })
]
```

Example (Path-less):
```typescript
integrations: [
  astroI18nPlugin({
    pathBasedRouting: false
  })
]
```

## Path-based Mode Configuration (pathBasedRouting: true)

When `pathBasedRouting` is `true`, Astro-i18n operates in path-based mode. All the common configuration options apply.

## Path-less Mode Configuration (pathBasedRouting: false)

When `pathBasedRouting` is `false`, Astro-i18n operates in path-less mode. All the common configuration options apply.

The main difference is in how the language is determined and how URLs are handled:
- In path-based mode, the language is part of the URL path.
- In path-less mode, the language is determined by other means (cookie, Accept-Language header) and the URL path remains unchanged.