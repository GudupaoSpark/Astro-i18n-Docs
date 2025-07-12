---
title: 开始
description: 快速开始
---

# Astro-i18n

## 安装

```bash
pnpm add @gudupao/astro-i18n
```

## 使用方法

### 1. 在Astro配置中添加插件

```typescript
// astro.config.mjs
import { defineConfig } from 'astro/config';
import { astroI18nPlugin } from '@gudupao/astro-i18n';

export default defineConfig({
  integrations: [
    astroI18nPlugin({
      localesDir: './locales',      // 语言文件目录，默认为'locales'
      fallbackLang: 'en',        // 后备语言，默认为'en'
      autoRedirect: true         // 是否自动重定向到用户语言路径，默认为true
    })
  ]
});
```

### 2. 创建语言文件

在项目根目录创建`locales`文件夹，并添加语言文件：

```
/locales
  /en.json      # 英文翻译
  /zh.json      # 中文翻译
```

### 3. 在页面中使用

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
## 语言切换器示例（包含Cookies检测设置方法）

```astro
---

interface Props {
  lang: string;
}

const { lang } = Astro.props;

---
<script is:inline>
  function switchLanguage(language) {
    // 設置語言 Cookie
    document.cookie = `lang=${language}; path=/; max-age=31536000; samesite=lax`;
    console.log('[LanguageSwitcher] 設置語言 Cookie:', language);
    
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

## 特性

- 支持URL路径语言代码 (`/en/about`, `/zh/about`)
- 自动语言检测（从URL、Cookie、Accept-Language头）
- 自动重定向到用户首选语言
- 支持参数化翻译
- 支持多级目录结构的翻译文件

## 许可证

MIT
