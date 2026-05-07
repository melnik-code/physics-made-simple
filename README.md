# Физика просто

Сайт-справочник по школьной физике (7–11 классы): темы, термины, формулы, примеры, задачи и мини‑тесты.

Проект сделан на **Next.js (App Router)** и хранит контент в коде (в `lib/physicsContent.js`).

## Быстрый старт

Требования:

- Node.js (рекомендуется LTS)
- npm

Установка и запуск:

```bash
npm install
npm run dev
```

Открыть в браузере:

- http://localhost:3000

Сборка и запуск production:

```bash
npm run build
npm run start
```

## Структура проекта

- **`app/`** — страницы (App Router) и системные роуты
  - **`app/page.js`** — главная
  - **`app/topics/...`** — дерево страниц по темам: раздел → группа → тема
  - **`app/formulas/page.js`** — справочник формул
  - **`app/terms/page.js`** — словарь терминов
  - **`app/exam/page.js`** — план повторения перед контрольной/ОГЭ/ЕГЭ
  - **`app/scientists/page.js`** — страница об учёных
  - **`app/about/page.js`** — о проекте
  - **`app/contacts/page.js`** — контакты
  - **`app/sitemap.js`** — генерация `sitemap.xml` на основе контента
  - **`app/robots.js`** — генерация `robots.txt`
  - **`app/opengraph-image.js`**, **`app/twitter-image.js`** — OG/Twitter изображения
  - **`app/layout.js`** — общий layout, метаданные и подключение аналитики

- **`components/`** — UI-компоненты
  - `Header`, `Footer`, `Section`, `Card`, `Button`
  - `TestBlock` — интерактивный мини‑тест (client component)
  - `FormulaBrowser`, `TermBrowser` — фильтрация справочников
  - `StructuredData` — вывод JSON‑LD в `<script type="application/ld+json">`

- **`lib/`** — данные и вспомогательная логика
  - **`physicsContent.js`** — основной контент: разделы/темы/термины/формулы/тесты
  - `structuredData.js` — генераторы schema.org (WebSite, FAQ, LearningResource и др.)
  - `breadcrumbsSchema.js` — JSON‑LD для хлебных крошек
  - `site.js` — определение базового URL (для canonical, sitemap, JSON‑LD)
  - `seoDefaults.js` — базовые ключевые слова

- **`public/`** — статические файлы (иконки, изображения, и т.д.)

## Где менять контент

Основной контент находится в файле:

- `lib/physicsContent.js`

Там описаны:

- разделы (`sections`)
- группы тем (`groups`)
- темы (`topics`) с полями `theory`, `terms`, `formulas`, `example`, `practice`, `test`, `mistakes`
- справочные наборы (формулы/термины) для страниц `/formulas` и `/terms`

После изменения контента достаточно перезапустить dev‑сервер (обычно Next подхватывает изменения автоматически).

## Маршруты

Основные страницы:

- `/` — главная
- `/topics` — список разделов
- `/topics/[section]` — раздел
- `/topics/[section]/[group]` — группа тем
- `/topics/[section]/[group]/[topic]` — тема
- `/formulas` — справочник формул
- `/terms` — словарь терминов
- `/exam` — подготовка к экзамену
- `/scientists` — учёные
- `/about` — о проекте
- `/contacts` — контакты

## SEO и structured data

В проекте используются:

- **Next.js Metadata API** (`export const metadata` / `generateMetadata`) для title/description/open graph/canonical.
- **JSON‑LD (schema.org)** через компонент `components/StructuredData`.

Схемы:

- `lib/structuredData.js`
  - `getSiteWideJsonLdGraph()` — общие сущности WebSite/Organization (в `app/layout.js`)
  - `getHomeWebPageJsonLd()` — главная
  - `getFaqPageJsonLd()` — FAQ schema
  - `getLearningResourceJsonLd()` — страницы тем
  - `getContactPageJsonLd()` — контакты

Хлебные крошки:

- `lib/breadcrumbsSchema.js` + использование на страницах.

## Переменные окружения

Проект может использовать следующие переменные:

- `NEXT_PUBLIC_SITE_URL` или `SITE_URL`
  - Базовый URL сайта. Используется для canonical, sitemap и JSON‑LD.
  - Пример: `https://example.com`
- `VERCEL_URL`
  - Автоматически выставляется на Vercel (используется как fallback в `lib/site.js`).
- `NEXT_PUBLIC_OG_IMAGE`
  - Полный URL до OG‑картинки (если задан, добавляется в OpenGraph/Twitter метаданные layout).

Локально можно создать файл `.env.local`:

```bash
NEXT_PUBLIC_SITE_URL=http://localhost:3000
```

## Аналитика

В `app/layout.js` подключены:

- `@vercel/analytics`
- `@vercel/speed-insights`

Они удобны при деплое на Vercel, но не мешают локальной разработке.

## Конфиги разработки

- `jsconfig.json` настроен для алиаса импортов `@/*`.
- `.gitignore` игнорирует `.next/`, `node_modules/`, `.idea/`, `.DS_Store`, `.env*`.

## Деплой

Самый простой вариант — **Vercel**:

- подключить репозиторий
- собрать проект как Next.js
- (опционально) задать `NEXT_PUBLIC_SITE_URL` доменом продакшн‑сайта

## Лицензия

См. файл `LICENSE`.
