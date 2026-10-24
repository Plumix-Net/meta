# NetBench — тексты витрины App Store и Google Play

Актуально на 2026-10-25 (версия 1.0.0). Копирайт витрины App Store и Google Play. Выгружает его fastlane
из `NetBench/fastlane/metadata` — при правке текста меняйте оба места (при расхождении верен репозиторий). Технические ответы
(App Review notes, Data safety, рейтинг) — в `NetBench/store/README.md`.

Лимиты символов указаны в скобках. Конкурентов (k6, wrk, JMeter) в ключевых словах
и описании не упоминаем — Apple отклоняет чужие торговые марки в метаданных.

## Общие поля

| Поле | Значение |
|---|---|
| Bundle ID / applicationId | `net.plumix.netbench` |
| Категория App Store | Developer Tools (основная), Utilities (вторая) |
| Категория Google Play | Tools (приложение) |
| Возраст | 4+ (App Store), Everyone / PEGI 3 (IARC) |
| Цена | бесплатно, без встроенных покупок и рекламы |
| Политика конфиденциальности | https://plumix.net/netbench/privacy |
| Условия использования | https://plumix.net/netbench/terms |
| Сайт / Support URL | https://plumix.net/netbench |
| Маркетинговый URL | https://plumix.net/netbench |
| Контакт | GitHub issues — https://github.com/Plumix-Net/NetBench/issues; **Play требует email разработчика — не выбран** |
| Copyright (App Store) | © 2026 Plumix-Net |

## English (en-US) — основной язык листинга

**App Store name (30):** NetBench – Powered by Plumix
*(в App Store Connect сейчас «Netbench - Powered by Plumix» — бренд пишется с заглавной B)*

**Google Play title (30):** NetBench: HTTP load testing

**App Store subtitle (30):** HTTP load testing for your API

**Google Play short description (80):**
Load test your own HTTP API from your phone: RPS, latency percentiles, errors.

**App Store promotional text (170):**
Point NetBench at your API, pick users and duration, and watch requests per second,
p95 latency and error rate update live. Share the report as HTML, CSV or JSON.

**Keywords — App Store (100, через запятую без пробелов):**
`load test,http,api,benchmark,latency,rps,performance,stress test,rest,server,backend,throughput`

**Description (App Store 4000 / Google Play 4000):**

NetBench is an HTTP load-testing tool for developers who want a quick, honest answer
to "how does my API hold up?" — right from their phone.

Enter a URL, choose how many virtual users to simulate and for how long, and start
the run. NetBench shows live metrics while the test is running:

• Requests per second with a live throughput chart
• p95 latency, updated four times per second
• Error rate and HTTP status code breakdown

When the run finishes you get a full report: average and total requests, latency
percentiles (min, p50, mean, p95, p99, max) and status codes. Share it as a text
summary, an HTML report, a CSV table or JSON data.

Save frequent targets as scenarios with their own method, users and duration, and
keep a history of recent runs on the home screen.

Built for testing your own services:
• Runs from a phone are limited to 50 virtual users and 5 minutes
• Plain http:// is allowed, so you can test local and staging servers
• Nothing leaves your device except the requests you send to your own targets —
  no account, no analytics, no ads, no tracking

Use NetBench only against servers you own or are allowed to test.

NetBench is open source and is a showcase app for Plumix, a Flutter-like UI framework
for .NET: the whole mobile UI is written in C# with Plumix widgets.

**What's new (1.0.0):** см. `NetBench/fastlane/metadata/ios/en-US/release_notes.txt`.

## Русский (ru-RU)

**App Store name (30):** NetBench – Powered by Plumix

**Google Play title (30):** NetBench: нагрузочный тест
*(«…нагрузочный тест HTTP» — 31 символ, не влезает)*

**App Store subtitle (30):** Нагрузочный тест своего API

**Google Play short description (80):**
Нагрузочное тестирование своего HTTP API с телефона: RPS, задержки, ошибки.

**App Store promotional text (170):**
Укажите адрес API, выберите число пользователей и длительность — и смотрите,
как меняются запросы в секунду, p95 задержки и доля ошибок. Отчёт — в HTML, CSV или JSON.

**Keywords — App Store (100):**
`нагрузочный тест,http,api,бенчмарк,задержка,rps,производительность,стресс тест,сервер,бэкенд`

**Description:**

NetBench — инструмент нагрузочного тестирования HTTP для разработчиков, которым нужен
быстрый и честный ответ на вопрос «как держит нагрузку мой API?» — прямо с телефона.

Введите адрес, выберите, сколько виртуальных пользователей имитировать и как долго,
и запустите прогон. Пока он идёт, NetBench показывает метрики в реальном времени:

• Запросы в секунду и живой график пропускной способности
• p95 задержки — обновляется четыре раза в секунду
• Доля ошибок и разбивка по кодам ответа HTTP

По завершении — полный отчёт: средний и общий объём запросов, перцентили задержки
(min, p50, среднее, p95, p99, max) и коды ответов. Отчётом можно поделиться
текстом, HTML-отчётом, таблицей CSV или данными JSON.

Частые цели сохраняются как сценарии со своим методом, числом пользователей
и длительностью, а история последних прогонов — на главном экране.

Для тестирования своих сервисов:
• С телефона — до 50 виртуальных пользователей и 5 минут за прогон
• Можно http:// — удобно для локальных и стейджинговых стендов
• С устройства уходят только запросы к вашим целям — без аккаунта, аналитики,
  рекламы и отслеживания

Запускайте NetBench только против своих серверов или тех, что вам разрешено тестировать.

NetBench — открытое приложение-витрина Plumix, Flutter-подобного UI-фреймворка
для .NET: весь мобильный интерфейс написан на C# виджетами Plumix.

**Что нового (1.0.0):** см. `NetBench/fastlane/metadata/ios/ru/release_notes.txt`.

## Графика

| Что | Где | Статус |
|---|---|---|
| Иконка App Store 1024×1024 | `NetBench/NetBench/NetBench.iOS/Assets.xcassets/AppIcon.appiconset` (из `scripts/generate_icons.py`) | готова |
| Иконка Google Play 512×512 | `NetBench/fastlane/metadata/android/<язык>/images/icon.png` (`scripts/generate_icons.py`) | готова |
| Feature graphic Google Play 1024×500 (ru/en) | `NetBench/fastlane/metadata/android/<язык>/images/featureGraphic.png` (`scripts/generate_store_graphics.py`) | готова |
| Скриншоты iPhone 6.9" (1320×2868), ru/en | `NetBench/fastlane/screenshots/ios/<язык>/` (`fastlane screenshots`) | готовы |
| Скриншоты телефона Google Play (1080×1920), ru/en | `NetBench/fastlane/metadata/android/<язык>/images/phoneScreenshots/` | готовы |
