# PropVault Bot

Telegram-бот для платформы инвестиций в российскую недвижимость.

## Структура

```
propvault_bot/
├── main.py              # точка входа
├── config.py            # переменные окружения
├── database.py          # SQLite: пользователи, подписки, объекты
├── keyboards.py         # все клавиатуры
├── utils.py             # уведомления администраторам
├── requirements.txt
├── .env.example
└── handlers/
    ├── common.py        # /start, выбор роли
    ├── investor.py      # экраны инвестора
    ├── realtor.py       # подписка, добавление объектов (FSM)
    └── admin.py         # очередь модерации, статистика
```

## Быстрый старт

### 1. Создайте бота

Напишите [@BotFather](https://t.me/BotFather):
```
/newbot
```
Получите `BOT_TOKEN`.

### 2. Узнайте свой Telegram ID

Напишите [@userinfobot](https://t.me/userinfobot) — он покажет ваш числовой ID.

### 3. Настройте окружение

```bash
cp .env.example .env
```

Отредактируйте `.env`:
```
BOT_TOKEN=7123456789:AAH...ваш_токен...
ADMIN_IDS=123456789          # ваш Telegram ID (модератор)
WEBAPP_URL=https://your-app.vercel.app  # URL задеплоенного propvault.html
```

### 4. Установите зависимости

```bash
pip install -r requirements.txt
```

### 5. Запустите бота

```bash
python main.py
```

### 6. Подключите Mini App в BotFather

```
/setmenubutton → выберите бота → введите URL → "Открыть PropVault"
```

---

## Как работает

### Инвестор (`role: investor`)
- Регистрируется бесплатно
- Получает кнопку-меню для открытия Mini App
- Видит все одобренные объекты, нажимает «Показать контакт»

### Риелтор (`role: realtor`)
- Выбирает тариф (Старт / Pro / Elite)
- Добавляет объекты через FSM-диалог (10 шагов)
- Получает уведомление об одобрении / отклонении

### Модератор (ID в `ADMIN_IDS`)
- Получает уведомление о каждом новом объекте
- Одобряет или отклоняет с указанием причины
- Видит статистику платформы

---

## Тарифы риелторов

| Тариф | Цена | Объектов |
|-------|------|----------|
| Старт | ₽1 900/мес | 1 |
| Pro   | ₽4 900/мес | 5 |
| Elite | ₽12 900/мес | безлимит |

> ⚠️ Оплата сейчас имитируется. Для продакшна подключите **ЮKassa** или **Telegram Stars** — напишите, добавлю.

---

## Продакшн-деплой (Railway / VPS)

```bash
# Railway
railway init
railway up

# VPS (systemd)
# создайте /etc/systemd/system/propvault.service
# и запустите: systemctl enable --now propvault
```
