 🚀 RoAuth SDK

<div align="center">

![RoAuth Logo](https://raw.githubusercontent.com/RomanKarb/RoAuth/main/assets/favicon.ico)

**Мощный JavaScript SDK для простой и безопасной интеграции RoAuth аутентификации**

[![Version](https://img.shields.io/badge/version-1.28.1-blue.svg)](https://github.com/RomanKarb/RoAuth)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Browser Support](https://img.shields.io/badge/browser-Chrome%20%7C%20Firefox%20%7C%20Safari%20%7C%20Edge-green.svg)](#системные-требования)

[📖 Полная документация](./docs/RoAuth_SDK_Documentation.md) • [🌐 Примеры](#примеры) • [💬 Поддержка](https://github.com/RomanKarb/RoAuth/issues)

</div>

---

## ✨ Что такое RoAuth SDK?

RoAuth SDK - это легкая и удобная библиотека для интеграции авторизации через RoAuth в ваши веб-приложения. Позволяет быстро добавить вход пользователей, управление сессиями и взаимодействие с API без лишней боли.

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
<script src="https://cdn.jsdelivr.net/gh/RomanKarb/RoAuth/dist/authButton.js"></script>
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

    <script src="https://cdn.jsdelivr.net/gh/RomanKarb/RoAuth/dist/authButton.js"></script>
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
# Скачайте SDK файл с GitHub Releases
wget https://github.com/RomanKarb/RoAuth/releases/download/v1.28.1/authButton.js

# Или через curl
curl -O https://github.com/RomanKarb/RoAuth/releases/download/v1.28.1/authButton.js
```

### npm (если опубликовано)

```bash
npm install roauth-sdk
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

📖 **[Полная документация](./docs/RoAuth_SDK_Documentation.md)** содержит:

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

1. 📖 Проверьте [документацию](./docs/RoAuth_SDK_Documentation.md)
2. 🔍 Включите verbose logging: `window.roAuthVerbose('on')`
3. 💬 Создайте [issue](https://github.com/RomanKarb/RoAuth/issues)
4. 📝 Проверьте [discussions](https://github.com/RomanKarb/RoAuth/discussions)

---

## 📈 Статистика

[![GitHub stars](https://img.shields.io/github/stars/RomanKarb/RoAuth?style=social)](https://github.com/RomanKarb/RoAuth/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/RomanKarb/RoAuth?style=social)](https://github.com/RomanKarb/RoAuth/network/members)

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

- **GitHub**: https://github.com/RomanKarb/RoAuth
- **Issues**: https://github.com/RomanKarb/RoAuth/issues
- **Discussions**: https://github.com/RomanKarb/RoAuth/discussions
- **Служба поддержки**: https://support.roshkam.ru
- **Telegram**: https://t.me/roshkam

---

<div align="center">

**Разработано с ❤️**

[🌟 Поставьте звезду](https://github.com/RomanKarb/RoAuth) если проект вам понравился!

</div>

## API Rate Limiting

- `login` и `authorize`: максимум 7 запросов за 60 секунд на один IP клиента
- `validate` и `verify-token`: максимум 15 запросов за 60 секунд на один IP клиента

При превышении лимита API возвращает HTTP `429` с JSON:

```json
{
	"success": false,
	"error": "rate_limit_exceeded",
	"message": "rate_limit_exceeded Retry after 42 seconds"
}
```

Дополнительно вызов `errorResponse` включает количество секунд для повтора в качестве 4-го параметра, где применимо.
