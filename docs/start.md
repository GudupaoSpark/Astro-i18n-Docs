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

### 1. Add the plugin in your Astro configuration

```typescript
// astro.config.mjs
import { defineConfig } from 'astro/config';
import { astroI18nPlugin } from '@gudupao/astro-i18n';

export default defineConfig({
  integrations: [
    astroI18nPlugin({
      localesDir: './locales',      // Directory for locale files (default: 'locales')
      fallbackLang: 'en',           // Fallback language (default: 'en')
      autoRedirect: true            // Automatically redirect to user's language (default: true)
    })
  ]
});
```

### 2. Create locale files

Create a `locales` folder in your project root and add your language files:

```
/locales
  /en.json      # English translations
  /zh.json      # Chinese translations
```

### 3. Use in your pages

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

## Language Switcher Example (with cookie detection)

```astro
---

interface Props {
  lang: string;
}

const { lang } = Astro.props;

---
<script is:inline>
  function switchLanguage(language) {
    // Set language cookie
    document.cookie = `lang=${language}; path=/; max-age=31536000; samesite=lax`;
    console.log('[LanguageSwitcher] Set language cookie:', language);
    
    const urlParts = window.location.pathname.split('/');
    urlParts[1] = language;
    window.location.href = urlParts.join('/');
  }

  const currentPath = window.location.pathname;
  const urlParts = currentPath.split('/');
</script>

<div class="language-switcher">
  <button
    onclick="switchLanguage('en')"
    class={lang === 'en' ? 'active' : ''}
  >
    English
  </button>
  <button
    onclick="switchLanguage('zh')"
    class={lang === 'zh' ? 'active' : ''}
  >
    中文
  </button>
  <button
    onclick="switchLanguage('jp')"
    class={lang === 'jp' ? 'active' : ''}
  >
    日本語
  </button>
</div>

<style>
.language-switcher {
  display: flex;
  gap: 10px;
  margin-top: 20px;
}

.language-switcher button {
  padding: 5px 10px;
  border: none;
  background: #f0f0f0;
  border-radius: 5px;
  cursor: pointer;
}

.language-switcher button.active {
  background: #007bff;
  color: white;
}
</style>
```

## Features

* Supports language codes in URL paths (`/en/about`, `/zh/about`)
* Automatic language detection (from URL, Cookie, or Accept-Language header)
* Auto-redirects to the user's preferred language
* Supports parameterized translation
* Supports multi-level translation file structure

## License

MIT

