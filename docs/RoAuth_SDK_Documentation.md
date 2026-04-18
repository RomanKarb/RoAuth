# RoAuth SDK Документация

## Оглавление

1. [Введение](#введение)
2. [Установка](#установка)
3. [Быстрый старт](#быстрый-старт)
4. [API Reference](#api-reference)
   - [window.RoAuth.createButton](#windowroauthcreatebutton)
   - [window.roAuthVerbose](#windowroauthverbose)
   - [window.roAuthShowNotification](#windowroauthshownotification)
   - [window.roAuthResetSuppress](#windowroauthresetsuppress)
   - [window.roAuthTestNotify](#windowroauthtestnotify)
5. [Конфигурация](#конфигурация)
   - [Основные параметры](#основные-параметры)
   - [Стилизация](#стилизация)
   - [Темы](#темы)
   - [Локализация](#локализация)
6. [Примеры использования](#примеры-использования)
   - [Базовый пример](#базовый-пример)
   - [Расширенная конфигурация](#расширенная-конфигурация)
   - [Обработка событий](#обработка-событий)
   - [Пользовательские стили](#пользовательские-стили)
7. [Безопасность](#безопасность)
8. [Troubleshooting](#troubleshooting)
9. [Changelog](#changelog)
10. [Лицензия](#лицензия)

## Введение

RoAuth SDK - это мощный JavaScript SDK для интеграции аутентификации через RoAuth в веб-приложения. SDK позволяет создавать стильные кнопки авторизации, которые открывают popup-окно для безопасной аутентификации пользователей через платформу RoAuth.

### Основные возможности

- **Простая интеграция**: Добавьте аутентификацию RoAuth в ваше приложение за считанные минуты
- **Настраиваемые кнопки**: Полный контроль над внешним видом кнопок авторизации
- **Безопасная аутентификация**: Использование HMAC-SHA256 подписей для защиты от подделок
- **Popup-поддержка**: Аутентификация в отдельном окне без перезагрузки страницы
- **Многоязычность**: Поддержка английского и русского языков
- **Verbose logging**: Детальное логирование для отладки
- **Уведомления**: Встроенная система уведомлений для обратной связи с пользователем
- **Кросс-браузерная совместимость**: Работает во всех современных браузерах

### Архитектура

SDK состоит из следующих компонентов:

- **Core Engine**: Основной движок для инициации аутентификации
- **Button Factory**: Фабрика для создания кнопок с различными стилями
- **Message Handler**: Обработчик сообщений между popup и родительским окном
- **Notification System**: Система уведомлений для отображения статуса аутентификации
- **Security Module**: Модуль безопасности с HMAC-подписями
- **Verbose Logger**: Система детального логирования

## Установка

### Подключение SDK

SDK RoAuth поставляется как единый JavaScript файл. Подключите его к вашей HTML-странице:

```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Мое приложение</title>
    <!-- Подключение SDK RoAuth -->
    <script src="https://auth.roshkam.ru/public/authButton.js"></script>
</head>
<body>
    <!-- Контейнер для кнопки RoAuth -->
    <div class="roauth-container"></div>

    <script>
        // Ваш код инициализации
    </script>
</body>
</html>
```

### Версии SDK

Текущая версия SDK: **1.28.1** (лучше смотрите в релизах)

Версии SDK следуют семантическому версионированию (Semantic Versioning):

- **MAJOR**: Несовместимые изменения API
- **MINOR**: Добавление новой функциональности
- **PATCH**: Исправления багов и мелкие улучшения

### Системные требования

- **Браузеры**: Chrome 60+, Firefox 55+, Safari 12+, Edge 79+
- **JavaScript**: ES6+ поддержка
- **Протоколы**: HTTPS (обязательно для production)
- **API**: Web Crypto API для генерации подписей

## Быстрый старт

### Минимальная настройка

1. Создайте HTML-контейнер для кнопки:

```html
<div class="roauth-container"></div>
```

2. Подключите SDK и инициализируйте кнопку:

```javascript
// Инициализация кнопки RoAuth
window.RoAuth.createButton({
    clientId: 'your-client-id',
    redirectUrl: 'https://your-app.com/callback',
    permissions: 'email+username'
});
```

3. Обработайте callback на сервере для получения токенов.

### Полный пример

```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RoAuth Integration</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
        }
        .roauth-container {
            margin: 20px 0;
        }
    </style>
</head>
<body>
    <h1>Вход через RoAuth</h1>
    
    <div class="roauth-container"></div>
    
    <script src="https://auth.roshkam.ru/public/authButton.js"></script>
    <script>
        window.RoAuth.createButton({
            selector: '.roauth-container',
            clientId: 'demo-client-id',
            redirectUrl: 'https://your-app.com/auth/callback',
            permissions: 'email+username+profile',
            popup: true,
            onSuccess: function(data) {
                console.log('Успешная авторизация:', data);
                // Перенаправление или обработка данных
            },
            onError: function(error) {
                console.error('Ошибка авторизации:', error);
            },
            style: {
                text: "Войти через RoAuth",
                theme: 'light',
                buttonType: 'withText'
            }
        });
    </script>
</body>
</html>
```

## API Reference

### window.RoAuth.createButton

Создает и внедряет кнопку RoAuth в указанные контейнеры.

#### Синтаксис

```javascript
window.RoAuth.createButton(options)
```

#### Параметры

| Параметр | Тип | Обязательный | Описание |
|----------|-----|--------------|----------|
| `selector` | string | Нет | CSS-селектор контейнеров для кнопок. По умолчанию: `'.roauth-container'` |
| `clientId` | string | Да | Идентификатор клиента OAuth |
| `redirectUrl` | string | Да | URL для перенаправления после авторизации |
| `permissions` | string | Нет | Запрашиваемые разрешения. По умолчанию: `'email+username'` |
| `onSuccess` | function | Нет | Callback-функция при успешной авторизации |
| `onError` | function | Нет | Callback-функция при ошибке авторизации |
| `redirectAfterAuth` | boolean | Нет | Перенаправлять после авторизации. По умолчанию: `false` |
| `popup` | boolean | Нет | Использовать popup для авторизации. По умолчанию: `false` |
| `lang` | string | Нет | Язык интерфейса (`'en'` или `'ru'`) |
| `style` | object | Нет | Объект стилизации кнопки |

#### Возвращаемое значение

Объект с методом `update` для обновления конфигурации:

```javascript
{
    update: function(newConfig) {
        // Обновляет все кнопки с новой конфигурацией
    }
}
```

#### Примеры

```javascript
const buttonInstance = window.RoAuth.createButton({
    clientId: 'my-app-client',
    redirectUrl: 'https://myapp.com/callback',
    permissions: 'email+username+profile',
    onSuccess: (data) => {
        // data.code - авторизационный код
        // data.state - состояние для защиты от CSRF
    },
    onError: (error) => {
        console.error('Auth failed:', error);
    }
});

// Позже обновить конфигурацию
buttonInstance.update({
    permissions: 'email+username+profile+openid'
});
```

### window.roAuthVerbose

Управляет verbose-логированием SDK.

#### Синтаксис

```javascript
window.roAuthVerbose(command)
```

#### Параметры

| Параметр | Тип | Описание |
|----------|-----|----------|
| `command` | string/boolean | Команда или состояние логирования |

#### Команды

- `'on'` или `true`: Включить логирование
- `'off'` или `false`: Выключить логирование
- `'toggle'`: Переключить состояние
- Без параметров или `true`: Включить логирование

#### Примеры

```javascript
// Включить verbose логирование
window.roAuthVerbose('on');

// Выключить
window.roAuthVerbose('off');

// Переключить
window.roAuthVerbose('toggle');

// Использовать как функцию
window.roAuthVerboseOn();
window.roAuthVerboseOff();
```

### window.roAuthShowNotification

Отображает уведомление пользователю.

#### Синтаксис

```javascript
window.roAuthShowNotification(type, text, options)
```

#### Параметры

| Параметр | Тип | Описание |
|----------|-----|----------|
| `type` | string | Тип уведомления: `'success'` или `'error'` |
| `text` | string | Текст уведомления |
| `options` | object | Дополнительные опции |

#### Опции уведомлений

```javascript
{
    duration: 5000  // Длительность в миллисекундах
}
```

#### Примеры

```javascript
// Успешное уведомление
window.roAuthShowNotification('success', 'Авторизация успешна!');

// Ошибка с кастомной длительностью
window.roAuthShowNotification('error', 'Ошибка сети', { duration: 10000 });
```

### window.roAuthResetSuppress

Сбрасывает подавление ошибок.

#### Синтаксис

```javascript
window.roAuthResetSuppress()
```

#### Описание

После успешной авторизации SDK автоматически подавляет отображение ошибок. Эта функция позволяет сбросить это состояние.

### window.roAuthTestNotify

Тестовая функция для уведомлений (для разработчиков).

#### Синтаксис

```javascript
window.roAuthTestNotify(type, text, durationOrOptions)
```

## Конфигурация

### Основные параметры

#### clientId

Идентификатор OAuth-клиента. Получается при регистрации приложения в RoAuth.

```javascript
clientId: 'your-unique-client-id'
```

#### redirectUrl

URL, на который будет перенаправлен пользователь после авторизации. Должен соответствовать зарегистрированному в RoAuth.

```javascript
redirectUrl: 'https://your-app.com/auth/callback'
```

#### permissions

Запрашиваемые разрешения. Перечисляются через `+` или `,`.

```javascript
permissions: 'email+username+profile+openid'
```

Доступные разрешения:
- `email`: Доступ к email пользователя
- `username`: Доступ к имени пользователя
- `profile`: Доступ к профилю пользователя
- `openid`: OpenID Connect claims
- `offline_access`: Доступ для refresh tokens

#### popup

Включает popup-режим авторизации.

```javascript
popup: true  // По умолчанию: false
```

В popup-режиме:
- Аутентификация происходит в отдельном окне
- Родительская страница остается активной
- Результат передается через postMessage API

#### redirectAfterAuth

Автоматическое перенаправление после успешной авторизации.

```javascript
redirectAfterAuth: true  // По умолчанию: false
```

### Стилизация

SDK предоставляет широкие возможности для кастомизации внешнего вида кнопок.

#### Основные стили

```javascript
style: {
    text: "Войти через RoAuth",        // Текст кнопки
    height: "auto",                    // Высота
    width: "auto",                     // Ширина
    margin: 8,                         // Отступы
    paddingX: 16,                      // Горизонтальный padding
    paddingY: 10,                      // Вертикальный padding
    fontSize: 16,                      // Размер шрифта
    borderRadius: 4,                   // Радиус скругления
    buttonType: "withText",            // Тип кнопки
    textPosition: "textRight",         // Позиция текста
    iconType: "svg",                   // Тип иконки
    theme: "light",                    // Тема
    customColors: {                    // Кастомные цвета
        background: "#4285F4",
        text: "#FFFFFF",
        hover: "#3367D6"
    }
}
```

#### Типы кнопок (buttonType)

- `"textOnly"`: Только текст
- `"iconOnly"`: Только иконка
- `"withText"`: Иконка + текст

#### Позиции текста (textPosition)

- `"textLeft"`: Текст слева от иконки
- `"textRight"`: Текст справа от иконки

#### Типы иконок (iconType)

- `"svg"`: SVG-иконка (рекомендуется)
- `"png"`: PNG-иконка

### Темы

SDK включает предустановленные темы для быстрой стилизации.

#### Доступные темы

```javascript
// Светлая тема (по умолчанию)
theme: "light"

// Темная тема
theme: "dark"

// Кастомная тема
theme: "custom"
```

#### Цвета тем

**Light тема:**
```javascript
{
    background: "#4285F4",  // Синий фон
    text: "#FFFFFF",       // Белый текст
    hover: "#3367D6"       // Темно-синий при наведении
}
```

**Dark тема:**
```javascript
{
    background: "#3367D6",  // Темно-синий фон
    text: "#FFFFFF",       // Белый текст
    hover: "#5C9AFF"       // Светло-синий при наведении
}
```

#### Кастомные цвета

При использовании `theme: "custom"` определите свои цвета:

```javascript
customColors: {
    background: "#FF6B35",  // Оранжевый
    text: "#FFFFFF",       // Белый текст
    hover: "#E55A2B"       // Темно-оранжевый
}
```

### Локализация

SDK поддерживает английский и русский языки.

#### Автоматическое определение языка

```javascript
// Автоматически на основе браузера
lang: undefined  // По умолчанию
```

#### Явное указание языка

```javascript
// Английский
lang: 'en'

// Русский
lang: 'ru'
```

#### Локализованные сообщения

- **Успешная авторизация**: "Authorization successful" / "Авторизация успешна"
- **Ошибка авторизации**: "Auth failed" / "Ошибка авторизации"
- **Прервана**: "Authorization cancelled" / "Авторизация прервана"
- **Таймаут**: "Authorization timeout" / "Таймаут авторизации"

## Примеры использования

### Базовый пример

```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Простой пример RoAuth</title>
</head>
<body>
    <h1>Вход в систему</h1>
    <div class="roauth-container"></div>

    <script src="https://auth.roshkam.ru/public/authButton.js"></script>
    <script>
        window.RoAuth.createButton({
            clientId: 'demo-app-123',
            redirectUrl: 'https://demo-app.com/callback',
            permissions: 'email+username'
        });
    </script>
</body>
</html>
```

### Расширенная конфигурация

```javascript
window.RoAuth.createButton({
    selector: '.auth-buttons',
    clientId: 'advanced-app-456',
    redirectUrl: 'https://advanced-app.com/oauth/callback',
    permissions: 'email+username+profile+openid',
    popup: true,
    lang: 'ru',
    redirectAfterAuth: false,
    onSuccess: function(data) {
        console.log('Получен код авторизации:', data.code);
        console.log('State:', data.state);
        
        // Отправка кода на сервер для обмена на токены
        fetch('/api/auth/token', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({
                code: data.code,
                state: data.state
            })
        })
        .then(response => response.json())
        .then(tokens => {
            // Сохранение токенов
            localStorage.setItem('access_token', tokens.access_token);
            localStorage.setItem('refresh_token', tokens.refresh_token);
            
            // Перенаправление на защищенную страницу
            window.location.href = '/dashboard';
        })
        .catch(error => {
            console.error('Ошибка обмена токенов:', error);
        });
    },
    onError: function(error) {
        console.error('Ошибка авторизации:', error);
        
        // Показать пользователю понятное сообщение
        alert('Не удалось войти. Попробуйте еще раз.');
    },
    style: {
        text: "Авторизоваться через RoAuth",
        theme: 'dark',
        buttonType: 'withText',
        textPosition: 'textLeft',
        fontSize: 18,
        borderRadius: 8,
        paddingX: 20,
        paddingY: 12
    }
});
```

### Обработка событий

```javascript
const authButton = window.RoAuth.createButton({
    clientId: 'event-demo',
    redirectUrl: 'https://demo.com/callback',
    onSuccess: function(data) {
        // Успешная авторизация
        trackEvent('auth_success', {
            method: 'roauth',
            timestamp: Date.now()
        });
        
        // Обновление UI
        document.getElementById('user-status').textContent = 'Вход выполнен';
        document.getElementById('logout-btn').style.display = 'block';
    },
    onError: function(error) {
        // Обработка ошибок
        trackEvent('auth_error', {
            error_type: error.error,
            timestamp: Date.now()
        });
        
        // Показать соответствующее сообщение
        switch(error.error) {
            case 'popup_closed':
                showMessage('Авторизация была прервана');
                break;
            case 'invalid_state':
                showMessage('Ошибка безопасности. Попробуйте еще раз');
                break;
            default:
                showMessage('Произошла ошибка. Попробуйте позже');
        }
    }
});

// Функция отслеживания событий (например, Google Analytics)
function trackEvent(eventName, params) {
    if (typeof gtag !== 'undefined') {
        gtag('event', eventName, params);
    }
}

// Функция показа сообщений
function showMessage(text) {
    const messageEl = document.createElement('div');
    messageEl.className = 'auth-message error';
    messageEl.textContent = text;
    document.body.appendChild(messageEl);
    
    setTimeout(() => {
        messageEl.remove();
    }, 5000);
}
```

### Пользовательские стили

```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Кастомные стили RoAuth</title>
    <style>
        .custom-roauth-btn {
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
            transition: all 0.3s ease;
        }
        
        .custom-roauth-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(0,0,0,0.15);
        }
        
        .auth-container {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 200px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-radius: 12px;
            margin: 20px 0;
        }
    </style>
</head>
<body>
    <div class="auth-container">
        <div class="roauth-custom"></div>
    </div>

    <script src="https://auth.roshkam.ru/public/authButton.js"></script>
    <script>
        window.RoAuth.createButton({
            selector: '.roauth-custom',
            clientId: 'styled-app',
            redirectUrl: 'https://styled-app.com/callback',
            style: {
                text: "🚀 Войти через RoAuth",
                theme: 'custom',
                customColors: {
                    background: 'linear-gradient(45deg, #FF6B35, #F7931E)',
                    text: '#FFFFFF',
                    hover: 'linear-gradient(45deg, #E55A2B, #E8871A)'
                },
                fontSize: 20,
                paddingX: 24,
                paddingY: 14,
                borderRadius: 25,
                buttonType: 'withText',
                iconType: 'svg'
            },
            onSuccess: function(data) {
                // Анимация успеха
                const button = document.querySelector('.roauth-custom button');
                button.style.background = 'linear-gradient(45deg, #4CAF50, #45a049)';
                button.textContent = '✓ Успешно!';
                
                setTimeout(() => {
                    window.location.href = '/welcome';
                }, 1500);
            }
        });
        
        // Дополнительная стилизация после создания кнопки
        setTimeout(() => {
            const button = document.querySelector('.roauth-custom button');
            if (button) {
                button.classList.add('custom-roauth-btn');
            }
        }, 100);
    </script>
</body>
</html>
```

### Интеграция с фреймворками

#### React

```jsx
import React, { useEffect } from 'react';

function RoAuthButton({ config }) {
    useEffect(() => {
        if (window.RoAuth && window.RoAuth.createButton) {
            window.RoAuth.createButton({
                selector: '.roauth-react-container',
                ...config
            });
        }
    }, [config]);

    return <div className="roauth-react-container"></div>;
}

// Использование
function App() {
    const roAuthConfig = {
        clientId: 'react-app-123',
        redirectUrl: 'https://react-app.com/auth',
        permissions: 'email+username+profile',
        popup: true,
        onSuccess: (data) => {
            // Обработка в React
            console.log('React auth success:', data);
        }
    };

    return (
        <div className="App">
            <h1>React + RoAuth</h1>
            <RoAuthButton config={roAuthConfig} />
        </div>
    );
}
```

#### Vue.js

```vue
<template>
  <div class="vue-app">
    <h1>Vue + RoAuth</h1>
    <div class="roauth-vue-container"></div>
  </div>
</template>

<script>
export default {
  name: 'App',
  mounted() {
    this.initRoAuth();
  },
  methods: {
    initRoAuth() {
      if (window.RoAuth && window.RoAuth.createButton) {
        window.RoAuth.createButton({
          selector: '.roauth-vue-container',
          clientId: 'vue-app-456',
          redirectUrl: 'https://vue-app.com/auth',
          permissions: 'email+username',
          popup: true,
          onSuccess: this.handleAuthSuccess,
          onError: this.handleAuthError
        });
      }
    },
    handleAuthSuccess(data) {
      this.$emit('auth-success', data);
      // Vue логика
    },
    handleAuthError(error) {
      this.$emit('auth-error', error);
      // Vue логика
    }
  }
}
</script>
```

#### Angular

```typescript
import { Component, OnInit } from '@angular/core';

declare var window: any;

@Component({
  selector: 'app-roauth-button',
  template: '<div class="roauth-angular-container"></div>'
})
export class RoAuthButtonComponent implements OnInit {

  ngOnInit() {
    this.initRoAuth();
  }

  private initRoAuth() {
    if (window.RoAuth && window.RoAuth.createButton) {
      window.RoAuth.createButton({
        selector: '.roauth-angular-container',
        clientId: 'angular-app-789',
        redirectUrl: 'https://angular-app.com/auth',
        permissions: 'email+username+profile',
        popup: true,
        onSuccess: (data: any) => {
          this.handleAuthSuccess(data);
        },
        onError: (error: any) => {
          this.handleAuthError(error);
        }
      });
    }
  }

  private handleAuthSuccess(data: any) {
    console.log('Angular auth success:', data);
    // Angular логика
  }

  private handleAuthError(error: any) {
    console.error('Angular auth error:', error);
    // Angular логика
  }
}
```

## Безопасность

### HMAC-SHA256 Подписи

SDK использует HMAC-SHA256 для подписи запросов авторизации, обеспечивая защиту от подделок.

```javascript
// Пример генерации подписи
async function generateSignature(params, secret) {
    const stringToSign = Object.entries(params)
        .sort(([a], [b]) => a.localeCompare(b))
        .map(([key, value]) => `${key}=${value}`)
        .join('&');
    
    const encoder = new TextEncoder();
    const keyData = encoder.encode(secret);
    const data = encoder.encode(stringToSign);
    
    const cryptoKey = await crypto.subtle.importKey(
        'raw',
        keyData,
        { name: 'HMAC', hash: 'SHA-256' },
        false,
        ['sign']
    );
    
    const signature = await crypto.subtle.sign('HMAC', cryptoKey, data);
    return Array.from(new Uint8Array(signature))
        .map(b => b.toString(16).padStart(2, '0'))
        .join('');
}
```

### Защита от CSRF

SDK генерирует уникальный `state` параметр для каждой авторизации и проверяет его при получении ответа.

### HTTPS Требования

Для production-среды обязательно использование HTTPS протокола.

### Валидация URL

SDK проверяет корректность `redirectUrl`:

```javascript
try {
    const url = new URL(config.redirectUrl);
    if (url.protocol !== 'http:' && url.protocol !== 'https:') {
        throw new Error('Invalid protocol');
    }
} catch (e) {
    console.error('Invalid redirectUrl format');
}
```

### Безопасные Headers

Рекомендуемые headers для страницы с SDK:

```
Content-Security-Policy: default-src 'self'; script-src 'self' https://auth.roshkam.ru; connect-src 'self' https://auth.roshkam.ru
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
```

## Troubleshooting

### Распространенные проблемы

#### Кнопка не отображается

**Симптомы:**
- Контейнер пустой
- JavaScript ошибки в консоли

**Решения:**
1. Проверьте подключение SDK файла
2. Убедитесь в наличии контейнера с правильным селектором
3. Проверьте валидность `clientId` и `redirectUrl`

```javascript
// Проверка загрузки SDK
if (typeof window.RoAuth === 'undefined') {
    console.error('RoAuth SDK not loaded');
}
```

#### Popup блокируется браузером

**Симптомы:**
- Popup не открывается
- Браузер показывает предупреждение

**Решения:**
1. Включите popup в настройках браузера
2. Добавьте обработчик для случаев блокировки

```javascript
// Проверка поддержки popup
function checkPopupSupport() {
    const testPopup = window.open('', '_blank', 'width=1,height=1');
    if (!testPopup) {
        alert('Popup заблокирован браузером. Разрешите popup для этого сайта.');
        return false;
    }
    testPopup.close();
    return true;
}
```

#### Ошибка "Invalid state"

**Симптомы:**
- Авторизация завершается с ошибкой `invalid_state`

**Решения:**
1. Проверьте, что страница не кэшируется
2. Убедитесь, что `localStorage` доступен
3. Проверьте отсутствие конфликтов с другими SDK

#### Проблемы с CORS

**Симптомы:**
- Ошибки CORS в консоли

**Решения:**
1. Убедитесь, что `redirectUrl` соответствует зарегистрированному
2. Проверьте настройки CORS на сервере

### Отладка

#### Включение verbose логирования

```javascript
// Включить детальное логирование
window.roAuthVerbose('on');

// Просмотр логов в консоли
// SDK будет логировать все действия
```

#### Проверка конфигурации

```javascript
// Вывод текущей конфигурации
console.log('RoAuth config:', {
    version: window.RoAuth ? window.RoAuth.version : 'unknown',
    verbose: window.RoAuth ? window.RoAuth._verbose : false,
    suppressErrors: window.RoAuth ? window.RoAuth._suppressErrors : false
});
```

#### Мониторинг сообщений

```javascript
// Прослушивание всех сообщений RoAuth
window.addEventListener('message', function(event) {
    if (event.origin.includes('auth.roshkam.ru')) {
        console.log('RoAuth message:', event.data);
    }
});
```

### Производительность

#### Оптимизация загрузки

```html
<!-- Асинхронная загрузка SDK -->
<script>
    (function(d, s, id) {
        var js, fjs = d.getElementsByTagName(s)[0];
        if (d.getElementById(id)) return;
        js = d.createElement(s); js.id = id;
        js.src = 'https://auth.roshkam.ru/public/authButton.js';
        fjs.parentNode.insertBefore(js, fjs);
    }(document, 'script', 'roauth-sdk'));
</script>
```

#### Кэширование

SDK автоматически кэшируется браузером. Для принудительного обновления:

```html
<script src="https://auth.roshkam.ru/public/authButton.js?v=1.28.1"></script>
```

### Поддержка браузеров

#### Совместимые браузеры

- Chrome 60+
- Firefox 55+
- Safari 12+
- Edge 79+
- Opera 47+

#### Fallback для старых браузеров

```javascript
// Проверка поддержки Web Crypto API
if (typeof window.crypto === 'undefined' || 
    typeof window.crypto.subtle === 'undefined') {
    console.warn('Web Crypto API not supported. Using fallback.');
    // Fallback логика
}
```

## Changelog

### Версия 1.28.1 (Текущая)

**Исправления:**
- Улучшена обработка ошибок popup
- Исправлены проблемы с локализацией
- Оптимизирована производительность уведомлений

**Новые возможности:**
- Добавлена функция `roAuthTestNotify` для тестирования
- Улучшен verbose logging

### Версия 1.27.0

**Новые возможности:**
- Поддержка темной темы
- Кастомные цвета для кнопок
- Улучшенная система уведомлений

### Версия 1.26.0

**Новые возможности:**
- Popup-режим авторизации
- HMAC-SHA256 подписи
- Защита от CSRF

### Версия 1.25.0

**Новые возможности:**
- Базовая функциональность SDK
- Создание кнопок авторизации
- Callback обработчики

## Лицензия

RoAuth SDK распространяется под лицензией MIT.

```
MIT License

Copyright (c) 2024 RoSHkam

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

**Контакты поддержки:**
- Email: support@roshkam.ru
- Сайт: https://support.roshkam.ru
- Документация: https://docs.roshkam.ru/roauth-sdk

**GitHub:** https://github.com/roshkam/roshkam-gatekeeper

---

*Эта документация была автоматически сгенерирована для версии 1.28.1 RoAuth SDK.*
