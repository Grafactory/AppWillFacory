# 📖 **AppWill Content Factory — Инструкция по установке и запуску**

## 🚀 1. Описание

Этот workflow в **n8n** автоматизирует процесс создания рекламных видео для приложений на AppWill.co:

* Получает данные о приложении через **Telegram Bot**
* Сохраняет и обновляет их в **Google Sheets**
* Генерирует сценарий, описание и хэштеги через **OpenAI GPT-4**
* Создаёт вертикальное видео с помощью **Veo3 API**
* Автопубликует ролик в **Instagram Reels** и **TikTok**
* Отправляет уведомления в **Telegram**

---

## 📌 2. Подготовка окружения

### **2.1. Переменные окружения (Environment Variables)**

Добавь в n8n (Settings → Environment Variables):

```
TELEGRAM_BOT_TOKEN=твой_telegram_bot_token
TELEGRAM_CHAT_ID=твой_telegram_chat_id

GOOGLE_SHEETS_ID=id_твоей_google_таблицы
GOOGLE_DRIVE_FOLDER_ID=id_папки_в_google_drive

OPENAI_API_KEY=твой_openai_api_key
YOUTUBE_API_KEY=твой_youtube_api_key
VEO3_API_KEY=твой_veo3_api_key

FB_PAGE_ACCESS_TOKEN=твой_facebook_page_access_token
IG_USER_ID=твой_instagram_user_id

TIKTOK_CLIENT_KEY=твой_tiktok_client_key
TIKTOK_CLIENT_SECRET=твой_tiktok_client_secret
TIKTOK_ACCESS_TOKEN=твой_tiktok_access_token
```

---

### **2.2. Подключение OAuth Credentials**

В **n8n → Credentials** создай:

* **Google Sheets OAuth2** (доступ к таблице)
* **Google Drive OAuth2** (доступ к папке для видео)
* Свяжи их с узлами `"googleSheetsApi"` и `"googleDriveOAuth2Api"`.

---

## ⚙️ 3. Как это работает

1. **Получение данных**

   * Пользователь отправляет сообщение в Telegram в формате:

     ```
     Название: 3 Dead Tubbies
     Ссылка: https://appwill.co/product/3-deadtubbies/
     Цена: $2,500
     Доход (3 мес): $388
     Установки: 700K+
     Категория: Хоррор-шутер
     Ссылка на геймплей: https://youtu.be/XXXXX
     ```
   * Можно запустить вручную через **Manual Trigger** для теста.

2. **Парсинг и сохранение**

   * Узел **Parse Message** извлекает данные.
   * **Google Sheets Lookup** проверяет, есть ли приложение в таблице.
   * **Update Row** или **Append Row** обновляют/добавляют данные.

3. **Генерация контента**

   * **YouTube API** получает метаданные видео.
   * **GPT-4 Script** — сценарий ролика.
   * **GPT-4 Description** — описание поста.
   * **GPT-4 Hashtags** — хэштеги.

4. **Создание видео**

   * **Prepare Veo3 Assets** формирует данные.
   * **Veo3 Start Job** запускает генерацию.
   * **Veo3 Poll Status** ждёт готовности.
   * **Download Video** скачивает результат.
   * **Upload to Google Drive** заливает файл в Drive.

5. **Публикация**

   * **Instagram Create Container** + **Instagram Publish** — публикация Reels.
   * **TikTok Init Upload** → **TikTok Upload Video** → **TikTok Publish** — публикация в TikTok.

6. **Отчёт**

   * **Update Sheet Status** — статус в Google Sheets.
   * **Telegram Success/Error** — уведомления в Telegram.

---

## 📂 4. Настройка Google Sheets

В первой строке должны быть заголовки:

```
Название | Цена | Доход | Установки | Категория | Ссылка на видео | Статус | ...
```

---

## 🧪 5. Тестирование

1. Запусти workflow через **Manual Trigger**.
2. Убедись, что данные попали в Google Sheets.
3. Проверь в Telegram уведомление.
4. Дождись публикаций в Instagram и TikTok.

---

## 🛠 6. Возможные улучшения

* Добавить **AI-агента** для проверки трендов и адаптации сценария.
* Подключить **автоанализ конверсий** через Google Analytics API.
* Расширить на **YouTube Shorts** и **Facebook Reels**.

---

Если хочешь, я могу сделать для тебя **дополненный AI-вариант workflow** с уже встроенным AI-агентом, который будет сам подбирать маркетинговую стратегию под каждое приложение.
Тогда контент-завод будет работать полностью автономно.

Хочешь, соберём такой?
