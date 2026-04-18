# 🚀 RoAuth SDK

<div align="center">

![RoAuth Logo](https://auth.roshkam.ru/favicon.ico)

**Мощный JavaScript SDK для простой и безопасной интеграции RoAuth аутентификации**

[![Version](https://img.shields.io/badge/version-1.28.1-blue.svg)](https://github.com/roshkam/roshkam-gatekeeper)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Browser Support](https://img.shields.io/badge/browser-Chrome%20%7C%20Firefox%20%7C%20Safari%20%7C%20Edge-green.svg)](#системные-требования)

[📖 Полная документация](RoAuth_SDK_Documentation.md) • [🌐 Демо](https://auth.roshkam.ru/demo) • [💬 Поддержка](https://support.roshkam.ru)

</div>

---

## ✨ Что такое RoAuth SDK?

RoAuth SDK - это современный JavaScript SDK, который позволяет интегрировать аутентификацию через платформу RoAuth в ваши веб-приложения всего за несколько минут. Создавайте стильные кнопки авторизации с поддержкой popup-окон, кастомными темами и полной безопасностью.

### 🎯 Ключевые возможности

- 🔐 **Безопасная аутентификация** с HMAC-SHA256 подписями
- 🎨 **Полная кастомизация** внешнего вида кнопок
- 🪟 **Popup-режим** без перезагрузки страницы
- 🌍 **Многоязычность** (Русский/English)
- 📱 **Адаптивный дизайн** для всех устройств
- 🛠️ **Verbose logging** для отладки
- 🔔 **Встроенные уведомления** для обратной связи
- 🔄 **Кросс-браузерная поддержка**

---

## 🚀 Быстрый старт

### 1. Подключите SDK

```html
<script src="https://auth.roshkam.ru/public/authButton.js"></script>
```

### 2. Создайте контейнер

```html
<div class="roauth-container"></div>
```

### 3. Инициализируйте кнопку

```javascript
window.RoAuth.createButton({
    clientId: 'your-client-id',
    redirectUrl: 'https://your-app.com/callback',
    permissions: 'email+username'
});
```

### 🎉 Готово! Ваша кнопка RoAuth готова к работе.

---

## 📦 Установка

### CDN (Рекомендуется)

```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Мое приложение</title>
</head>
<body>
    <div class="roauth-container"></div>

    <script src="https://auth.roshkam.ru/public/authButton.js"></script>
    <script>
        window.RoAuth.createButton({
            clientId: 'your-client-id',
            redirectUrl: 'https://your-app.com/callback'
        });
    </script>
</body>
</html>
```

### Скачивание файла

```bash
# Скачайте SDK файл
wget https://auth.roshkam.ru/public/authButton.js

# Или через curl
curl -O https://auth.roshkam.ru/public/authButton.js
```

---

## 🎨 Примеры

### Базовая кнопка

```javascript
window.RoAuth.createButton({
    clientId: 'demo-app',
    redirectUrl: 'https://demo-app.com/callback',
    permissions: 'email+username'
});
```

### С кастомными стилями

```javascript
window.RoAuth.createButton({
    clientId: 'styled-app',
    redirectUrl: 'https://styled-app.com/callback',
    style: {
        text: "🚀 Войти через RoAuth",
        theme: 'dark',
        borderRadius: 8,
        fontSize: 18
    },
    onSuccess: (data) => {
        console.log('Успешная авторизация!', data);
    }
});
```

### С popup-режимом

```javascript
window.RoAuth.createButton({
    clientId: 'popup-app',
    redirectUrl: 'https://popup-app.com/callback',
    popup: true,
    permissions: 'email+username+profile'
});
```

---

## 🔧 Конфигурация

| Параметр | Тип | Обязательный | Описание |
|----------|-----|--------------|----------|
| `clientId` | string | ✅ | Идентификатор OAuth-клиента |
| `redirectUrl` | string | ✅ | URL для перенаправления после авторизации |
| `permissions` | string | ❌ | Запрашиваемые разрешения (`email+username`) |
| `popup` | boolean | ❌ | Включить popup-режим |
| `style` | object | ❌ | Настройки внешнего вида |
| `onSuccess` | function | ❌ | Callback при успешной авторизации |
| `onError` | function | ❌ | Callback при ошибке |

### Темы

- 🌞 **Light** (по умолчанию)
- 🌙 **Dark**
- 🎨 **Custom** (свои цвета)

---

## 📚 Документация

📖 **[Полная документация](RoAuth_SDK_Documentation.md)** содержит:

- Детальное API описание
- Расширенные примеры использования
- Руководство по безопасности
- Troubleshooting
- Changelog

---

## 🛡️ Безопасность

- 🔐 HMAC-SHA256 подписи для защиты от подделок
- 🛡️ CSRF-защита с state параметрами
- 🔒 HTTPS обязательный для production
- ✅ Валидация всех URL и параметров

---

## 🌐 Системные требования

- **Браузеры**: Chrome 60+, Firefox 55+, Safari 12+, Edge 79+
- **JavaScript**: ES6+ поддержка
- **Протокол**: HTTPS (production)
- **API**: Web Crypto API

---

## 🐛 Проблемы и поддержка

Если у вас возникли проблемы:

1. 📖 Проверьте [документацию](RoAuth_SDK_Documentation.md)
2. 🔍 Включите verbose logging: `window.roAuthVerbose('on')`
3. 💬 Обратитесь в [поддержку](https://support.roshkam.ru)
4. 🐛 Создайте [issue](https://github.com/roshkam/roshkam-gatekeeper/issues)

---

## 📈 Статистика

[![GitHub stars](https://img.shields.io/github/stars/roshkam/roshkam-gatekeeper?style=social)](https://github.com/roshkam/roshkam-gatekeeper/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/roshkam/roshkam-gatekeeper?style=social)](https://github.com/roshkam/roshkam-gatekeeper/network/members)

---

## 📄 Лицензия

Этот проект распространяется под лицензией MIT - см. файл [LICENSE](LICENSE) для деталей.

---

## 🤝 Вклад в проект

Мы приветствуем вклад! Пожалуйста:

1. Форкните репозиторий
2. Создайте feature branch (`git checkout -b feature/AmazingFeature`)
3. Зафиксируйте изменения (`git commit -m 'Add some AmazingFeature'`)
4. Отправьте в branch (`git push origin feature/AmazingFeature`)
5. Откройте Pull Request

---

## 📞 Контакты

- **Email**: support@roshkam.ru
- **Сайт**: https://roshkam.ru
- **Поддержка**: https://support.roshkam.ru
- **Telegram**: [@roshkam_support](https://t.me/roshkam_support)

---

<div align="center">

**Создано с ❤️ командой RoSHkam**

[🌟 Поставьте звезду](https://github.com/roshkam/roshkam-gatekeeper) если проект вам понравился!

</div>

- `login` and `authorize`: max 7 requests per 60 seconds per client IP
- `validate` and `verify-token`: max 15 requests per 60 seconds per client IP

When a client exceeds the limit the API returns HTTP `429` with JSON:

```json
{
	"success": false,
	"error": "rate_limit_exceeded",
	"message": "rate_limit_exceeded Retry after 42 seconds"
}
```

Additionally the `errorResponse` call includes the retry seconds as the 4th parameter where applicable.

