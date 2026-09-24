# 🎙️ n8n Голосовой дневник

Автоматизация для Telegram: превращает голосовые сообщения в аккуратные посты для личного канала-дневника.

## ✨ Как это работает

1. Вы записываете голосовое сообщение боту в Telegram.
2. n8n скачивает аудио и отправляет его в Groq Whisper для расшифровки.
3. Текст проходит через нейросеть OpenRouter (модель Nemotron 3.5 Lightning), которая:
   - сохраняет ваш авторский стиль и эмоции,
   - исправляет ошибки и убирает слова-паразиты,
   - структурирует текст (абзацы, списки, эмодзи).
4. Бот присылает вам готовый черновик с кнопками «✅ Опубликовать» и «✏️ Переписать».
5. По нажатию кнопки пост публикуется в ваш Telegram-канал.

## 🏗️ Схема workflow
Telegram Trigger → Switch (голос?) → Get a file → Code (rename .oga → .ogg)
↓
HTTP Request (Groq Whisper)
↓
Basic LLM Chain (редактура)
↓
HTTP Request1 (черновик + кнопки)
↓
Telegram Trigger1 (callback)
↓
Switch1 (publish?) → HTTP Request2 (публикация) 
## 🧰 Что нужно для работы

| Сервис | Зачем | Где взять |
|---|---|---|
| Telegram Bot | Приём голосовых и публикация | [@BotFather](https://t.me/BotFather) |
| Groq API | Расшифровка голоса (Whisper) | [console.groq.com/keys](https://console.groq.com/keys) |
| OpenRouter API | Обработка текста нейросетью | [openrouter.ai/keys](https://openrouter.ai/keys) |

## ⚙️ Установка

1. **Импорт workflow**: в n8n → Workflows → Import from File → выбрать `workflow_sanitized.json`.

2. **Заменить заглушки** в узлах:
   - `<TELEGRAM_BOT_TOKEN>` — токен вашего бота (в URL двух HTTP Request).
   - `<YOUR_PERSONAL_ID>` — ваш личный ID (узнать у [@userinfobot](https://t.me/userinfobot)).
   - `<YOUR_CHANNEL_ID>` — ID вашего канала (начинается с `-100...`).

3. **Создать учётные данные в n8n**:
   - **Telegram API** — токен бота.
   - **Header Auth** (для Groq) — Name: `Authorization`, Value: `Bearer ВАШ_КЛЮЧ_GROQ`.
   - **OpenRouter** — API-ключ.

4. **Добавить бота в канал** как администратора с правом «Публикация сообщений».

5. **Активировать workflow** — кнопка **Publish** в правом верхнем углу.

## 📋 Требования

- n8n (self-hosted или cloud)
- Community node `@mentoster/n8n-nodes-telegram-polling` (устанавливается через Settings → Community Nodes)
- Telegram-бот
- Аккаунты Groq и OpenRouter (бесплатных лимитов достаточно)

## 💰 Бесплатные лимиты

| Сервис | Лимит |
|---|---|
| Groq Whisper | 14 400 запросов/день |
| OpenRouter | 50 запросов/день (для `:free` моделей) |
| n8n (self-hosted) | Без ограничений |

## 📄 Структура

- `workflow_sanitized.json` — экспорт n8n workflow (без секретных данных).

## ⚠️ Безопасность

Все секретные ключи и персональные ID **не хранятся в репозитории**. Перед публикацией они были заменены на заглушки вида `<TELEGRAM_BOT_TOKEN>`. При импорте вы вводите свои данные заново.

## 📝 Лицензия

MIT (или другая по вашему выбору) Update README with full documentation
