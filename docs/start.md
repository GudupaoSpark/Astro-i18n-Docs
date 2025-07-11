---
title: Start
description: Quick start
---


# Astro-i18n

## Installation

```bash
pnpm add @gudupao/astro-i18n
```

## Usage

### 1. Add the plugin to your Astro configuration

```typescript
// astro.config.mjs
import { defineConfig } from 'astro/config';
import { astroI18nPlugin } from '@gudupao/astro-i18n';

export default defineConfig({
  integrations: [
    astroI18nPlugin({
      localesDir: './locales',      // Directory for language files, defaults to 'locales'
      fallbackLang: 'en',           // Fallback language, defaults to 'en'
      autoRedirect: true            // Automatically redirect to user's preferred language path, defaults to true
    })
  ]
});
```

### 2. Create language files

Create a `locales` folder in your project root and add language files:

```
/locales
  /en.json      # English translations
  /zh.json      # Chinese translations
```

### 3. Use in pages

```astro
---
const { lang } = Astro.params;
const { t } = Astro.locals;
---

<html lang={lang}>
<head>
  <title>{t('welcome')}</title>
</head>
<body>
  <h1>{t('hello')}</h1>
  <p>{t('language')}: {lang}</p>
</body>
</html>
```

## Features

* Supports language codes in URL paths (`/en/about`, `/zh/about`)
* Automatic language detection (from URL, cookies, and `Accept-Language` header)
* Auto-redirects to the user's preferred language
* Supports parameterized translations
* Supports multi-level translation file structures

## License

MIT
