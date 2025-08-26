---
title: 开始
description: 快速开始
---

# Astro-i18n

> **注意**: 安装说明已移至 [安装](installation)。

## 使用方法

有关详细的配置选项，请参见 [配置](configuration)。

### Astro

```astro
---
import { getTranslator } from '@gudupao/astro-i18n';

// 从参数获取语言
const lang = Astro.params.lang || 'en';
const t = getTranslator(lang);
---

<div>
  <h1>Astro Test: {t('hello')} {t('123.a')}</h1>
</div>
```

### React

```jsx
import React from 'react';
import { createClientTranslator } from '@gudupao/astro-i18n';
import type { Translations } from '@gudupao/astro-i18n';

const TestComponent = ({ translations }: { translations: Translations }) => {
  const t = createClientTranslator(translations);
  
 return (
    <div>
      <h1>React Test: {t('hello')} {t('123.a')}</h1>
    </div>
  );
};

export default TestComponent;
```

### Vue

```vue
<template>
  <div>
    <h1>Vue Test: {{ t('hello') }} {{ t('123.a') }}</h1>
  </div>
</template>

<script setup lang="ts">
import { createClientTranslator } from '@gudupao/astro-i18n';
import type { Translations } from '@gudupao/astro-i18n';

interface Props {
  translations: Translations;
}

const props = defineProps<Props>();
const t = createClientTranslator(props.translations);
</script>
```

## 语言切换器示例

### 有路径版本

```astro
---
interface Props {
  lang: string;
}

const { lang } = Astro.props;
---

<script is:inline>
  function switchLanguage(language) {
    // 设置语言 Cookie
    document.cookie = `lang=${language}; path=/; max-age=31536000; samesite=lax`;
    console.log('[LanguageSwitcher] 设置语言 Cookie:', language);
    
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

### 无路径版本

```astro
---
// 获取所有可用的语言
const { getAvailableLanguages } = await import('@gudupao/astro-i18n');
const availableLanguages = getAvailableLanguages();

interface Props {
  pathBasedRouting?: boolean; // 是否使用路径基于路由的模式
}

const { pathBasedRouting = true } = Astro.props;

// 检测当前语言的函数
function detectCurrentLanguage() {
  // 首先检查 URL 参数
  const urlParams = new URLSearchParams(Astro.url.search);
  const langParam = urlParams.get('lang');
  if (langParam) return langParam;

  // 然后检查 Cookie
  const cookies = Astro.request.headers.get('cookie');
  if (cookies) {
    const cookieObj = Object.fromEntries(
      cookies.split(';').map(cookie => {
        const [name, value] = cookie.trim().split('=');
        return [name, decodeURIComponent(value)];
      })
    );
    if (cookieObj.lang) return cookieObj.lang;
 }

  // 返回默认语言
  return 'en';
}

// 获取当前语言
const currentLang = detectCurrentLanguage();
---

<div class="language-switcher">
  <h3>语言切换器 (新版本)</h3>
  <p>当前模式: {pathBasedRouting ? '路径基于路由' : '无路径基于路由'}</p>
  <div class="switcher-buttons">
    {availableLanguages.map((language) => (
      <button
        data-lang={language}
        class={language === currentLang ? 'active' : ''}
        onclick={`switchLanguage('${language}', ${pathBasedRouting})`}
      >
        {language === 'en' && 'English'}
        {language === 'zh' && '中文'}
        {language === 'jp' && '日本語'}
        {language !== 'en' && language !== 'zh' && language !== 'jp' && language}
      </button>
    ))}
  </div>
</div>

<script is:inline>
  // 切换语言的函数
  function switchLanguage(language, pathBasedRouting) {
    if (pathBasedRouting) {
      // 路径基于路由的模式
      // 设置语言 Cookie
      document.cookie = `lang=${language}; path=/; max-age=31536000; samesite=lax`;
      console.log('[LanguageSwitcher] 设置语言 Cookie:', language);
      
      const urlParts = window.location.pathname.split('/');
      urlParts[1] = language;
      window.location.href = urlParts.join('/');
    } else {
      // 无路径基于路由的模式
      // 设置语言 Cookie
      document.cookie = `lang=${language}; path=/; max-age=31536000; samesite=lax`;
      // 设置 localStorage
      localStorage.setItem('lang', language);
      // 重新加载页面以应用新语言
      window.location.reload();
    }
  }
</script>

<style>
.language-switcher {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin: 20px 0;
  padding: 15px;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #f9f9;
}

.switcher-buttons {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.language-switcher button {
  padding: 8px 12px;
  border: none;
  background: #f0f0f0;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.language-switcher button:hover {
  background: #e0e0e0;
}

.language-switcher button.active {
  background: #007bff;
  color: white;
}

.language-switcher h3 {
  margin: 0 0 10px 0;
  font-size: 16px;
  color: #333;
}

.language-switcher p {
  margin: 0 0 10px 0;
  font-size: 14px;
  color: #666;
}
</style>

```